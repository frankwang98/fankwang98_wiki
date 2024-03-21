## brpc介绍
Apache bRPC（以前称为 brpc）是一个开源的高性能、可伸缩的 RPC 框架，由百度开发并捐赠给 Apache 软件基金会。它专注于提供可靠的 RPC 通信，并针对大规模分布式系统进行了优化，旨在满足互联网公司的高性能、高可用性、高可伸缩性的需求。

github: `https://github.com/apache/brpc`

wiki: `https://brpc.apache.org/zh/docs`

下面是一些 Apache bRPC 的主要特性和优势：

>1.高性能：bRPC 采用了多种技术手段来提升性能，包括零拷贝、IO 多路复用、多线程模型等。它还支持请求复用、连接池等机制，提高了并发处理能力。

>2.可伸缩性：bRPC 的架构设计允许在大规模分布式系统中进行水平扩展。它支持服务注册中心、负载均衡、服务发现等功能，可以灵活地扩展和管理服务。

>3.多语言支持：bRPC 提供了多种语言的客户端和服务器库，包括 C++、Python、Java 等，使得开发者可以使用自己熟悉的编程语言来构建 RPC 服务。

>4.丰富的功能：除了基本的 RPC 调用功能之外，bRPC 还支持灵活的参数传递、自定义序列化、异步调用、流式处理等高级特性，满足了各种复杂场景下的需求。

>5.可靠性：bRPC 提供了多种机制来确保通信的可靠性，包括超时控制、重试机制、心跳检测等。它还支持异步通知、流量控制等功能，帮助用户构建可靠的分布式系统。

总的来说，Apache bRPC 是一个功能丰富、性能优异、可靠稳定的 RPC 框架，适用于构建大规模分布式系统中的各种服务。

## 环境配置

apt安装：
```sh
sudo apt-get install protobuf-compiler libprotobuf-dev
sudo apt-get install zookeeper zookeeper-bin zookeeperd zookeeper-examples
sudo apt-get install brpc
```

## 应用示例
brpc c++示例：

```cpp
#include <iostream>
#include <brpc/server.h>
#include <brpc/channel.h>
#include "echo.pb.h"  // 生成的协议文件

// 定义 RPC 服务
class EchoServiceImpl : public EchoService {
public:
    virtual void Echo(::google::protobuf::RpcController* controller,
                      const EchoRequest* request,
                      EchoResponse* response,
                      ::google::protobuf::Closure* done) {
        // 实现 Echo 方法
        response->set_message(request->message());
        done->Run();
    }
};

int main() {
    // 启动 RPC 服务器
    brpc::Server server;
    EchoServiceImpl echo_service_impl;
    if (server.AddService(&echo_service_impl,
                          brpc::SERVER_DOESNT_OWN_SERVICE) != 0) {
        std::cerr << "Fail to add EchoService" << std::endl;
        return -1;
    }
    brpc::ServerOptions options;
    if (server.Start(50051, &options) != 0) {
        std::cerr << "Fail to start EchoService" << std::endl;
        return -1;
    }
    std::cout << "EchoService is running on 50051" << std::endl;

    // 创建 RPC 客户端
    brpc::Channel channel;
    if (channel.Init("127.0.0.1:50051", nullptr) != 0) {
        std::cerr << "Fail to initialize Channel" << std::endl;
        return -1;
    }
    EchoService_Stub stub(&channel);

    // 调用 RPC 方法
    EchoRequest request;
    EchoResponse response;
    brpc::Controller controller;
    request.set_message("Hello, brpc");
    stub.Echo(&controller, &request, &response, nullptr);
    if (controller.Failed()) {
        std::cerr << "RPC failed, " << controller.ErrorText() << std::endl;
        return -1;
    }
    std::cout << "Response: " << response.message() << std::endl;

    // 关闭 RPC 服务器
    server.Stop(0);

    return 0;
}
```