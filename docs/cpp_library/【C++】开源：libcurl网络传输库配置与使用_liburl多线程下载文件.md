







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍libcurl网络传输库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__33)
	+ [:satisfied:3. 使用说明](#satisfied3__45)




### 😏1. 项目介绍


官网：`https://curl.se/`


项目Github地址：`https://github.com/curl/curl`


`libcurl` 是一个功能强大、开源的网络传输库，它支持多种协议，包括 `HTTP、HTTPS、FTP、SMTP、POP3` 等。libcurl 提供了一组易于使用的 API，可以用于在应用程序中进行网络通信。


下面是一些 `libcurl` 的主要特点和功能：



> 
> 1.支持多种协议：libcurl 支持常用的网络协议，例如 HTTP、HTTPS、FTP、SMTP、POP3、IMAP 等，使开发者能够通过统一的接口处理各种网络传输需求。
> 
> 
> 



> 
> 2.容易集成：libcurl 提供了简洁易用的 C/C++ API，可以轻松地将其集成到各种应用程序中，无论是命令行工具还是图形界面应用。
> 
> 
> 



> 
> 3.多线程支持：libcurl 可以与多线程环境很好地配合使用，可以在多个线程中同时进行网络操作。
> 
> 
> 



> 
> 4.支持代理：libcurl 具有广泛的代理支持，可以配置和使用各种代理服务器进行网络传输。
> 
> 
> 



> 
> 5.SSL/TLS 加密支持：libcurl 内部集成了 OpenSSL 或者其他加密库，支持安全的 HTTPS 连接，保证数据的机密性和完整性。
> 
> 
> 



> 
> 6.断点续传：libcurl 允许断点续传功能，可以在网络连接中断后继续传输（可实现类似IDM多线程下载器的应用）。
> 
> 
> 



> 
> 7.Cookie 支持：libcurl 具有完整的 Cookie 支持，可以处理和管理 Web 服务器发送的 Cookie。
> 
> 
> 



> 
> 8.自定义回调：libcurl 提供了回调函数接口，允许开发者自定义处理网络传输过程中的事件和数据。
> 
> 
> 



> 
> 9.跨平台：libcurl 可以在多个操作系统上运行，包括 Windows、Linux、macOS 等。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libcurl4-openssl-dev

```

编译运行：



```
g++ -o main main.cpp -lcurl && ./main

```

### 😆3. 使用说明


HTTP 请求和响应示例：



```
#include <iostream>
#include <curl/curl.h>

// 回调函数，用于处理服务器响应的数据
size_t WriteCallback(void\* contents, size_t size, size_t nmemb, std::string\* response) {
    size_t totalSize = size \* nmemb;
    response->append(static\_cast<char\*>(contents), totalSize);
    return totalSize;
}

int main() {
    CURL\* curl;
    CURLcode res;

    // 初始化Curl库
    curl\_global\_init(CURL_GLOBAL_DEFAULT);

    // 创建Curl句柄
    curl = curl\_easy\_init();
    if (curl) {
        std::string response;

        // 设置请求URL
        curl\_easy\_setopt(curl, CURLOPT_URL, "https://example.com");

        // 设置POST请求
        curl\_easy\_setopt(curl, CURLOPT_POST, 1L);

        // 设置POST数据
        std::string postData = "key1=value1&key2=value2";
        curl\_easy\_setopt(curl, CURLOPT_POSTFIELDS, postData.c\_str());

        // 设置回调函数，处理服务器响应的数据
        curl\_easy\_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl\_easy\_setopt(curl, CURLOPT_WRITEDATA, &response);

        // 执行请求
        res = curl\_easy\_perform(curl);
        if (res == CURLE_OK) {
            // 请求成功，打印服务器响应的数据
            std::cout << "Response: " << response << std::endl;
        } else {
            // 请求失败，打印错误信息
            std::cerr << "Curl request failed: " << curl\_easy\_strerror(res) << std::endl;
        }

        // 清理Curl句柄
        curl\_easy\_cleanup(curl);
    }

    // 清理Curl库
    curl\_global\_cleanup();

    return 0;
}

```

FTP文件下载示例：



```
#include <iostream>
#include <curl/curl.h>

// 回调函数，用于处理接收到的 FTP 数据
size_t writeCallback(char\* buf, size_t size, size_t nmemb, std::string\* response) {
    size_t totalSize = size \* nmemb;
    response->append(buf, totalSize);
    return totalSize;
}

int main() {
    CURL\* curl;
    CURLcode res;

    // 初始化 libcurl
    curl\_global\_init(CURL_GLOBAL_DEFAULT);

    // 创建一个 curl 句柄
    curl = curl\_easy\_init();
    if (curl) {
        // 设置 FTP URL
        curl\_easy\_setopt(curl, CURLOPT_URL, "ftp://example.com/path/to/ftp/file.txt");

        // 设置用户名和密码（如果需要）
        curl\_easy\_setopt(curl, CURLOPT_USERPWD, "username:password");

        // 设置写回调函数
        std::string response;
        curl\_easy\_setopt(curl, CURLOPT_WRITEFUNCTION, writeCallback);
        curl\_easy\_setopt(curl, CURLOPT_WRITEDATA, &response);

        // 执行 FTP 下载操作
        res = curl\_easy\_perform(curl);
        if (res != CURLE_OK) {
            std::cerr << "curl\_easy\_perform() failed: " << curl\_easy\_strerror(res) << std::endl;
        } else {
            std::cout << "FTP Response: " << response << std::endl;
        }

        // 清理 curl 句柄
        curl\_easy\_cleanup(curl);
    }

    // 清理 libcurl
    curl\_global\_cleanup();

    return 0;
}

```

基于libcurl的多线程下载工具，github地址：`https://github.com/tuyungang/http_multi_thread_download_tool`



```
# 编译
cd xxx && make
# 下载示例(微信安装文件，200M左右)
./multi_thread_download -H https://dldir1.qq.com/weixin/Windows/WeChatSetup.exe -s 50
# 多线程下载器本质是自定义启动多个线程，分别下载文件的不同部分，最后将各个段拼接起来组成最后文件，中间文件删除，实现并行下载和分块下载的效果（libcurl支持了断点续传功能）。

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





