







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍grpc远程过程调用（RPC）配置与使用。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__31)
	+ [:satisfied:3. 使用说明](#satisfied3__72)
	+ - [example官方示例：](#example_75)
		- [服务端和客户端调用示例：](#_89)




### 😏1. 项目介绍


项目Github地址：`https://github.com/grpc/grpc`


官网：`https://grpc.io/`


中文文档：`https://doc.oschina.net/grpc?t=57966`


`gRPC`是一个高性能、开源的远程过程调用（`RPC`）框架，由Google开发并基于`Protocol Buffers`实现。它可以在客户端和服务器之间进行快速、有效的通信，并支持多种编程语言。gRPC的设计目标是让开发者能够像调用本地函数一样调用远程服务，从而简化分布式系统的开发。


以下是gRPC的主要特点和优势：



> 
> 1.高效性能：gRPC使用基于HTTP/2的协议进行通信，支持双向流、流式处理和多路复用等特性，从而实现了更高效的数据传输和低延迟的通信。
> 
> 
> 



> 
> 2.强大的IDL（接口定义语言）：gRPC使用Protocol Buffers作为其默认的IDL，它提供了简单、轻量级的数据交换格式，并通过代码生成工具生成符合各种编程语言的代码。这使得开发者可以轻松定义服务接口和数据结构，并自动生成相应的代码。
> 
> 
> 



> 
> 3.多语言支持：gRPC支持多种编程语言，包括但不限于Java、C++、Python、Go、Node.js等，这使得不同语言的应用程序可以无缝地进行通信和集成。
> 
> 
> 



> 
> 4.支持多种服务类型：gRPC支持四种服务类型：Unary、Server Streaming、Client Streaming和Bidirectional Streaming。这使得开发者可以根据实际需求选择最适合的服务类型，并实现灵活的数据传输方式。
> 
> 
> 



> 
> 5.可插拔的认证和流控制：gRPC提供了可插拔的认证机制，可以基于SSL/TLS进行安全通信。此外，还支持流控制、拦截器、错误处理等功能，使得开发者可以更好地控制和管理通信过程。
> 
> 
> 



> 
> 6.丰富的生态系统：gRPC拥有活跃的社区和广泛的应用场景，许多知名公司和项目都在使用gRPC。这意味着你可以从丰富的资源中获取支持、文档和示例代码，从而更好地学习和使用gRPC。
> 
> 
> 


总结起来，gRPC是一个强大的远程过程调用框架，它具有高效性能、强大的IDL、多语言支持、多种服务类型和丰富的生态系统。通过使用gRPC，开发者可以轻松构建高性能、可扩展的分布式系统，并简化不同语言之间的通信和集成。


### 😊2. 环境配置


下面进行环境配置，包括apt安装和源码安装：



```
# apt安装
sudo apt install protobuf-compiler-grpc libgrpc++-dev

```


```
# 安装依赖
sudo apt install build-essential autoconf libtool pkg-config
# 源码安装
git clone --recurse-submodules -b v1.56.0 --depth 1 --shallow-submodules https://github.com/grpc/grpc
cd grpc
mkdir -p cmake/build
cd cmake/build
cmake -DgRPC\_INSTALL=ON \
      -DgRPC\_BUILD\_TESTS=OFF \
      -DCMAKE\_INSTALL\_PREFIX=$MY\_INSTALL\_DIR \
      ../..
make -j4
sudo make install
sudo ldconfig

```

测试代码，验证版本：



```
#include <iostream>
#include <grpcpp/grpcpp.h>

int main() {
  std::cout << "gRPC version: " << grpc\_version\_string() << std::endl;
  return 0;
}

```

编译运行：



```
g++ -o main main.cpp -lgrpc++ -lgrpc -lprotobuf -lpthread  && ./main

```

### 😆3. 使用说明


源码提供了一些案例，在example，可在此基础上扩充服务信息，即更改proto相关协议，用法跟`protobuf`类似。


#### example官方示例：



```
cd examples/cpp/helloworld
mkdir -p cmake/build
cd cmake/build
cmake -DCMAKE\_PREFIX\_PATH=$MY\_INSTALL\_DIR ../..
make -j4
# 运行
./greeter_server
./greeter_client
# 结果
Greeter received: Hello world

```

#### 服务端和客户端调用示例：


因为grpc也是用protobuf作为数据序列化协议，所以先创建`helloworld.proto`：



```
syntax = "proto3";

package helloworld;

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

```

使用protobuf-grpc编译器生成c++代码：



```
protoc -I=. --cpp\_out=. --grpc\_out=. --plugin=protoc-gen-grpc=`which grpc\_cpp\_plugin` helloworld.proto

```

服务端示例：



```
#include <iostream>
#include <memory>
#include <string>
#include <grpcpp/grpcpp.h>
#include "helloworld.grpc.pb.h"

using grpc::Server;
using grpc::ServerBuilder;
using grpc::ServerContext;
using grpc::Status;
using helloworld::Greeter;
using helloworld::HelloReply;
using helloworld::HelloRequest;

class GreeterServiceImpl final : public Greeter::Service
{
    Status SayHello(ServerContext \*context, const HelloRequest \*request,
                    HelloReply \*reply) override
    {
        std::string prefix("Hello, ");
        reply->set\_message(prefix + request->name());
        return Status::OK;
    }
};

void RunServer()
{
    std::string server\_address("0.0.0.0:50051");
    GreeterServiceImpl service;

    ServerBuilder builder;
    builder.AddListeningPort(server_address, grpc::InsecureServerCredentials());
    builder.RegisterService(&service);

    std::unique_ptr<Server> server(builder.BuildAndStart());
    std::cout << "Server listening on " << server_address << std::endl;
    server->Wait();
}

int main()
{
    RunServer();
    return 0;
}

```

客户端示例：



```
#include <iostream>
#include <memory>
#include <string>
#include <grpcpp/grpcpp.h>
#include "helloworld.grpc.pb.h"

using grpc::Channel;
using grpc::ClientContext;
using grpc::Status;
using helloworld::Greeter;
using helloworld::HelloReply;
using helloworld::HelloRequest;

class GreeterClient
{
public:
    GreeterClient(std::shared_ptr<Channel> channel)
        : stub\_(Greeter::NewStub(channel)) {}

    std::string SayHello(const std::string &name)
    {
        HelloRequest request;
        request.set\_name(name);

        HelloReply reply;
        ClientContext context;

        Status status = stub_->SayHello(&context, request, &reply);
        if (status.ok())
        {
            return reply.message();
        }
        else
        {
            return "RPC failed";
        }
    }

private:
    std::unique_ptr<Greeter::Stub> stub_;
};

int main()
{
    std::string server\_address("localhost:50051");
    GreeterClient greeter(grpc::CreateChannel(
        server_address, grpc::InsecureChannelCredentials()));
    std::string user("World");

    std::string reply = greeter.SayHello(user);
    std::cout << "Server replied: " << reply << std::endl;

    return 0;
}

```

编译：



```
g++ server.cpp helloworld.grpc.pb.cc helloworld.pb.cc -lgrpc++ -lgrpc -lprotobuf -lpthread -o server
g++ client.cpp helloworld.grpc.pb.cc helloworld.pb.cc -lgrpc++ -lgrpc -lprotobuf -lpthread -o client

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





