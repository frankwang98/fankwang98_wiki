







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍abseil-cpp基础组件库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__42)




### 😏1. 项目介绍


项目Github地址：`https://github.com/abseil/abseil-cpp`


官网：`https://abseil.io/`


Abseil 是 Google 开源的 C++ 库，旨在提供高质量、可靠且易于使用的基础设施组件。它由多个模块组成，涵盖了广泛的功能和领域，包括**字符串处理、并发编程、日志记录、时间处理**等。以下是 Abseil 的主要特点和组件：



> 
> 1.字符串库：Abseil 提供了一套强大而灵活的字符串处理工具，包括字符串拼接、分割、查找、替换等常用操作，以及基于模式匹配的功能。
> 
> 
> 



> 
> 2.并发库：Abseil 提供了各种并发编程的工具和原语，包括互斥锁、条件变量、原子操作等，帮助开发人员编写高效且线程安全的并发代码。
> 
> 
> 



> 
> 3.容器库：Abseil 包含了一系列高性能的容器类型，如 flat\_hash\_map、flat\_hash\_set、InlinedVector 等，用于管理数据集合并提供高效的访问和操作。
> 
> 
> 



> 
> 4.日志库：Abseil 提供了灵活的日志记录功能，支持多级别的日志消息、消息格式化、日志过滤等，方便开发人员进行调试和错误追踪。
> 
> 
> 



> 
> 5.时间库：Abseil 提供了可靠且易于使用的时间处理工具，包括时钟类型、时间间隔计算、日期时间格式化等，满足日常的时间操作需求。
> 
> 
> 



> 
> 6.效用库：Abseil 包含了许多实用的小工具和功能，如命令行解析器、随机数生成器、文件操作等，简化了常见任务的编码过程。
> 
> 
> 



> 
> 7.测试框架：Abseil 提供了全面而强大的测试框架，包括单元测试、性能测试和基准测试等，方便开发人员进行代码测试和性能优化。
> 
> 
> 


Abseil 遵循现代 C++ 的最佳实践，注重代码的易读性、可维护性和高性能，已被广泛应用于 Google 内部的项目。（很强）


### 😊2. 环境配置


下面进行环境配置：



```
git clone https://github.com/abseil/abseil-cpp.git
cd abseil-cpp
mkdir build
cd build
cmake .. -DCMAKE\_INSTALL\_PREFIX=/usr/local -DCMAKE\_CXX\_FLAGS=-fPIC
make
sudo make install

```

### 😆3. 使用说明


下面进行使用分析：


拼接字符串示例：



```
#include <iostream>
#include <string>
#include "absl/strings/str\_cat.h"

int main() {
  std::string str1 = "Hello";
  std::string str2 = "Abseil";
  std::string str3 = "!";
  
  // 使用 absl::StrCat 进行字符串拼接
  std::string result = absl::StrCat(str1, ", ", str2, str3);
  
  // 输出拼接结果
  std::cout << result << std::endl;
  
  return 0;
}

```

编译运行：



```
# 这个组件库每个要链接的库名不一样，原先以为是-labsl，一致不成功，开始怀疑自己了，后面发现要写明具体的组件库名称
g++ -o main main.cpp -labsl\_strings
./main

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





