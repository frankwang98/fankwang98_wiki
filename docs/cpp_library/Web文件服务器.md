







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍开源项目——Web文件服务器。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 安装运行](#blush2__24)
	+ [:satisfied:3. 源码分析](#satisfied3__32)




### 😏1. 项目介绍


项目Github地址：`https://github.com/shangguanyongshi/WebFileServer`


WebFileServer是一个基于Web的文件服务器，它提供了通过浏览器访问和管理文件的功能。使用WebFileServer，你可以在任何设备上通过网络轻松上传、下载、查看和管理文件。


该项目的功能特点有：



> 
> 1.文件上传和下载：WebFileServer允许用户通过浏览器上传文件到服务器，并从服务器上下载文件到本地设备。这使得文件传输变得简单方便，不需要额外的FTP客户端或其他工具。
> 
> 
> 



> 
> 2.文件管理：WebFileServer提供了文件和文件夹的管理功能，你可以创建、删除文件和文件夹。通过简单的操作，你可以组织和管理服务器上的文件结构。
> 
> 
> 



> 
> 3.多用户支持：WebFileServer支持多个用户账户，并允许为每个用户配置不同的权限和访问级别。这样，你可以控制用户对文件的访问和操作权限，确保文件的安全性和隐私性。
> 
> 
> 



> 
> 4.便捷的界面：WebFileServer提供了一个用户友好的Web界面，使得文件的浏览和操作变得直观和易于使用。你可以在浏览器中通过简单的点击和拖放完成文件操作。
> 
> 
> 



> 
> 5.安全性和权限控制：WebFileServer支持基本的安全认证和权限控制机制，保护服务器上的文件免受未经授权的访问。你可以设置用户的登录凭据，并为每个用户分配不同的访问权限。
> 
> 
> 


简单来说，就是通过http协议实现文件上传、查看和下载、删除操作。


### 😊2. 安装运行


编译运行：



```
make && ./main
# 在浏览器输入127.0.0.1:8888即可访问
# 默认ip和端口是这个，可以修改，部署在服务器上做个简单的文件存储

```

### 😆3. 源码分析


下面进行源码分析：



> 
> 使用 Reactor 事件处理模型，通过统一事件源，主线程使用 epoll 监听所有的事件，工作线程负责执行事件的逻辑处理  
>  预先创建线程池，当有事件发生时，加入线程池的工作队列中，使用随机选择算法选择线程池中的一个线程处理工作队列的事件  
>  使用 HTTP GET 方法获取文件列表，发起下载文件、删除文件的请求。使用 POST 方法向服务器上传文件  
>  服务端使用有限状态机对请求消息进行解析，根据解析结果执行操作后，向客户端发送页面、发送文件或发送重定向报文  
>  服务端使用 sendfile 函数实现零拷贝数据发送
> 
> 
> 


`filedir`是文件存储的目录。


main.cpp



```
// 作者已经备注很详细了，即创建线程池后，监听客户端的事件请求并处理数据
#include "./fileserver/fileserver.h"

int main(void){

    WebServer webserver;

    // 创建线程池
    int ret = webserver.createThreadPool(4);
    if(ret != 0){
        std::cout << outHead("error") << "创建线程池失败" << std::endl;
        return -1;
    }

    // 初始化用于监听的套接字
    int port = 8888;
    ret = webserver.createListenFd(port);
    if(ret != 0){
        std::cout << outHead("error") << "创建并初始化监听套接字失败" << std::endl;
        return -2;
    }

    // 初始化监听的epoll例程
    ret = webserver.createEpoll();
    if(ret != 0){
        std::cout << outHead("error") << "初始化监听的epoll例程失败" << std::endl;
        return -3;
    }
    // 向 epoll 中添加监听套接字
    ret = webserver.epollAddListenFd();
    if(ret != 0){
        std::cout << outHead("error") << "epoll 添加监听套接字失败" << std::endl;
        return -4;
    }

    // 开启监听并处理请求
    ret = webserver.waitEpoll();
    if(ret != 0){
        std::cout << outHead("error") << "epoll 例程监听失败" << std::endl;
        return -5;
    }
    
    return 0;
}

```

threadpoll：



```
// 线程池事件处理
// 向事件队列中添加一个待处理的事件，线程池中的线程会循环处理其中的事件
int appendEvent(EventBase\* event, const std::string eventType);

```

event：



```
// 事件对象处理
// 事件基类
class EventBase{
public:
    EventBase(){
    }
    virtual ~EventBase(){
    }
protected:
    // 保存文件描述符对应的请求的状态，因为一个连接上的数据可能非阻塞一次读取不完，所以保存到这里，当该连接上有新数据时，可以继续读取并处理
    static std::unordered_map<int, Request> requestStatus;

    // 保存文件描述符对应的发送数据的状态，一次proces中非阻塞的写数据可能无法将数据全部传过去，所以保存当前数据发送的状态，可以继续传递数据
    static std::unordered_map<int, Response> responseStatus;

public:
    // 不同类型事件中重写该函数，执行不同的处理方法
    virtual void process(){
    }
};
// 接收客户端连接的事件
// 处理客户端发送的请求
// 处理向客户端发送数据

```

message：



```
// 请求消息和响应消息

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





