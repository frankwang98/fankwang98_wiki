







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍glog日志库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__31)
	+ [:satisfied:3. 使用说明](#satisfied3__50)




### 😏1. 项目介绍


项目Github地址：`https://github.com/google/glog`


glog（Google Logging Library）是由 Google 开发的 C++ 日志库。它提供了一个简单易用的接口，用于在应用程序中记录日志消息。glog 被设计为高效、可靠和线程安全的，可以广泛应用于各种 C++ 项目中。


下面是 glog 的一些主要特点和优势：



> 
> 1.简单易用的接口：glog 提供了简洁明了的 API，使得开发人员可以轻松地在应用程序中记录日志消息。通过使用 LOG(INFO)、LOG(ERROR) 等宏，可以方便地输出不同级别的日志信息。
> 
> 
> 



> 
> 2.多级别日志：glog 支持不同级别的日志记录，包括 INFO、WARNING、ERROR、FATAL 等。通过配置日志级别，可以控制记录哪些级别及以上的日志消息。
> 
> 
> 



> 
> 3.日志格式化：glog 允许开发人员自定义日志消息的格式。它支持类似于 printf 的格式化字符串，可以添加变量、时间戳等信息到日志消息中。
> 
> 
> 



> 
> 4.完整的调用栈：glog 可以记录完整的调用栈信息，包括文件名、行号和函数名。这对于定位和调试问题非常有帮助。
> 
> 
> 



> 
> 5.后台线程写入：glog 使用后台线程异步写入日志文件，避免了频繁的磁盘 I/O 操作对应用程序性能的影响。
> 
> 
> 



> 
> 6.日志文件分割：glog 具有自动分割日志文件的功能，可以按照时间或大小进行日志文件的切换和轮转，避免了日志文件过大的问题。
> 
> 
> 



> 
> 7.线程安全：glog 被设计为线程安全的，可以在多线程环境中使用，而不会产生竞争条件或死锁。线程安全是通过内部使用互斥锁（mutex）来实现的。
> 
> 
> 



> 
> 8.支持日志级别过滤：glog 支持根据日志级别设置过滤规则，可以控制输出哪些级别的日志消息到终端或文件。
> 
> 
> 


glog 是一个简单易用、高效可靠的 C++ 日志库。它具有多级别日志记录、格式化、完整调用栈、后台线程写入、日志文件分割等功能。


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libgoogle-glog-dev

# 源码安装
git clone https://github.com/google/glog.git
cd glog
mkdir build && cd build
cmake ..
make && sudo make install

```

编译运行：



```
g++ -o main main.cpp -lglog && ./main

```

### 😆3. 使用说明


下面进行使用分析：


glog基本使用示例：



```
#include <glog/logging.h>

int main(int argc, char \*argv[])
{
    // 初始化 Google Logging Library , argv[0] 是当前程序的名称
    google::InitGoogleLogging(argv[0]);

    // 设置日志输出目录
    FLAGS_log_dir = "./logs";

    // 标准错误输出
    FLAGS_logtostderr = true;

    // 设置日志级别，INFO 级别及以上的日志会被输出
    FLAGS_minloglevel = google::INFO;

    // 使用宏输出不同级别的日志
    LOG(INFO) << "This is an INFO level log.";
    LOG(WARNING) << "This is a WARNING level log.";
    LOG(ERROR) << "This is an ERROR level log.";
    // LOG(FATAL) << "This is a FATAL level log."; // 会使程序abort

    return 0;
}
// result:
// I0811 11:25:20.819756 24411 main.cpp:18] This is an INFO level log.
// W0811 11:25:20.819849 24411 main.cpp:19] This is a WARNING level log.
// E0811 11:25:20.819866 24411 main.cpp:20] This is an ERROR level log.

```

glog将日志输出到文件示例：



```
#include <glog/logging.h>

void SetupLogging(const std::string& logDir)
{
    // 设置日志输出目录
    FLAGS_log_dir = logDir;
    // google::SetLogDestination(google::INFO, "./logs/my\_log\_");

    // 设置日志级别
    FLAGS_stderrthreshold = google::INFO;

    // 设置日志文件大小为10MB
    FLAGS_max_log_size = 10 \* 1024;  // 10MB

    // 禁用日志文件名中的机器名后缀
    FLAGS_alsologtostderr = true;
    FLAGS_logtostderr = false;

    // 自动移除旧日志 day （apt旧版本没有）
    // google::EnableLogCleaner(3);
}

void logCondition()
{
    for (int i = 0; i < 10; ++i)
    {
        // 条件日志
        LOG\_IF(INFO, i > 5) << "Got lots of cookies" << i;
        LOG\_EVERY\_N(INFO, 10) << "Got the " << google::COUNTER << "th cookie";
    }
}

int main(int argc, char \*argv[])
{
    // 设置日志目录
    std::string logDir = "./logs";

    // 初始化日志
    SetupLogging(logDir);

    // 初始化 Glog
    google::InitGoogleLogging(argv[0]);

    // 示例输出不同级别的日志
    LOG(INFO) << "This is an informational message.";
    LOG(WARNING) << "This is a warning message.";
    LOG(ERROR) << "This is an error message.";
    // LOG(FATAL) << "This is a fatal error message.";

    logCondition();

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





