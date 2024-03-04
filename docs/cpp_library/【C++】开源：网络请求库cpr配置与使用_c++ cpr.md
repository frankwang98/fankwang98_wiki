







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍网络请求库cpr配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__45)




### 😏1. 项目介绍


官网：`https://docs.libcpr.org/`


Github地址：`https://github.com/libcpr/cpr`


`CPR`（`C++ Requests`）是一个现代化的、轻量级的、功能强大的C++网络请求库，用于进行HTTP请求和处理HTTP响应。它提供了简洁且易于使用的API，使开发人员能够以简单的方式执行HTTP请求并处理响应。


以下是CPR库的一些特点和功能：



> 
> 1.轻量级和易于使用：CPR具有简单而直观的API设计，使您能够轻松地执行常见的HTTP请求，如GET、POST、PUT和DELETE等。
> 
> 
> 



> 
> 2.多种请求参数设置：您可以设置请求的URL、请求头、请求体、查询参数、超时时间等各种请求参数。这使得您可以根据需要进行灵活的配置。
> 
> 
> 



> 
> 3.异步和同步请求：CPR支持异步和同步两种方式进行请求。您可以选择适合您应用程序的方式来执行请求。
> 
> 
> 



> 
> 4.响应处理和错误处理：CPR提供了处理HTTP响应的丰富功能，包括获取响应状态码、响应头、响应体等。它还提供了错误处理机制，以便在出现错误时进行适当的处理。
> 
> 
> 



> 
> 5.文件上传和下载：CPR支持文件上传和下载功能，使您能够方便地进行文件的传输操作。
> 
> 
> 



> 
> 6.支持各种平台：CPR可以在多种平台上运行，包括Windows、Linux和macOS等。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# 源码安装
sudo apt install build-essential cmake libcurl4-openssl-dev libssl-dev
git clone https://github.com/libcpr/cpr
mkdir build
cd build
cmake ..
make
sudo make install

```


```
# 编译
g++ -std=c++17 -o main main.cpp -lcpr

```

### 😆3. 使用说明


HTTP请求与响应示例：



```
// Get
#include <cpr/cpr.h>
#include <iostream>

int main(int argc, char\*\* argv) {
    cpr::Response r = cpr::Get(cpr::Url{"https://api.github.com/repos/libcpr/cpr/contributors"},
                      cpr::Authentication{"user", "pass", cpr::AuthMode::BASIC},
                      cpr::Parameters{{"anon", "true"}, {"key", "value"}});
    r.status_code;                  // 200
    r.header["content-type"];       // application/json; charset=utf-8
    r.text;                         // JSON text string
    std::cout << "status\_code: " << r.status_code << std::endl;
    std::cout << "header: " << r.header["content-type"] << std::endl;
    std::cout << "text: " << r.text << std::endl;
}

```


```
// Post
#include <iostream>
#include <cpr/cpr.h>

int main() {
    // 构造要发送的JSON数据
    std::string json_data = R"({"name": "John", "age": 30})";

    // 发起POST请求 
    cpr::Response response = cpr::Post(cpr::Url{"https://api.github.com/repos/libcpr/cpr/contributors"}, cpr::Body{json_data});

    // 检查请求是否成功
    if (response.status_code == 200) {
        std::cout << "请求成功！" << std::endl;
        std::cout << "响应内容：" << response.text << std::endl;
    } else {
        std::cout << "请求失败！错误代码：" << response.status_code << std::endl;
        std::cout << "错误信息：" << response.error.message << std::endl;
    }

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





