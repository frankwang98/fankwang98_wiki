







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Asio网络库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__38)
	+ [:satisfied:3. 使用说明](#satisfied3__46)
	+ - [http](#http_49)
		- [TCP](#TCP_160)
		- [UDP](#UDP_259)
		- [websocket](#websocket_334)
		- [websocket2](#websocket2_462)




### 😏1. 项目介绍


项目Github地址：`https://github.com/boostorg/asio`


`Boost.Asio`是一个用于网络和底层I/O编程的C++库，它提供了一种简洁而高效的方式来处理异步事件驱动的网络编程。Asio是"`异步 I/O`"的缩写。


下面是一些关于`Boost.Asio`的特点和功能的介绍：



> 
> 1.异步模型：Boost.Asio使用异步编程模型，允许你以非阻塞的方式处理多个并发的I/O操作。这样可以提高程序的性能和响应能力。
> 
> 
> 



> 
> 2.跨平台性：Boost.Asio在不同操作系统上提供统一的API，使得你可以在多个平台上轻松开发和移植网络应用程序。
> 
> 
> 



> 
> 3.支持多种协议：Boost.Asio支持多种网络协议，包括TCP、UDP、SSL等，让你能够轻松地进行各种网络通信。
> 
> 
> 



> 
> 4.网络编程基础功能：Boost.Asio提供了一系列的类和函数，用于处理套接字、地址解析、定时器、缓冲区等常见的网络编程任务。
> 
> 
> 



> 
> 5.可扩展性：Boost.Asio提供了灵活的接口和设计，允许你根据需要对其进行扩展和定制，以满足特定的应用需求。
> 
> 
> 



> 
> 6.高性能：Boost.Asio通过使用异步I/O、事件驱动和零拷贝等技术，可以实现高效的网络编程，提供出色的性能。
> 
> 
> 


`Boost.Asio`是一个功能强大而灵活的库，它被广泛应用于构建各种类型的网络应用程序，包括Web服务器、游戏服务器、实时通信系统等。它不仅提供了一种简单易用的方式来处理网络编程任务，还允许你利用C++的强大功能来开发高性能和可扩展的应用程序。


此外，`Boost`中网络相关的库还包括：



> 
> 1.Boost.Asio：Boost.Asio 是一个跨平台的网络编程库，提供了异步 I/O 操作和网络编程的基本功能，支持 TCP、UDP、串口、定时器等。它是 Boost 网络编程的核心库，也是其他 Boost 网络库的基础。
> 
> 
> 



> 
> 2.Boost.Beast：Boost.Beast 是一个基于 Boost.Asio 的 HTTP 和 WebSocket 协议库。它提供了一个高性能、易于使用的 API，用于构建和处理 HTTP 请求和响应，以及实现 WebSocket 通信。
> 
> 
> 



> 
> 3.Boost.Asio SSL：Boost.Asio SSL 提供了对 SSL/TLS 安全传输协议的支持，用于在 Boost.Asio 中进行安全的网络通信。
> 
> 
> 



> 
> 4.Boost.Asio IPC：Boost.Asio IPC 提供了在本地进程间进行通信的功能，包括命名管道、共享内存、信号量等。
> 
> 
> 



> 
> 5.Boost.Asio Coroutine：Boost.Asio Coroutine 是一个用于在异步网络编程中使用协程的库。它结合了 Boost.Asio 和 Boost.Coroutine，使得编写异步代码更加简洁和易读。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt-get install libboost-dev libasio-dev

```

### 😆3. 使用说明


下面进行使用分析：


#### http


http服务端示例：



```
#include <boost/beast.hpp>
#include <boost/asio.hpp>
#include <iostream>

namespace beast = boost::beast;
namespace http = beast::http;
namespace net = boost::asio;
using tcp = net::ip::tcp;

void handle\_request(http::request<http::string_body>& request, http::response<http::string_body>& response) {
    // 处理请求并生成响应
    response.version(request.version());
    response.result(http::status::ok);
    response.set(http::field::server, "Boost Beast HTTP Server");
    response.body() = "Hello, World!";
    response.prepare\_payload();
}

int main() {
    try {
        // 创建 IO 上下文和解析器
        net::io_context io_context;

        // 创建监听端口
        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), 8881));

        while (true) {
            // 接受连接
            tcp::socket socket(io_context);
            acceptor.accept(socket);

            // 读取请求
            beast::flat_buffer buffer;
            http::request<http::string_body> request;
            http::read(socket, buffer, request);

            // 处理请求并生成响应
            http::response<http::string_body> response;
            handle\_request(request, response);

            // 发送响应
            http::write(socket, response);
        }
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}

```

http客户端示例：



```
#include <boost/beast.hpp>
#include <boost/asio.hpp>
#include <iostream>

namespace beast = boost::beast;
namespace http = beast::http;
namespace net = boost::asio;
using tcp = net::ip::tcp;

int main() {
    try {
        // 创建 IO 上下文和解析器
        net::io_context io_context;
        tcp::resolver resolver(io_context);
        beast::tcp_stream stream(io_context);

        // 解析主机和端口
        auto const results = resolver.resolve("localhost", "8881");

        // 连接到服务器
        stream.connect(results);

        // 创建 HTTP 请求
        http::request<http::string_body> request(http::verb::get, "/", 11);
        request.set(http::field::host, "localhost");
        request.set(http::field::user_agent, "Boost Beast");

        // 发送请求
        http::write(stream, request);

        // 接收并打印响应
        beast::flat_buffer buffer;
        http::response<http::string_body> response;
        http::read(stream, buffer, response);

        std::cout << "Response code: " << response.result\_int() << std::endl;
        std::cout << "Response body: " << response.body() << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}

```

编译运行：



```
g++ -o server server.cpp -lboost_system -lboost_thread -lpthread
g++ -o client client.cpp -lboost_system -lboost_thread -lpthread

```

#### TCP


TCP服务端示例：



```
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

int main()
{
    try
    {
        boost::asio::io_context io_context;

        // 创建一个 TCP acceptor，监听指定端口
        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), 8081));

        // 等待并接受连接
        tcp::socket socket(io_context);
        acceptor.accept(socket);

        // 接收客户端的消息
        char response[1024];
        size_t bytesRead = socket.read\_some(boost::asio::buffer(response));

        std::cout << "Response from client: ";
        std::cout.write(response, bytesRead);

        // 处理连接请求
        std::string message = "Hello, Boost.Asio!";
        boost::system::error_code ignored_error;
        boost::asio::write(socket, boost::asio::buffer(message), ignored_error);
    }
    catch (std::exception& e)
    {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

编译运行：



```
g++ -o server server.cpp -lboost_system -lpthread
./server

```

TCP客户端示例：



```
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

int main() {
  try {
    // 创建IO上下文对象
    boost::asio::io_context io_context;

    // 创建socket对象
    tcp::socket socket(io_context);

    // 解析服务器地址和端口
    tcp::resolver resolver(io_context);
    tcp::resolver::results_type endpoints = resolver.resolve("127.0.0.1", "8081");

    // 连接到服务器
    boost::asio::connect(socket, endpoints);

    // 发送数据给服务器
    std::string request = "Hello, server!";
    boost::asio::write(socket, boost::asio::buffer(request));

    // 接收服务器的响应
    char response[1024];
    size_t bytesRead = socket.read\_some(boost::asio::buffer(response));

    // 显示服务器的响应
    std::cout << "Response from server: ";
    std::cout.write(response, bytesRead);
    std::cout << std::endl;

  } catch (std::exception& e) {
    std::cerr << "Exception: " << e.what() << std::endl;
  }

  return 0;
}

```

编译运行：



```
g++ -o client client.cpp -lboost_system -lpthread
./client

```

#### UDP


UDP服务端示例：



```
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::udp;

int main()
{
    boost::asio::io_context io_context;

    // 创建UDP端点并绑定到特定端口
    udp::socket socket(io_context, udp::endpoint(udp::v4(), 8888));

    // 接收缓冲区
    char recv_buffer[1024];

    while (true) {
        udp::endpoint remote_endpoint;

        // 接收数据
        size_t bytes_received = socket.receive\_from(boost::asio::buffer(recv_buffer), remote_endpoint);

        // 打印接收到的数据
        std::cout.write(recv_buffer, bytes_received);
        std::cout << std::endl;

        // 发送回复
        std::string message = "Hello from server!";
        socket.send\_to(boost::asio::buffer(message), remote_endpoint);
    }

    return 0;
}

```

UDP客户端示例：



```
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::udp;

int main()
{
    boost::asio::io_context io_context;

    // 创建UDP端点并绑定到任意端口
    udp::socket socket(io_context, udp::endpoint(udp::v4(), 0));

    // 远程服务器端点
    udp::endpoint remote\_endpoint(boost::asio::ip::address::from\_string("127.0.0.1"), 8888);

    // 发送数据
    std::string message = "Hello from client!";
    socket.send\_to(boost::asio::buffer(message), remote_endpoint);

    // 接收缓冲区
    char recv_buffer[1024];

    // 接收回复
    udp::endpoint sender_endpoint;
    size_t bytes_received = socket.receive\_from(boost::asio::buffer(recv_buffer), sender_endpoint);

    // 打印回复数据
    std::cout.write(recv_buffer, bytes_received);
    std::cout << std::endl;

    return 0;
}

```

#### websocket


websocket服务端示例：



```
#include <iostream>
#include <string>
#include <boost/asio.hpp>
#include <boost/beast.hpp>
#include <thread>

namespace asio = boost::asio;
namespace beast = boost::beast;
using tcp = asio::ip::tcp;

void SendMessages(beast::websocket::stream<tcp::socket>& ws) {
    try {
        // Send a message every 1 seconds
        while (true) {
            std::this_thread::sleep\_for(std::chrono::seconds(1));
            ws.write(asio::buffer("Server: Sending a message from the server!"));
        }
    } catch (const std::exception &e) {
        std::cerr << "Exception in SendMessages: " << e.what() << std::endl;
    }
}

int main() {
    try {
        asio::io_context io_context;

        // Create and bind the acceptor
        tcp::acceptor acceptor(io_context, {tcp::v4(), 8881});

        while (true) {
            // Accept connection
            tcp::socket socket(io_context);
            acceptor.accept(socket);

            // WebSocket upgrade
            beast::websocket::stream<tcp::socket> ws(std::move(socket));
            ws.accept();

            // Start a thread to send messages to the client
            std::thread senderThread(SendMessages, std::ref(ws));

            // Read WebSocket message
            beast::flat_buffer buffer;
            ws.read(buffer);

            // Print received message
            std::cout << "Received: " << beast::buffers\_to\_string(buffer.data()) << std::endl;

            // Echo the message back to the client
            ws.text(ws.got\_text());
            ws.write(buffer.data());

            // Join the sender thread
            senderThread.join();
        }
    } catch (const std::exception &e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

websocket客户端示例：



```
#include <iostream>
#include <string>
#include <boost/asio.hpp>
#include <boost/beast.hpp>

namespace asio = boost::asio;
namespace beast = boost::beast;
using tcp = asio::ip::tcp;

int main() {
    try {
        asio::io_context io_context;

        // Resolve the host name
        tcp::resolver resolver(io_context);
        auto const results = resolver.resolve("localhost", "8881");

        // Connect to the server
        tcp::socket socket(io_context);
        asio::connect(socket, results);

        // WebSocket upgrade
        beast::websocket::stream<tcp::socket> ws(std::move(socket));
        ws.handshake("localhost", "/");

        // Receive and print the initial message from the server
        beast::flat_buffer buffer;
        ws.read(buffer);
        std::cout << "Received: " << beast::buffers\_to\_string(buffer.data()) << std::endl;

        // Start a thread to continuously receive messages from the server
        std::thread receiverThread([&ws] {
            try {
                while (true) {
                    beast::flat_buffer buffer;
                    ws.read(buffer);
                    std::cout << "Received: " << beast::buffers\_to\_string(buffer.data()) << std::endl;
                }
            } catch (const std::exception &e) {
                std::cerr << "Exception in receiverThread: " << e.what() << std::endl;
            }
        });

        // Send a WebSocket message
        ws.text(true);
        ws.write(asio::buffer("Client: Hello, Server!"));

        // Join the receiver thread
        receiverThread.join();

        // Close the WebSocket connection
        ws.close(beast::websocket::close_code::normal);
    } catch (const std::exception &e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

#### websocket2


服务端示例：



```
#include <iostream>
#include <string>
#include <boost/beast/core.hpp>
#include <boost/beast/websocket.hpp>
#include <boost/asio/ip/tcp.hpp>
#include <boost/asio/io\_context.hpp>

namespace beast = boost::beast;
namespace websocket = beast::websocket;
using tcp = boost::asio::ip::tcp;

void runWebSocketServer(unsigned short port) {
    try {
        boost::asio::io_context io_context;

        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), port));
        tcp::socket socket(io_context);
        acceptor.accept(socket);

        websocket::stream<tcp::socket> ws(std::move(socket));
        ws.accept();

        while (true) {
            beast::flat_buffer buffer;
            ws.read(buffer);

            std::string message(beast::buffers\_to\_string(buffer.data()));
            std::cout << "Received message: " << message << " port: " << port << std::endl;

            ws.text(ws.got\_text());
            ws.write(boost::asio::buffer("Response: " + message));
        }
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
}

int main() {
    std::cout << "WebSocket server is running..." << std::endl;

    unsigned short port = 9002;
    runWebSocketServer(port);

    // 可在多线程中开启多个端口
    // unsigned short port2 = 9003;
    // runWebSocketServer(port2);

    return 0;
}

```

客户端示例：



```
#include <iostream>
#include <chrono>
#include <thread>
#include <boost/beast/core.hpp>
#include <boost/beast/websocket.hpp>
#include <boost/asio/ip/tcp.hpp>
#include <boost/asio/connect.hpp>
#include <boost/asio/io\_context.hpp>

namespace beast = boost::beast;
namespace websocket = beast::websocket;
using tcp = boost::asio::ip::tcp;

void runWebSocketClient(const std::string& serverAddress, unsigned short port) {
    try {
        boost::asio::io_context io_context;
        tcp::resolver resolver(io_context);
        websocket::stream<tcp::socket> ws(io_context);

        auto const results = resolver.resolve(serverAddress, std::to\_string(port));
        boost::asio::connect(ws.next\_layer(), results.begin(), results.end());
        ws.handshake(serverAddress, "/");

        while (true) {
            std::string message = "Hello, WebSocket!";
            ws.write(boost::asio::buffer(message));

            beast::flat_buffer buffer;
            ws.read(buffer);
            std::cout << "Server response: " << beast::buffers\_to\_string(buffer.data()) << " port: " << port << std::endl;

            std::this_thread::sleep\_for(std::chrono::seconds(1));
        }

        // ws.close(websocket::close\_code::normal);
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
}

int main() {
    std::cout << "WebSocket client example" << std::endl;

    std::string serverAddress = "localhost";
    unsigned short port = 9002;
    runWebSocketClient(serverAddress, port);

    // 可在多线程中开启多个端口
    // unsigned short port2 = 9003;
    // runWebSocketClient(serverAddress, port2);

    return 0;
}

```

编译：



```
g++ server.cpp -o server -lboost_system -lboost_thread -lpthread
g++ client.cpp -o client -lboost_system -lboost_thread -lpthread

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





