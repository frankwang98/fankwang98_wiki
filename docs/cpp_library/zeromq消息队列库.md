> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ZeroMQ的使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. ZMQ介绍

官网：`https://zeromq.org/`

Github：`https://github.com/zeromq/libzmq`

`ZMQ`（`ZeroMQ`）是一种高性能的异步消息传递库，它可以在不同的进程和机器之间进行消息传递。它提供了多种传输协议、通信模式和编程语言支持，并且非常易于使用。

`ZMQ` 的核心思想是将网络通信抽象出来成为 `socket` 概念，使用不同类型的 socket 可以实现不同的消息传递模式，例如**请求-应答模式、发布-订阅模式、推送-拉取模式**等。`ZMQ` 提供了 TCP、IPC、inproc 等多种传输协议，可以根据需要选择合适的协议。

`ZMQ` 还提供了众多编程语言的封装，包括 `C、C++、Python、Java` 等，使得开发者可以方便地在各种平台上进行开发，并且具有很好的可扩展性和高效性。

总的来说，`ZMQ` 是一个轻量级、高效、灵活的消息传递库，适用于分布式系统、并发处理、网络爬虫等场景。

### 😊2. ZMQ安装

#### 源码安装

```bash
sudo apt-get install libtool pkg-config build-essential autoconf automake
# 安装Sodium加密库
git clone https://github.com/jedisct1/libsodium.git
cd libsodium
./autogen.sh -s
./configure 
make check
sudo make install 
sudo ldconfig 
# 编译安装ZMQ核心库（ZMQ的核心库和C/C++依赖是分开的。）
git clone https://github.com/zeromq/libzmq
./autogen.sh
./configure
make check
sudo make install
sudo ldconfig
# 编译安装ZMQ的C依赖
git clone https://github.com/zeromq/czmq.git
cd czmq
./autogen.sh
./configure
make check
sudo make install
sudo ldconfig
编译方式：`gcc -lczmq -lzmq main.c -o main`
# 添加ZMQ的C++依赖，将头文件添加到系统目录即可
git clone https://github.com/zeromq/cppzmq.git
cd cppzmq
sudo cp zmq.hpp /usr/local/include/

```

#### apt安装

```bash
# 这种方式简单方便
sudo apt install libzmq3-dev libzmqpp-dev
# g++编译
g++ -o main main.cpp -lzmq

```

### 😆3. ZMQ入门案例

#### 官方示例

```bash
git clone https://github.com/imatix/zguide.git
# C
cd zguide/examples/C
./build all
./hwserver
./hwclient
# C++
./build all
./hwserver
./hwclient

```

ZMQ支持多种模式和多种协议，常用的ZeroMQ URL格式如下：

```bash
TCP: "tcp://<address>:<port>"(使用TCP协议)
in-process: "inproc://<name>"(进程内通信)
inter-process: "ipc://<path>" (Unix系统) 或 "ipc://<name>" (Windows系统)(进程间通信)
多播: "epgm://<address>:<port>" (使用PGM协议) 或 "epub://<address>:<port>" (使用UDP协议)

```

#### 请求-应答模式

server.cpp

```cpp
#include <zmq.hpp>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::rep);

    // 绑定到指定地址
    socket.bind("tcp://*:5555");

    while (true) {
        // 接收请求
        zmq::message_t request;
        socket.recv(request, zmq::recv_flags::none);

        // 打印请求内容
        std::cout << "Received request: " << std::string(static_cast<char*>(request.data()), request.size()) << std::endl;

        // 发送响应
        zmq::message_t response(5);
        memcpy(response.data(), "World", 5);
        socket.send(response, zmq::send_flags::none);
    }

    return 0;
}

```

client.cpp

```cpp
#include <zmq.hpp>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::req);

    // 连接到服务端地址
    socket.connect("tcp://localhost:5555");

    // 发送请求
    zmq::message_t request(5);
    memcpy(request.data(), "Hello", 5);
    socket.send(request, zmq::send_flags::none);

    // 接收响应
    zmq::message_t response;
    socket.recv(response, zmq::recv_flags::none);

    // 打印响应
    std::cout << "Received response: " << std::string(static_cast<char*>(response.data()), response.size()) << std::endl;

    return 0;
}

```

#### 发布-订阅模式

publisher.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>
#include <unistd.h>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::pub);

    // 绑定到指定地址
    socket.bind("tcp://*:5555");

    // 发布消息
    int count = 0;
    while (true) {
        std::string topic = "Topic";
        std::string message = "Message " + std::to_string(count);

        // 发布主题和消息
        zmq::message_t topicMsg(topic.size());
        memcpy(topicMsg.data(), topic.data(), topic.size());
        socket.send(topicMsg, zmq::send_flags::sndmore);

        zmq::message_t messageMsg(message.size());
        memcpy(messageMsg.data(), message.data(), message.size());
        socket.send(messageMsg, zmq::send_flags::none);

        std::cout << "Published: " << topic << " - " << message << std::endl;

        count++;
        sleep(1); // 每秒发布一条消息
    }

    return 0;
}

```

subscriber.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::sub);

    // 连接到发布者地址
    socket.connect("tcp://localhost:5555");

    // 订阅所有主题
    socket.setsockopt(ZMQ_SUBSCRIBE, "", 0);

    // 接收并处理消息
    while (true) {
        // 接收主题
        zmq::message_t topicMsg;
        socket.recv(topicMsg, zmq::recv_flags::none);
        std::string topic(static_cast<char*>(topicMsg.data()), topicMsg.size());

        // 接收消息
        zmq::message_t messageMsg;
        socket.recv(messageMsg, zmq::recv_flags::none);
        std::string message(static_cast<char*>(messageMsg.data()), messageMsg.size());

        std::cout << "Received: " << topic << " - " << message << std::endl;
    }

    return 0;
}

```

#### 推送-拉取模式

pusher.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>
#include <unistd.h>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::push);

    // 绑定到指定地址
    socket.bind("tcp://*:5555");

    // 发送消息
    int count = 0;
    while (true) {
        std::string message = "Message " + std::to_string(count);

        // 发送消息
        zmq::message_t messageMsg(message.size());
        memcpy(messageMsg.data(), message.data(), message.size());
        socket.send(messageMsg, zmq::send_flags::none);

        std::cout << "Pushed: " << message << std::endl;

        count++;
        sleep(1); // 每秒推送一条消息
    }

    return 0;
}

```

puller.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::pull);

    // 连接到推送者地址
    socket.connect("tcp://localhost:5555");

    // 接收消息
    while (true) {
        zmq::message_t messageMsg;
        socket.recv(messageMsg, zmq::recv_flags::none);
        std::string message(static_cast<char*>(messageMsg.data()), messageMsg.size());

        std::cout << "Pulled: " << message << std::endl;
    }

    return 0;
}

```

#### 进程间通信示例

sender.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::push);

    // 连接到接收者的地址
    socket.connect("ipc:///tmp/zmq_ipc_example");

    // 发送消息
    std::string message = "Hello from sender!";
    zmq::message_t messageMsg(message.size());
    memcpy(messageMsg.data(), message.data(), message.size());
    socket.send(messageMsg, zmq::send_flags::none);

    return 0;
}

```

receiver.cpp

```cpp
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    // 创建上下文和套接字
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::pull);

    // 绑定到指定地址
    socket.bind("ipc:///tmp/zmq_ipc_example");

    // 接收消息
    zmq::message_t messageMsg;
    socket.recv(messageMsg, zmq::recv_flags::none);
    std::string message(static_cast<char*>(messageMsg.data()), messageMsg.size());

    std::cout << "Received message: " << message << std::endl;

    return 0;
}

```

以上。
