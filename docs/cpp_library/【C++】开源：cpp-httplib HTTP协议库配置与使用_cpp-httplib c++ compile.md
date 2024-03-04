







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍cpp-httplib HTTP协议库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__36)




### 😏1. 项目介绍


项目Github地址：`https://github.com/yhirose/cpp-httplib`


`cpp-httplib`是一个C++编写的开源HTTP客户端/服务器库，用于处理HTTP请求和响应。它提供了简单易用的接口和功能，使开发者能够轻松地构建基于HTTP协议的应用程序。


以下是`cpp-httplib`的一些关键特点和功能：



> 
> 1.轻量级：cpp-httplib是一个轻量级的库，仅依赖于C++标准库，无需安装额外的依赖项。
> 
> 
> 



> 
> 2.简单易用的接口：cpp-httplib提供了简单直观的接口，使开发者能够方便地处理HTTP请求和响应。您可以轻松地创建服务器、处理路由、读取请求参数、设置响应头等。
> 
> 
> 



> 
> 3.客户端功能：cpp-httplib可以用作HTTP客户端，发送HTTP请求并接收响应。您可以设置请求头、请求参数、处理响应数据等。
> 
> 
> 



> 
> 4.SSL/TLS支持：cpp-httplib支持通过SSL/TLS进行安全的HTTP通信。您可以使用HTTPS协议进行加密通信，确保数据传输的安全性。
> 
> 
> 



> 
> 5.静态文件服务器：cpp-httplib提供了静态文件服务器的功能，可以轻松地将静态文件（如HTML、CSS、JavaScript、图像等）提供给客户端。
> 
> 
> 



> 
> 6.跨平台支持：cpp-httplib可在多个平台上运行，包括Windows、Linux和macOS等。
> 
> 
> 


`cpp-httplib`是一个简单而功能丰富的C++ HTTP库，适用于构建各种基于HTTP协议的应用程序，如Web服务器、RESTful API、HTTP客户端等。


### 😊2. 环境配置


`cpp-httplib`是一个单头文件的c++库，因此在项目中只有加入该头文件`httplib.h`即可。



```
# 编译
g++ -o server server.cpp -lpthread
g++ -o client client.cpp -lpthread

```

### 😆3. 使用说明


HTTP请求/响应服务端和客户端示例：



```
// server.cpp
#include <iostream>
#include "httplib.h"

void handle\_post(const httplib::Request& req, httplib::Response& res) {
    // 获取请求体数据
    std::string body = req.body;

    // 在服务器端打印请求体数据
    std::cout << "Received POST request with body: " << body << std::endl;

    // 设置响应内容
    res.set\_content("POST request received", "application/json");
}

int main() {
    httplib::Server svr;

    svr.Get("/", [](const httplib::Request& req, httplib::Response& res) {
        res.set\_content("Hello, World!", "text/plain");
    });

    svr.Get("/about", [](const httplib::Request& req, httplib::Response& res) {
        res.set\_content("About page", "text/plain");
    });

    svr.Post("/api/users", handle_post);

    std::cout << "Server started on port 8080" << std::endl;
    svr.listen("localhost", 8080);

    return 0;
}

```


```
// client.cpp
#include <iostream>
#include "httplib.h"

int main() {
    httplib::Client cli("localhost", 8080);

    // 发送GET请求
    auto res = cli.Get("/");
    if (res && res->status == 200) {
        std::cout << res->body << std::endl;
    }

    res = cli.Get("/about");
    if (res && res->status == 200) {
        std::cout << res->body << std::endl;
    }

    // 发送POST请求
    httplib::Headers headers = {
        { "Content-Type", "application/json" }  // 设置请求头 MIME类型
    };

    std::string body = R"({"name": "John", "age": 30})";  // 请求体数据

    auto res2 = cli.Post("/api/users", headers, body, "application/json");

    if (res2 && res2->status == 200) {
        std::cout << "Request successful: " << res2->body << std::endl;
    } else {
        std::cout << "Request failed!" << std::endl;
    }

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





