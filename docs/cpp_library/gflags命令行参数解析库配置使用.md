







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍gflags命令行参数解析库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__49)




### 😏1. 项目介绍


项目Github地址：`https://github.com/gflags/gflags`


gflags（也称为 Google Flags）是 Google 开源的一个命令行参数解析库，用于处理命令行参数的定义、解析和访问。它可以帮助开发者方便地定义和使用命令行参数，以控制程序的行为。


下面是 gflags 的一些主要特点和用法：



> 
> 1.定义命令行参数：使用 gflags，您可以通过宏来定义命令行参数，例如 DEFINE\_bool、DEFINE\_int32、DEFINE\_string 等。这些宏将会生成相应类型的全局变量，并可用于指定默认值、设置帮助信息等。
> 
> 
> 



> 
> 2.解析命令行参数：gflags 提供了 ParseCommandLineFlags 函数，用于解析命令行参数并将其存储在相应的全局变量中。在程序启动时，您可以调用该函数来解析命令行参数。
> 
> 
> 



> 
> 3.访问命令行参数：一旦命令行参数被解析，您可以直接访问相应的全局变量来获取命令行参数的值。例如，如果定义了一个 bool 类型的命令行参数 my\_flag，则可以通过 FLAGS\_my\_flag 来访问其值。
> 
> 
> 



> 
> 4.支持多种数据类型：gflags 支持多种常见的数据类型，包括布尔型、整数型、浮点型、字符串型等。您可以根据参数的类型选择相应的宏进行定义。
> 
> 
> 



> 
> 5.自动生成帮助信息：通过定义命令行参数时提供的参数说明，gflags 可以自动生成帮助信息。您可以通过设置 --help 参数来显示帮助信息，以了解可用的命令行参数和其意义。
> 
> 
> 



> 
> 6.支持配置文件：gflags 可以读取和解析配置文件中的参数值，这样可以方便地批量设置参数。您可以使用 --flagfile 参数指定配置文件的路径。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libgflags-dev

```


```
# 源码编译
git clone https://github.com/gflags/gflags
cd gflags
mkdir build && cd build
cmake ..
cmake -DBUILD\_SHARED\_LIBS=ON -DBUILD\_STATIC\_LIBS=ON -DINSTALL\_HEADERS=ON -DINSTALL\_SHARED\_LIBS=ON -       DINSTALL\_STATIC\_LIBS=ON .. # 一些项目需要安装动态库和静态库，可以用这个cmake
make
sudo make install

```

编译运行示例：



```
g++ -o main main.cpp -lgflags
./main （对参数进行赋值，不加参数则为默认）

```

### 😆3. 使用说明


下面进行使用分析：


一个命令行参数解析示例：



```
#include <iostream>
#include <gflags/gflags.h>

DEFINE\_string(name, "zhang san", "your name");
DEFINE\_int32(age, 18, "your age");
DEFINE\_bool(verbose, false, "Enable verbose mode");
DEFINE\_int32(count, 10, "Number of iterations");

// Usage: --name= --age= --verbose= --count=

int main(int argc, char\*\* argv) {
	gflags::ParseCommandLineFlags(&argc, &argv, true);

	std::cout << "your name is : " << FLAGS_name
        << ", your age is: " << FLAGS_age << std::endl;

    if (FLAGS_verbose) {
        std::cout << "Verbose mode is enabled!" << std::endl;
    }

    std::cout << "Running " << FLAGS_count << " iterations." << std::endl;
    
    return 0;
}

// 运行示例：./main --age=16 --verbose=true --count=19

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





