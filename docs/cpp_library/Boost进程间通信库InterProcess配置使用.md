







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Boost进程间通信库InterProcess配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Boost.InterProcess介绍](#smirk1_BoostInterProcess_7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__38)
	+ - [共享内存读写示例](#_39)




### 😏1. Boost.InterProcess介绍


项目Github地址：`https://github.com/boostorg/interprocess`


官网：`https://www.boost.org/doc/libs/1_83_0/doc/html/interprocess.html`


`Boost.InterProcess`是Boost库中的一个模块，提供了用于在C++中进行进程间通信和共享内存操作的功能。它提供了一组类和函数，使得在不同的进程之间能够安全地共享数据和同步访问。


`Boost.InterProcess`提供了以下主要功能：



> 
> 1.共享内存段（Shared Memory Segments）：Boost.Interprocess允许创建具有命名或匿名标识符的共享内存段。共享内存段可以在不同的进程之间共享数据，而不需要进行显式的数据拷贝。通过共享内存段，进程可以直接访问和修改共享的数据。
> 
> 
> 



> 
> 2.互斥锁和条件变量（Mutexes and Condition Variables）：为了避免多个进程同时访问共享内存时的数据竞争和冲突，Boost.Interprocess提供了互斥锁和条件变量。互斥锁用于保护共享数据的互斥访问，条件变量用于线程间的等待和通知机制。
> 
> 
> 



> 
> 3.共享内存容器（Shared Memory Containers）：Boost.Interprocess提供了一些容器类，如vector、map、list等，这些容器可以在共享内存中存储数据。共享内存容器提供了与STL容器相似的接口和功能，但可以用于多个进程之间的数据共享。
> 
> 
> 



> 
> 4.共享内存分配器（Shared Memory Allocators）：Boost.Interprocess提供了共享内存分配器，可以在共享内存中动态分配和释放内存。共享内存分配器确保在共享内存中的对象能够正确地分配和管理内存，以避免内存碎片和资源泄漏。
> 
> 
> 


`Boost.Interprocess`是一个功能强大且灵活的库，它具有跨平台的特性，可以在各种操作系统上使用。它提供了简单而一致的接口，使得在C++中使用共享内存变得更加方便和安全。可以轻松地实现进程间通信和数据共享，从而构建高效的多进程应用程序。


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libboost-dev

```

编译：



```
g++ -o main main.cpp -lboost\_system -lrt && ./main # -lrt是POSIX的RealTime库

```

### 😆3. 使用说明


#### 共享内存读写示例



```
#include <boost/interprocess/shared\_memory\_object.hpp>
#include <boost/interprocess/mapped\_region.hpp>
#include <iostream>

using namespace boost::interprocess;

int main()
{
    // 创建或打开共享内存对象
    shared_memory_object shm(open_or_create, "my\_shared\_memory", read_write);

    // 设置共享内存对象的大小
    shm.truncate(1024);

    // 映射共享内存到当前进程的地址空间
    mapped_region region(shm, read_write);

    // 获取共享内存的首地址
    void\* addr = region.get\_address();

    // 写入数据到共享内存
    const char\* str = "Hello, Boost.Interprocess!";
    std::strcpy(static\_cast<char\*>(addr), str);

    // 从共享内存读取数据
    char buffer[1024];
    std::strcpy(buffer, static\_cast<char\*>(addr));

    // 输出读取到的数据
    std::cout << "Message from shared memory: " << buffer << std::endl;

    // 删除共享内存对象
    shared_memory_object::remove("my\_shared\_memory");

    return 0;
}

```

此外，还有托管共享内存同步等操作，用到再学习。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





