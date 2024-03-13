### 😏1. C++介绍

C++官网：`https://isocpp.org/`

cppreference：`http://cppreference.com/`

cplusplus：`https://cplusplus.com/`

[C++ standard style卡内基梅隆大学](https://users.ece.cmu.edu/~eno/coding/CppCodingStandard.html)

[STL tutorial](https://github.com/wuye9036/CppTemplateTutorial)

[awesome-cpp](https://github.com/fffaraz/awesome-cpp)

#### 官方语言

**`C++` 是一种通用的编程语言，具有高效的特性，适用于开发各种类型的软件和系统**。C++ 是一种高级语言，它是由 Bjarne Stroustrup 于 1979 年在贝尔实验室开始设计开发的。它是 C 语言的一个超集（*即任何合法的 C 程序都是合法的 C++ 程序*），可以使用 C 语言的所有特性和库，同时也引入了许多新的特性，例如**类、继承、多态**等面向对象编程的概念，以及**泛型编程、异常处理、STL** 等高级特性。

与 C 语言相比，C++ 更适合开发大型项目和复杂的系统。它具有严格的类型检查和内存管理，能够提高程序的可靠性和安全性。同时，C++ 也具备高效和灵活性的优势，支持直接操作底层硬件和编写高性能代码。这些优点使得 C++ 成为广泛使用的编程语言，被应用于各个领域，**如操作系统、嵌入式、数据库、游戏开发、音视频传输、图像处理、金融和科学计算等**。

除了标准 C++ 语言的基础特性外，C++ 标准库（`STL`）也提供了丰富的数据结构和算法库，可用于开发各种类型的应用程序。此外，C++ 还有许多扩展库和框架，如 `Boost`、`Qt`、`OpenCV` 等，可以扩展其功能和应用范围。

#### 组成

* 核心语法：编程语言通用模块，如输入输出、常量变量、数据类型等
* 标准库：库中提供了大量函数接口，可用于操作字符串、文件等
* 标准模板库STL：提供了许多数据类型操作的函数接口

#### 特性

C++ 支持面向对象的程序设计，包括面向对象开发的四大特性：

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

make构建用到的是`makefile`文件。makefile用于描述软件项目中的源代码文件如何编译和链接成可执行文件、库文件或其他目标文件，提供了一种便捷且灵活的方式来管理和构建项目。

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

```sh
.PHONY: all clean  
all:hello  
hello:main.o sum.o  
	gcc -o hello main.o sum.o  
main.o:main.c  
	gcc -c main.c  
sum.o:sum.c  
	gcc -c sum.c  
clean:  
	rm -f main.o sum.o hello  
```

#### CMake

CMake构建用到的是`CMakeLists.txt`文件。

CMake 是一个跨平台的开源构建工具，用于自动化地生成与平台特定编译器和构建系统无关的构建脚本和配置文件。

```bash
# 设置 CMake 最低版本要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本号
project(MyProject VERSION 1.0)

# 设置编译选项
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 添加依赖库
find_package(OpenCV REQUIRED)

# 添加头文件搜索路径
include_directories(include)

# 添加可执行文件
add_executable(myprogram main.cpp utils.cpp)
target_link_libraries(myprogram ${OpenCV_LIBS})

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
# 且最好设置git的换行符默认为LF
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
   cout << "Hello World\n"; // 输出 Hello World
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
float f = static_cast<float>(i);
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
    int *ptr1 = &num1;
    int *&ptr2 = ptr1;  // 引用指针的用法

    cout << "num1的值：" << num1 << endl;
    cout << "num2的值：" << num2 << endl;
    cout << "ptr1指向的值：" << *ptr1 << endl;
    cout << "ptr2引用的值：" << *ptr2 << endl;
    cout << endl;

    ptr2 = &num2;  // 将ptr2指向num2，ptr1同步改变
    cout << "num1的值：" << num1 << endl;
    cout << "num2的值：" << num2 << endl;
    cout << "ptr1指向的值：" << *ptr1 << endl;
    cout << "ptr2引用的值：" << *ptr2 << endl;

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
    if (outFile.is_open()) {
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
    if (inFile.is_open()) {
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

#### C++类&对象
类用于指定对象的形式，它包含了数据表示法和用于处理数据的方法。类中的数据和方法称为类的成员。函数在一个类中被称为类的成员。

通过一个实例来理解类的概念：

```cpp
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      // 成员函数声明
      double get(void);
      void set( double len, double bre, double hei );
};

// 成员函数定义
double Box::get(void)
{
    return length * breadth * height;
}
 
void Box::set( double len, double bre, double hei)
{
    length = len;
    breadth = bre;
    height = hei;
}

int main( )
{
   Box Box1;        // 声明 Box1，类型为 Box
   Box Box2;        // 声明 Box2，类型为 Box
   Box Box3;        // 声明 Box3，类型为 Box
   double volume = 0.0;     // 用于存储体积
 
   // box 1 详述
   Box1.height = 5.0; 
   Box1.length = 6.0; 
   Box1.breadth = 7.0;
 
   // box 2 详述
   Box2.height = 10.0;
   Box2.length = 12.0;
   Box2.breadth = 13.0;
 
   // box 1 的体积
   volume = Box1.height * Box1.length * Box1.breadth;
   cout << "Box1 的体积：" << volume <<endl;
 
   // box 2 的体积
   volume = Box2.height * Box2.length * Box2.breadth;
   cout << "Box2 的体积：" << volume <<endl;
 
   // box 3 详述
   Box3.set(16.0, 8.0, 12.0); 
   volume = Box3.get(); 
   cout << "Box3 的体积：" << volume <<endl;
   return 0;
}
```
需要注意的是，私有的成员和受保护的成员不能使用直接成员访问运算符 (.) 来直接访问。

继承示例：
```cpp
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   return 0;
}
```

#### C++运算符重载和函数重载

C++ 允许在同一作用域中的某个函数和运算符指定多个定义，分别称为函数重载和运算符重载。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个重载函数或重载运算符时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为重载决策。

函数重载示例：
```cpp
#include <iostream>
using namespace std;
 
class printData
{
   public:
      void print(int i) {
        cout << "整数为: " << i << endl;
      }
 
      void print(double  f) {
        cout << "浮点数为: " << f << endl;
      }
 
      void print(char c[]) {
        cout << "字符串为: " << c << endl;
      }
};
 
int main(void)
{
   printData pd;
 
   // 输出整数
   pd.print(5);
   // 输出浮点数
   pd.print(500.263);
   // 输出字符串
   char c[] = "Hello C++";
   pd.print(c);
 
   return 0;
}
```

运算符重载示例：
```cpp
#include <iostream>
using namespace std;
 
class Box
{
   public:
      double getVolume(void)
      {
         return length * breadth * height;
      }
      void setLength( double len )
      {
          length = len;
      }
 
      void setBreadth( double bre )
      {
          breadth = bre;
      }
 
      void setHeight( double hei )
      {
          height = hei;
      }
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}
```

#### C++多态&虚函数
多态按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。

C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。

虚函数 是在基类中使用关键字 virtual 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。

我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为动态链接，或后期绑定。

下面看一个实例：
```cpp

#include <iostream> 
using namespace std;

class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};

class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};

int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   
   return 0;
}
```

#### 类的设计策略
面向对象的系统可能会使用一个抽象基类为所有的外部应用程序提供一个适当的、通用的、标准化的接口。然后，派生类通过继承抽象基类，就把所有类似的操作都继承下来。

外部应用程序提供的功能（即公有函数）在抽象基类中是以纯虚函数的形式存在的。这些纯虚函数在相应的派生类中被实现。

这个架构也使得新的应用程序可以很容易地被添加到系统中，即使是在系统被定义之后依然可以如此。

#### C++异常处理
异常是程序在执行期间产生的问题。C++ 异常是指在程序运行时发生的特殊情况，比如尝试除以零的操作。

异常提供了一种转移程序控制权的方式。C++ 异常处理涉及到三个关键字：try、catch、throw。

throw: 当问题出现时，程序会抛出一个异常。这是通过使用 throw 关键字来完成的。
catch: 在您想要处理问题的地方，通过异常处理程序捕获异常。catch 关键字用于捕获异常。
try: try 块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 catch 块。

如果有一个块抛出一个异常，捕获异常的方法会使用 try 和 catch 关键字。
```cpp
try
{
   // 保护代码
}catch( ExceptionName e1 )
{
   // catch 块
}catch( ExceptionName e2 )
{
   // catch 块
}catch( ExceptionName eN )
{
   // catch 块
}
```

#### C++动态内存
了解动态内存在 C++ 中是如何工作的是成为一名合格的 C++ 程序员必不可少的。C++ 程序中的内存分为两个部分：

栈：在函数内部声明的所有变量都将占用栈内存。
堆：这是程序中未使用的内存，在程序运行时可用于动态分配内存。

在 C++ 中，您可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。这种运算符即 new 运算符。

如果您不再需要动态分配的内存空间，可以使用 delete 运算符，删除之前由 new 运算符分配的内存。

以上。