### 😏1. C++介绍

C++官网：`https://isocpp.org/`

cppreference：`http://cppreference.com/`

cplusplus：`https://cplusplus.com/`

#### 官方语言

**`C++` 是一种通用的编程语言，具有高效和强大的特性，适用于开发各种类型的软件和系统**。它是 C 语言的一个超集（*即任何合法的 C 程序都是合法的 C++ 程序*），可以使用 C 语言的所有特性和库，同时也引入了许多新的特性，例如类、继承、多态等面向对象编程的概念，以及泛型编程、异常处理、STL 等高级特性。

与 C 语言相比，C++ 更适合开发大型项目和复杂的系统。它具有严格的类型检查和内存管理，能够提高程序的可靠性和安全性。同时，C++ 也具备高效和灵活性的优势，支持直接操作底层硬件和编写高性能代码。这些优点使得 C++ 成为广泛使用的编程语言，被应用于各个领域，**如操作系统、嵌入式、数据库、游戏开发、音视频传输、图像处理、金融和科学计算等**。

除了标准 C++ 语言的基础特性外，C++ 标准库（`STL`）也提供了丰富的数据结构和算法库，可用于开发各种类型的应用程序。此外，C++ 还有许多扩展库和框架，如 `Boost`、`Qt`、`OpenCV` 等，可以扩展其功能和应用范围。

#### 组成

* 核心语法：编程语言通用模块，如输入输出、常量变量、数据类型等
* 标准库：库中提供了大量函数接口，可用于操作字符串、文件等
* 标准模板库STL：提供了许多数据类型操作的函数接口

#### 特性

C++ 完全支持面向对象的程序设计，包括面向对象开发的四大特性：

* 封装：用类class将属性和方法组合在一起，对外隐藏细节
* 继承：子类可以继承父类的属性和方法，并可扩展与修改
* 多态：同一种操作作用于不同的对象，可以有不同的解释和实现
* 抽象：从类的实例中提取公共特征，形成抽象类或接口，便于复用

#### 学习指南

内功四大件：数据结构与算法、计算机网络、操作系统、设计模式

应用实践：Windows API、Linux API、网络通信、多线程、数据库、GUI、OpenCV、OpenGL等

程序员学习路线：**函数式编程、面向对象编程、泛型编程、STL编程、数据结构与算法、网络编程、多线程与并发、操作系统编程和设计模式**等，无论哪种编程语言，在学习的同时需要不断实践，有条件的话跟着项目学是最好的。

在线书籍：  
 [C++ Primer Plus](https://shenjun.gitbooks.io/c-primer-plus/content/)  
 [C++ 程序设计语言](https://shenjun4cplusplus2.github.io/cplusplus2html/)  
 [STL](https://cui-jiacai.gitbook.io/c++-stl-tutorial/)  
 [Boost](https://wizardforcel.gitbooks.io/the-boost-cpp-libraries/content/1.html)  
 [Asio](https://mmoaay.gitbooks.io/boost-asio-cpp-network-programming-chinese/content/index.html)  
 [数据结构与算法](https://xiuxin.gitbook.io/datastructre/)  
 [数据结构与算法2](https://mqjyl2012.gitbook.io/algorithm/)  
 [代码随想录](https://www.programmercarl.com/)  
 [并发](https://nj.gitbooks.io/c/content/)  
 [ModernCpp](https://vivym.gitbooks.io/effective-modern-cpp-zh/content/)  
 [EffectiveCpp](https://wizardforcel.gitbooks.io/effective-cpp/content/index.html)  
 [重构](https://book-refactoring2.ifmicro.com/docs/ch1.html)  
 [GoogleStyle](https://zh-google-styleguide.readthedocs.io/en/latest/)

### 😊2. 环境安装与配置

工作中更多的Workspace是`Linux`，这里选择常用的`Ubuntu`发行版，因为我们常用的办公环境是`Windows`，选择Ubuntu系统可以是**WSL或者一台远程的Ubuntu服务器**。

当我们进入WSL或远程Ubuntu时，首先确认以下环境：

#### g++

一般Linux会预装g++，这里通过`g++ -v`查看g++版本。有了g++，我们就可以编译c++程序了，通过以下指令：

```bash
g++ main.cpp         # 默认生成a.out
g++ main.cpp -o main # 生成-o后的可执行文件
```

用g++一个个编译程序不太方便，随后又衍生出make、cmake等构建系统。

#### make

make构建用到的时`makefile`文件。makefile用于描述软件项目中的源代码文件如何编译和链接成可执行文件、库文件或其他目标文件，提供了一种便捷且灵活的方式来管理和构建项目。

```bash
# 编译器
CXX = g++
# 编译参数
CXXFLAGS = -Wall -g

# 目标文件
TARGET = myprogram
# 源代码文件
SRCS = main.cpp utils.cpp
# 对应的目标文件
OBJS = $(SRCS:.cpp=.o)

# 默认目标，生成可执行文件
all: $(TARGET)

# 生成可执行文件
$(TARGET): $(OBJS)
	$(CXX) $(CXXFLAGS) -o $@ $^

# 生成目标文件
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

# 清理中间文件和可执行文件
clean:
	rm -f $(OBJS) $(TARGET)

```

#### CMake

CMake构建用到的是`CMakeLists.txt`文件。

CMake 是一个跨平台的开源构建工具，用于自动化地生成与平台特定编译器和构建系统无关的构建脚本和配置文件。

```bash
# 设置 CMake 最低版本要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本号
project(MyProject VERSION 1.0)

# 添加可执行文件
add_executable(myprogram main.cpp utils.cpp)

# 设置编译选项
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 添加头文件搜索路径
include_directories(include)

# 添加依赖库
find_package(OpenCV REQUIRED)
target_link_libraries(myprogram ${OpenCV\_LIBS})

# 设置安装规则
install(TARGETS myprogram DESTINATION bin)
install(FILES README.md DESTINATION doc)

```

#### VSCode

`notepad`、`vim`、`sublime`等编辑器虽然也很好用，但为了编写调试大型程序方便，一般用`VSCode`比较多。它不仅有常用编辑器的功能，更有一套插件系统。比如C++开发时一般常用的插件有：

```bash
C/C++
CMake
Code Runner
Doxygen Documentation Generator
Markdown
Git Graph
SSH Tools
vscode-proto3
WSL
ROS
CodeGeeX

```

目前我常用的配置文件`setting.json`如下：

```bash
{
    "workbench.colorTheme": "Visual Studio 2017 Dark - C++",
    "editor.fontSize": 18,
    "security.workspace.trust.untrustedFiles": "open",
    // 自定义配置
    "doxdocgen.generic.authorEmail": "xxx@xxx.org",
    "doxdocgen.generic.authorName": "xxx",
    "doxdocgen.file.versionTag": "@version 1.0",
    "doxdocgen.file.copyrightTag": [
        "@copyright Copyright (c) {year} xxx.cn. All rights reserved."
    ],
    "doxdocgen.generic.order": [
        "brief",
        "tparam",
        "param",
        "return"
    ],
    "doxdocgen.generic.returnTemplate": "@return {type} ",
    "doxdocgen.generic.splitCasingSmartText": true,
    "explorer.confirmDelete": false,
    "files.eol": "\n",
    "git.enableSmartCommit": true,
    "Codegeex.Privacy": false
}

```

### 😆3. 基础语法示例

#### 第一个C++程序

```cpp
#include <iostream>
using namespace std;
 
// main() 是程序开始执行的地方
int main()
{
   cout << "Hello World"; // 输出 Hello World
   return 0;
}

```

可以试着在Linux系统中用g++、make、CMake试着编译运行一下这个程序。

#### 标识符和关键字

标识符是用来标识变量、函数、类、模块或任何其他用户需自定义项目的名称，以字母或下划线开始，不能有标点符号（如`value setValue ClassExample`）。

C++中的关键字不能用于用户自定义的标识符。

#### 数据类型

C++提供了7种基本数据类型，包含`bool char int float double void wchar_t`，宽字符型其实是`typedef short int wchar_t`。在基本类型的基础上，可以增加修饰符，包含`signed unsigned short long`。

此外，可以用`typedef`为一个已有的类型取一个新的名字，即`typedef type newname`，如`typedef int int32`。

enum是一种派生数据类型，是用户定义的一系列常量集合。当一种变量只有几种可能的情况时，可以将其定义为枚举类型。

```cpp
enum Color { red, green, blue };
Color c1 = red;

```

数据类型之间可以进行类型转换。C++ 中有四种类型转换：**静态转换、动态转换、常量转换和重新解释转换**。

```cpp
// 静态将int类型转换为float类型
int i = 10;
float f = static\_cast<float>(i);
std::cout << "f is " << f + 0.01 << std::endl;

```

#### 变量与常量

变量是用来存储数据的内存位置，在使用前需要先声明并指定数据类型。

常量是指在程序执行期间值不会改变的数据，可以使用`const`关键字声明（减少使用`#define`）。

C++类型限定符提供了变量的额外信息，用于在定义变量或函数时改变它们的默认行为的关键字，包含`const volatile restrict mutable static register`，可以理解为常量是一种特殊的变量。

#### 运算符

算术运算符包含`加(+), 减(-), 乘(*), 除(/), 取余(%), 自增(++), 自减(--)`等。

关系运算符包含`等于(==), 不等于(!=), 大于(>), 小于(<), 大于等于(>=), 小于等于(<=)`等。

逻辑运算符包含`与(&&), 或(||), 非(!)`等。

位运算符包含`按位与(&),按位或(|),异或(^),取反(~),二进制左移(<<),二进制右移(>>)`等。

此外，还有一些特殊的运算符，如`条件运算符、逗号运算符、成员运算符、指针运算符等`。

```cpp
int num1 = 10, num2 = 20, maxNum;
maxNum = (num1 > num2) ? num1 : num2;
cout << "较大的数是：" << maxNum << endl;

```

#### 控制流语句

```cpp
// 条件语句（if-else）
#include <iostream>
using namespace std;

int main() {
    int num = 10;

    if (num > 0) {
        cout << "num是正数" << endl;
    }
    else if (num < 0) {
        cout << "num是负数" << endl;
    }
    else {
        cout << "num是零" << endl;
    }

    return 0;
}

```

```cpp
// 循环语句（for）
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        cout << i << " ";
    }
    cout << endl;

    return 0;
}

```

```cpp
// 循环语句（while）
#include <iostream>
using namespace std;

int main() {
    int i = 1;
    while (i <= 5) {
        cout << i << " ";
        i++;
    }
    cout << endl;

    return 0;
}

```

```cpp
// 循环语句（do-while）
#include <iostream>
using namespace std;

int main() {
    int i = 1;
    do {
        cout << i << " ";
        i++;
    } while (i <= 5);
    cout << endl;

    return 0;
}

```

```cpp
// 跳转语句（break continue）
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        if (i == 3) {
            break;  // 跳出循环
            // continue; // 跳过本次循环，继续下一次循环
        }
        cout << i << " ";
    }
    cout << endl;

    return 0;
}

```

#### 函数

```cpp
// 求两数的最大值
int max(int num1, int num2) 
{
   int result;
 
   if (num1 > num2)
      result = num1;
   else
      result = num2;
 
   return result; 
}

```

#### 数组与字符串

```cpp
string str1 = "hello ";
string str2 = "world";
string str3;
int len ;

// 复制 str1 到 str3
str3 = str1;
std::cout << "str3 : " << str3 << std::endl;

// 连接 str1 和 str2
str3 = str1 + str2;
std::cout << "str1 + str2 : " << str3 << std::endl;

// 连接后，str3 的总长度
len = str3.size();
std::cout << "str3.size() : " << len << std::endl;

```

#### 类与对象

```cpp
#include <iostream>
using namespace std;

// 定义一个简单的学生类
class Student {
private:
    string name;
    int age;

public:
    // 构造函数
    Student(string studentName, int studentAge) {
        name = studentName;
        age = studentAge;
    }

    // 成员函数
    void display() {
        cout << "姓名：" << name << endl;
        cout << "年龄：" << age << endl;
    }
};

int main() {
    // 创建一个学生对象
    Student student1("小明", 18);

    // 调用成员函数显示学生信息
    student1.display();

    return 0;
}

```

#### 指针和引用

```cpp
#include <iostream>
using namespace std;

int main() {
    int num1 = 10, num2 = 20;
    int \*ptr1 = &num1;
    int \*&ptr2 = ptr1;  // 引用指针

    cout << "num1的值：" << num1 << endl;
    cout << "num2的值：" << num2 << endl;
    cout << "ptr1指向的值：" << \*ptr1 << endl;
    cout << "ptr2引用的值：" << \*ptr2 << endl;
    cout << endl;

    ptr2 = &num2;  // 将ptr2指向num2
    cout << "num1的值：" << num1 << endl;
    cout << "num2的值：" << num2 << endl;
    cout << "ptr1指向的值：" << \*ptr1 << endl;
    cout << "ptr2引用的值：" << \*ptr2 << endl;

    return 0;
}

```

#### 文件操作

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    // 写入文件
    ofstream outFile("example.txt", ios::out);
    if (outFile.is\_open()) {
        outFile << "Hello, World!" << endl;
        outFile << "This is an example file." << endl;
        outFile.close();
        cout << "文件写入成功！" << endl;
    }
    else {
        cout << "无法打开文件！" << endl;
    }

    // 读取文件
    ifstream inFile("example.txt", ios::in);
    if (inFile.is\_open()) {
        string line;
        while (getline(inFile, line)) {
            cout << line << endl;
        }
        inFile.close();
    }
    else {
        cout << "无法打开文件！" << endl;
    }

    return 0;
}

```

以上。