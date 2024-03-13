







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Muduo网络库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__31)
	+ [:satisfied:3. 使用说明](#satisfied3__52)




### 😏1. 项目介绍


项目Github地址：`https://github.com/chenshuo/muduo`


Muduo 是一个基于 C++11 的网络库，用于构建高性能的多线程服务器和应用程序。它由陈硕开发，并且致力于提供简单、可靠和高效的网络编程解决方案。


以下是 Muduo 的主要特点和功能：



> 
> 1.事件驱动：Muduo 使用事件驱动的方式处理网络通信，通过监听事件并相应地调用相应的回调函数来实现异步非阻塞的网络操作。
> 
> 
> 



> 
> 2.多线程支持：Muduo 使用多线程模型，可以通过创建多个线程来处理客户端请求，实现并发处理。
> 
> 
> 



> 
> 3.高性能：Muduo 使用了多种优化技术，如使用线程池、非阻塞 I/O 和事件分发等，以提高服务器的并发处理能力和响应速度。
> 
> 
> 



> 
> 4.TCP/IP 支持：Muduo 提供了对 TCP/IP 协议的支持，可以方便地进行网络通信。它提供了 TCP 客户端和服务器端的 API，以及常用的网络编程组件，如套接字、缓冲区等。
> 
> 
> 



> 
> 5.定时器：Muduo 提供了定时器功能，可以用于处理定时任务，执行周期性的操作，或者延迟执行某些任务。
> 
> 
> 



> 
> 6.异步日志：Muduo 内置了高性能的异步日志系统，可以方便地记录服务器运行过程中的日志信息，帮助开发者进行调试和故障排查。
> 
> 
> 



> 
> 7.线程同步：Muduo 提供了一些线程同步的原语，如互斥锁、条件变量等，用于保护共享资源的访问。
> 
> 
> 



> 
> 8.跨平台支持：Muduo 可以在多个主流操作系统上运行，包括 Linux、macOS 和 Windows 等。
> 
> 
> 


Muduo 的设计目标是提供简洁而高效的c++网络编程框架，使开发者可以专注于业务逻辑的实现，而无需过多关注底层细节。它被广泛应用于构建服务器程序、网络应用和分布式系统。


### 😊2. 环境配置


下面进行环境配置：



```
# 安装依赖项
sudo apt-get install -y g++ cmake libboost-all-dev
# 源码编译
git clone https://ghproxy.com/https://github.com/chenshuo/muduo.git
cd muduo
./build.sh
# 将库和头文件添加到系统目录
cd /build/release-install-cpp11/include
mv muduo/ /usr/include/
cd ../lib/
mv * /usr/local/lib/
# 验证安装
find /usr/include/muduo 
# 问题
Muduo 目前仅支持 Protobuf 2.6.x 版本，并不直接支持 Protobuf 3

```

### 😆3. 使用说明


下面进行使用分析：


一个简单的例子-简单 Echo 服务器：



```
#include <muduo/net/TcpServer.h>
#include <muduo/net/EventLoop.h>
#include <muduo/net/InetAddress.h>
#include <muduo/base/Logging.h>

using namespace muduo;
using namespace muduo::net;

void onConnection(const TcpConnectionPtr& conn)
{
    if (conn->connected())
    {
        LOG_INFO << "New connection: " << conn->peerAddress().toIpPort();
    }
    else
    {
        LOG_INFO << "Connection closed: " << conn->peerAddress().toIpPort();
    }
}

void onMessage(const TcpConnectionPtr& conn, Buffer\* buf, Timestamp time)
{
    std::string msg(buf->retrieveAllAsString());
    LOG_INFO << "Received " << msg.size() << " bytes from " << conn->peerAddress().toIpPort();
    conn->send(msg);
}

int main()
{
    LOG_INFO << "Server started.";

    EventLoop loop;
    InetAddress listenAddr(8888);
    TcpServer server(&loop, listenAddr, "EchoServer");

    server.setConnectionCallback(onConnection);
    server.setMessageCallback(onMessage);

    server.start();

    loop.loop();

    return 0;
}

```

编译运行：



```
g++ -o server server.cpp -lmuduo\_net -lmuduo\_base -lpthread
./server

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





