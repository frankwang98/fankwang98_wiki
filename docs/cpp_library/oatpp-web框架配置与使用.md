







> 
> 说到web开发，大家肯定会想到JS、Python，甚至Java，但应该不会想到C++。
> 
> 
> 


用C++开发web也不是不行，这不，`oatpp`就是一个轻量、跨平台、高性能的web框架。


crow也是一个c++ web框架，类似于Python的Flask，参考安装入门如下：



```
http://t.csdn.cn/eI3zD

```



#### 文章目录


* + [1. oatpp介绍](#1_oatpp_11)
	+ [2. 环境搭建](#2__36)
	+ [3. 示例程序](#3__52)




### 1. oatpp介绍


官网：`https://oatpp.io/`  
 文档：`https://oatpp.io/docs/start`  
 github地址：`https://github.com/oatpp/oatpp`


`oatpp-web`是一个基于C++的高性能Web框架，用于构建现代化、可扩展的Web应用程序和API服务。它提供了一整套工具和功能，方便开发人员设计、开发和维护高性能的Web应用。


以下是oatpp-web的主要特点和功能：



> 
> 1.轻量级和高性能：oatpp-web具有高效的设计，以提供最佳的性能和资源利用。它采用异步I/O和事件驱动的编程模型，能够处理大量并发请求。
> 
> 
> 



> 
> 2.RESTful API支持：oatpp-web对RESTful风格的API提供了良好的支持。您可以使用注解方式定义路由和控制器，轻松创建和管理API端点。
> 
> 
> 



> 
> 3.HTTP/HTTPS协议支持：oatpp-web支持HTTP和HTTPS协议，可以安全地处理加密通信，并提供SSL/TLS配置选项。
> 
> 
> 



> 
> 4.中间件支持：oatpp-web提供了中间件机制，允许开发人员在请求处理过程中添加自定义的中间件组件。这样可以方便地实现例如身份验证、日志记录、缓存等功能。
> 
> 
> 



> 
> 5.数据库集成：oatpp-web可以与各种关系型数据库（如MySQL、PostgreSQL）和NoSQL数据库（如MongoDB）进行集成，通过ORM（对象关系映射）和查询构建器，方便地操作和管理数据。
> 
> 
> 



> 
> 6.内置JSON支持：oatpp-web内置了强大的JSON序列化和反序列化功能，可以快速地处理JSON数据。它支持将C++对象转换为JSON格式，并能够自动进行类型映射和验证。
> 
> 
> 



> 
> 7.测试和调试支持：oatpp-web提供了丰富的测试和调试工具，包括单元测试框架、集成测试支持和调试日志输出等，有助于开发人员快速验证和调试应用程序。
> 
> 
> 



> 
> 8.跨平台支持：oatpp-web支持多种操作系统和平台，包括Linux、Windows和MacOS等。您可以在不同环境中轻松部署和运行oatpp-web应用程序。
> 
> 
> 


### 2. 环境搭建


编译安装：



```
# 下载源码
git clone https://github.com/oatpp/oatpp.git

# 编译
cd oatpp
mkdir build && cd build
cmake .. # （1.3.0需要cmake 3.20以上）
sudo make && sudo make install

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b8eac7034474691835d1057d117d93f.png)


### 3. 示例程序


运行一个基于oatpp框架的示例程序（响应hello world）：


创建oatpp\_demo目录，并创建`CMakeLists.txt`：



```
cmake\_minimum\_required(VERSION 3.1)
project(helloworld)
 
set(CMAKE_CXX_STANDARD 11)
set(SOURCE_FILES main.cpp handler.h)
 
# 查找 oatpp 依赖
find\_package(oatpp REQUIRED)
 
add\_executable(${PROJECT_NAME} ${SOURCE_FILES})
 
# 将目标文件与库文件进行链接
target\_link\_libraries(${PROJECT_NAME} oatpp::oatpp)

```

头文件`handler.h`，实现响应HttpRequestHandler：



```
// handler.h
#ifndef HANDLER\_H
#define HANDLER\_H
 
#include "oatpp/web/server/HttpRequestHandler.hpp"
 
#define O\_UNUSED(x) (void)x;
 
// 自定义请求处理程序
class Handler : public oatpp::web::server::HttpRequestHandler
{
public:
    // 处理传入的请求，并返回响应
    std::shared_ptr<OutgoingResponse> handle(const std::shared_ptr<IncomingRequest>& request) override {
        O\_UNUSED(request);
 
        return ResponseFactory::createResponse(Status::CODE_200, "Hello, World! This is oatpp\_demo!");
    }
};
 
#endif // HANDLER\_H

```

主程序main.cpp，提供路由Router请求：



```
// main.cpp
#include "oatpp/web/server/HttpConnectionHandler.hpp"
#include "oatpp/network/tcp/server/ConnectionProvider.hpp"
#include "oatpp/network/Server.hpp"
#include "handler.h"
 
void run()
{
    // 为 HTTP 请求创建路由器
    auto router = oatpp::web::server::HttpRouter::createShared();
 
    // 路由 GET - "/hello" 请求到处理程序
    router->route("GET", "/hello", std::make\_shared<Handler>());
 
    // 创建 HTTP 连接处理程序
    auto connectionHandler = oatpp::web::server::HttpConnectionHandler::createShared(router);
 
    // 创建 TCP 连接提供者
    auto connectionProvider = oatpp::network::tcp::server::ConnectionProvider::createShared({"localhost", 8080, oatpp::network::Address::IP_4});
 
    // 创建服务器，它接受提供的 TCP 连接并将其传递给 HTTP 连接处理程序
    oatpp::network::Server server(connectionProvider, connectionHandler);
 
    // 打印服务器端口
    OATPP\_LOGI("MyApp", "Server running on port %s", connectionProvider->getProperty("port").getData());
 
    // 运行服务器
    server.run();
}
 
int main()
{
    // 初始化 oatpp 环境
    oatpp::base::Environment::init();
 
    // 运行应用
    run();
 
    // 销毁 oatpp 环境
    oatpp::base::Environment::destroy();
 
    return 0;
}

```

cmake工程编译：



```
mkdir build && cd build
cmake ..
make

```

然后在浏览器打开：`http://127.0.0.1:8080/hello`


![在这里插入图片描述](https://img-blog.csdnimg.cn/4d691c0f722c40229ba466ae5eb95ba3.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/d7f5dbcdd2e74c5d8822f6ed804e6707.png#pic_center)


以上。





