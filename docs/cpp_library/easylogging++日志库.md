> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍跨平台轻量日志库easyloggingpp。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. 项目介绍

项目Github地址：`https://github.com/abumq/easyloggingpp`

`Easylogging++` 是一个轻量级、高性能的 C++ 日志库，旨在提供简单易用的日志记录功能。以下是 Easylogging++ 的一些特点和功能：

> 1.简单易用：Easylogging++ 提供了简洁的 API，使得添加日志记录功能变得非常容易。只需包含头文件并使用简单的宏即可进行日志记录，无需复杂的配置和初始化过程。

> 2.高性能：Easylogging++ 被设计为高性能的日志库，对于频繁的日志记录操作也有很好的性能表现。它采用了异步日志记录机制，将日志写入缓冲区，并在适当的时机进行批量写入，以提高性能。

> 3.灵活的日志配置：Easylogging++ 具有灵活的日志配置选项，可以根据需求进行自定义。你可以定义不同的日志级别，选择日志记录的目标（文件、终端等），设置日志格式等。可以通过配置文件或代码进行配置。

> 4.多线程支持：Easylogging++ 对多线程环境有良好的支持。它使用线程安全的方式处理日志记录，确保在多线程环境下的正确性和一致性。

> 5.跨平台：Easylogging++ 可以在多个平台上运行，包括 Windows、Linux、Mac等。它不依赖于任何特定的操作系统功能，具有很好的可移植性。

> 6.支持附加数据：除了记录文本日志消息外，Easylogging++ 还允许你附加其他数据，如时间戳、线程ID等，以便更详细地追踪和分析日志。

> 7.丰富的功能：Easylogging++ 提供了许多额外的功能，如日志滚动（按时间或文件大小自动分割日志文件）、日志过滤、标签支持等，以满足不同场景下的需求。

### 😊2. 安装运行

easyloggingpp日志库只需要在项目中包含头文件`easylogging++.h`和实现`easylogging++.cc`，即可实现丰富的日志打印功能。

最简日志打印示例：

```cpp
#include "easylogging++.h"

INITIALIZE_EASYLOGGINGPP    // 初始化宏，有且只能使用一次

int main(int argc, char* argv[]) {
   LOG(INFO) << "My first info log using default logger";
   return 0;
}

```

我们知道，简单的日志打印有`Fatal Error Warn Info Debug`5种等级，easylogging支持对日志等级和格式进行配置化，通常可通过配置文件进行管理，一个配置示例如下：

```bash
* GLOBAL:
   FORMAT               =  "%datetime %msg"
   FILENAME             =  "/tmp/logs/my.log"
   ENABLED              =  true
   TO_FILE              =  true
   TO_STANDARD_OUTPUT   =  true
   SUBSECOND_PRECISION  =  6
   PERFORMANCE_TRACKING =  true
   MAX_LOG_FILE_SIZE    =  2097152 ## 2MB - Comment starts with two hashes (##)
   LOG_FLUSH_THRESHOLD  =  100 ## Flush after every 100 logs
* DEBUG:
   FORMAT               = "%datetime{%d/%M} %func %msg"

```

另外，除了正常的日志输出外，还提供了**条件写日志，每执行n次写日志，写n次日志等功能**。但一般最常用的就是`LOG(LEVEL)`输出对应等级的日志。

示例如下：

```cpp
# 简单打印
LOG(INFO) << "Here is very simple example.";  
# 条件日志
LOG_IF(1 == 1, INFO) << "1 is equal to 1";  
# 偶然日志（配合for循环）
# 每n次记录一次
LOG_EVERY_N(20, INFO) << "LOG_EVERY_N i = ";
# 当计数达到n次之后，才开始记录日志 
LOG_AFTER_N(6, INFO) << "LOG_AFTER_N i = ";
# 当记录次数达到n次之后，就不再记录 
LOG_N_TIMES(1, INFO) << "LOG_N_TIMES i = ";

```

该项目也在`sample`目录内提供了多平台、多环境的应用示例，可参考。

### 😆3. 源码分析

源码也就是.h和.cc两个文件，一个单头文件的库。

easylogging++.h

```cpp
// 首先是很多平台、编译器的预判断，可以理解为一些开关
// 然后引用相关头文件
// 预先声明namespace和class
// Forward declarations
namespace el {
class Logger;
class LogMessage;
class PerformanceTrackingData;
class Loggers;
class Helpers;
template <typename T> class Callback;
class LogDispatchCallback;
class PerformanceTrackingCallback;
class LoggerRegistrationCallback;
class LogDispatchData;
namespace base {
class Storage;
class RegisteredLoggers;
class PerformanceTracker;
class MessageBuilder;
class Writer;
class PErrorWriter;
class LogDispatcher;
class DefaultLogBuilder;
class DefaultLogDispatchCallback;
#if ELPP_ASYNC_LOGGING
class AsyncLogDispatchCallback;
class AsyncDispatchWorker;
#endif // ELPP_ASYNC_LOGGING
class DefaultPerformanceTrackingCallback;
}  // namespace base
}  // namespace el

```

easylogging++.cc

```cpp
// 默认的日志保存路径
//---------------- DEFAULT LOG FILE -----------------------
#if defined(ELPP_NO_DEFAULT_LOG_FILE)
# if ELPP_OS_UNIX
static const char* kDefaultLogFile                         =      "/dev/null";
# elif ELPP_OS_WINDOWS
static const char* kDefaultLogFile                         =      "nul";
# endif // ELPP_OS_UNIX
#elif defined(ELPP_DEFAULT_LOG_FILE)
static const char* kDefaultLogFile                         =      ELPP_DEFAULT_LOG_FILE;
#else
static const char* kDefaultLogFile                         =      "myeasylog.log";
#endif // defined(ELPP_NO_DEFAULT_LOG_FILE)


#if !defined(ELPP_DISABLE_LOG_FILE_FROM_ARG)
static const char* kDefaultLogFileParam                    =      "--default-log-file";
#endif // !defined(ELPP_DISABLE_LOG_FILE_FROM_ARG)
#if defined(ELPP_LOGGING_FLAGS_FROM_ARG)
static const char* kLoggingFlagsParam                      =      "--logging-flags";
#endif // defined(ELPP_LOGGING_FLAGS_FROM_ARG)
static const char* kValidLoggerIdSymbols                   =
  "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._";
static const char* kConfigurationComment                   =      "##";
static const char* kConfigurationLevel                     =      "*";
static const char* kConfigurationLoggerId                  =      "--";

```

以上。





