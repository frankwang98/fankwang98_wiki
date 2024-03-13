







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍websocketpp的安装与使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. websocket介绍](#smirk1_websocket_7)
	+ [:blush:2. websocketpp安装](#blush2_websocketpp_30)
	+ [:satisfied:3. websocketpp使用](#satisfied3_websocketpp_55)




### 😏1. websocket介绍


`WebSocket` 是一种在单个 TCP 连接上进行全双工通信的协议，它使得客户端和服务器之间的数据交流变得更加实时、高效。相比传统的 HTTP 请求-响应机制，WebSocket 直接建立连接，并通过数据帧（`Data Frame`）来交换消息，从而避免了每次通信都要建立、断开连接的开销。


WebSocket 协议最早由 HTML5 规范提出，它可以用于 Web 应用程序、移动应用程序等不同类型的应用场景中。WebSocket 除了支持文本格式的消息外，还可以使用二进制数据格式发送数据，这使得 WebSocket 可以处理包括音频、视频等复杂类型的数据。


`WebSocket++`是一个C++编写的开源库，用于在Web应用程序中实现WebSocket协议的客户端和服务器端。


以下是WebSocket++的主要特点和功能：



> 
> 1. 遵循WebSocket协议：WebSocket++完全符合WebSocket协议标准（RFC 6455），支持基于TCP的双向通信，可以在客户端和服务器之间实时传输数据。
> 
> 
> 



> 
> 2. 跨平台支持：WebSocket++可以在多种操作系统和平台上运行，包括Linux、Windows和MacOS等。它依赖于标准的C++库，因此可以很容易地移植到不同的环境中。
> 
> 
> 



> 
> 3. 简单易用：WebSocket++提供了简洁而直观的API，使开发人员能够轻松地创建和管理WebSocket连接。它封装了底层的网络细节，提供了高级抽象，使开发人员能够专注于业务逻辑的实现。
> 
> 
> 



> 
> 4. 灵活性和可扩展性：WebSocket++允许开发人员自定义扩展和插件，以满足特定的需求。它提供了丰富的钩子函数和事件处理机制，使开发人员能够自由地扩展和定制库的功能。
> 
> 
> 



> 
> 5. 支持异步IO和多线程：WebSocket++支持异步IO模型，可以处理大量并发连接，提供高性能的实时通信。它还支持多线程处理，可以充分利用多核CPU的优势。
> 
> 
> 



> 
> 6. SSL/TLS支持：WebSocket++提供了对SSL/TLS加密的支持，可以确保WebSocket连接的安全性。开发人员可以使用TLS/SSL证书和配置，进行加密通信。
> 
> 
> 



> 
> 7. 扩展和子协议支持：WebSocket++支持WebSocket协议的扩展和子协议。开发人员可以自定义和实现自己的扩展和子协议，以满足特定的应用需求。
> 
> 
> 


### 😊2. websocketpp安装


以ubuntu18.04为例：


websocketpp库依赖`boost_system`，因此首先安装boost库：



```
# apt安装
sudo apt-get install libboost-dev

```

安装websocketpp库（这里用0.8.2版本）：


github地址：`https://github.com/zaphoyd/websocketpp`



```
# 编译安装
cd websocketcpp
mkdir build && cd build
cmake ..
make
sudo make install

```

安装完成：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2b95a5e8471e4e84855c680dd1d507c4.png)


### 😆3. websocketpp使用


通信例程测试：



```
# 服务端
cd websocketpp/examples/echo_server
g++ echo_server.cpp -o echo_server -lboost\_system -lpthread
./echo_server
# 客户端
cd websocketpp/examples/echo_client
g++ echo_client.cpp -o echo_client -lboost\_system -lpthread
./echo_client
# 默认通信在本地的9002端口

```

在线websocket收发测试：


测试地址：`http://www.websocket-test.com/`


测试如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/73a4f14de0e8438b919e35ebdf8d4bbf.png)


端口重用问题可以看：`http://t.csdn.cn/Z7AyU`（一般关掉服务后直接重启会报错，等一会就可以）


根本解决就是在listen函数前加入这条，这样在Linux端重启程序后就不会报错了：



```
server.set\_reuse\_addr(true);	// 加入端口复用
server.listen(websocketpp::lib::asio::ip::tcp::v4(), uPort);

```

参考：



```
https://zhuanlan.zhihu.com/p/59925926
http://t.csdn.cn/CGAFL

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





