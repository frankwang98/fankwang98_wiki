








#### 文章目录


* + [1. Protobuf介绍](#1_Protobuf_1)
	+ [2. C++版源码安装](#2_C_27)
	+ [3. Protobuf应用](#3_Protobuf_68)
	+ [4. 其他](#4__247)




### 1. Protobuf介绍


Protocol buffer是一种语言无关、平台无关、可扩展的序列化结构数据的方法，它可用于（数据）通信协议、数据存储等。是谷歌的开源数据交换格式。


Protocol Buffers 是一种灵活，高效，自动化机制的结构数据序列化方法－可类比 XML，但是比 XML 更小（3-10倍）、更快（20-100倍）、更为简单。


你可以定义数据的结构，然后使用特殊生成的源代码轻松的在各种数据流中使用各种语言进行编写和读取结构数据。你甚至可以更新数据结构，而不破坏由旧数据结构编译的已部署程序。


简单来说，protobuf有以下特点：


* 语言无关、平台无关。即 ProtoBuf 支持 Java、C++、Python 等多种语言，支持多个平台
* 高效。即比 XML 更小（3 ~ 10倍）、更快（20 ~ 100倍）、更为简单
* 扩展性、兼容性好。你可以更新数据结构，而不影响和破坏原有的旧程序


其中，序列化是指将结构数据或对象转换成能够被存储和传输（例如网络传输）的格式，同时应当要保证这个序列化结果在之后（可能在另一个计算环境中）能够被重建回原来的结构数据或对象。


类比于 XML：这里主要指在数据通信和数据存储应用场景中序列化方面的类比，但其实 XML 作为一种扩展标记语言和 ProtoBuf 还是有着本质区别的。


如果只是简单使用，在ubuntu用apt安装就可以：



```
# 安装
sudo apt-get install libprotobuf-dev protobuf-compiler
# 卸载
sudo apt-get autoremove --purge libprotobuf-dev protobuf-compiler

```

### 2. C++版源码安装


![在这里插入图片描述](https://img-blog.csdnimg.cn/865a2672742a4b5abba9ccddc6ff1780.png)


protobuf是一种灵活高效的独立于语言平台的结构化数据表示方法。在通信协议和数据存储等领域中使用较多。如b站的弹幕传输，另外，车端软件的指令也可以用这种协议。


1.安装依赖工具



```
sudo apt-get install autoconf automake libtool curl make g++ unzip

```

2.下载源代码



```
https://github.com/protocolbuffers/protobuf/releases

```

选择自己需要的版本，这里用`protobuf-cpp-3.19.1`。


3.编译安装



```
cd protobuf
./autogen.sh
./configure
make
make check
sudo make install
sudo ldconfig

```

编译不报错即完成安装。


在终端输入`protoc`有help信息。



```
protoc –-version
pkg-config --cflags --libs protobuf

```

在Windows中可用`VS`或`MinGW`编译器安装使用。


### 3. Protobuf应用


1.编译proto文件



```
protoc test.proto --cpp\_out=./	# 精简版
protoc -I=$SRC\_DIR --cpp\_out=$DST\_DIR $SRC\_DIR/test.proto	# 完整版 

```

2.使用  
 生成cc和h文件，可在其他程序中调用。


官方语法文档：`https://developers.google.com/protocol-buffers/docs/proto3`


我们需要学会以下这几点：


* 如何在一个 .proto 文件中定义 message
* 如何使用 protocol buffer 编译器
* 如何使用 C++ protocol buffer 的 API 读写 message


首先，创建`.proto`文件，定义数据结构  
 例1: 在 xxx.proto 文件中定义 Example1 message



```
message Example1 {
    optional string stringVal = 1;
    optional bytes bytesVal = 2;
    message EmbeddedMessage {
        int32 int32Val = 1;
        string stringVal = 2;
    }
    optional EmbeddedMessage embeddedExample1 = 3;
    repeated int32 repeatedInt32Val = 4;
    repeated string repeatedStringVal = 5;
}

```

在上例中，我们定义了：


* 类型 string，名为 stringVal 的 optional 可选字段，字段编号为 1，此字段可出现 0 或 1 次
* 类型 bytes，名为 bytesVal 的 optional 可选字段，字段编号为 2，此字段可出现 0 或 1 次
* 类型 EmbeddedMessage（自定义的内嵌 message 类型），名为 embeddedExample1 的 optional 可选字段，字段编号为 3，此字段可出现 0 或 1 次
* 类型 int32，名为 repeatedInt32Val 的 repeated 可重复字段，字段编号为 4，此字段可出现 任意多次（包括 0）
* 类型 string，名为 repeatedStringVal 的 repeated 可重复字段，字段编号为 5，此字段可出现 任意多次（包括 0）


protobuf中常用的数据类型：



```
bool,		布尔类型
double,		64位浮点数
float,		32位浮点数
int32,		32位整数
int64,		64位整数
uint64,		64位无符号整数
sint32,		32位整数，处理负数效率更高
sint64,		64位整数，处理负数效率更高
string,		只能处理ASCII字符
bytes,		用于处理多字节的语言字符
enum,		枚举类型

```

然后，protoc 编译 .proto 文件生成读写接口


我们在 .proto 文件中定义了数据结构，这些数据结构是面向开发者和业务程序的，并不面向存储和传输。


当需要把这些数据进行存储或传输时，就需要将这些结构数据进行序列化、反序列化以及读写。那么如何实现呢？不用担心， ProtoBuf 将会为我们提供相应的接口代码。如何提供？答案就是通过 protoc 这个编译器。


可通过如下命令生成相应的接口代码：



```
// $SRC\_DIR: .proto 所在的源目录
// --cpp_out: 生成 c++ 代码
// $DST\_DIR: 生成代码的目标目录
// xxx.proto: 要针对哪个 proto 文件生成接口代码
protoc -I=$SRC\_DIR --cpp\_out=$DST\_DIR $SRC\_DIR/xxx.proto
如：protoc student.proto --cpp\_out=./

```

最后，调用接口实现序列化、反序列化以及读写：


针对第一步中例1定义的 message，我们可以调用第二步中生成的接口，实现测试代码如下：



```
#include <iostream>
#include <fstream>
#include <string>
#include "single\_length\_delimited\_all.pb.h"
 
int main() {
    Example1 example1;
    example1.set\_stringval("hello,world");
    example1.set\_bytesval("are you ok?");
 
    Example1_EmbeddedMessage \*embeddedExample2 = new Example1\_EmbeddedMessage();
 
    embeddedExample2->set\_int32val(1);
    embeddedExample2->set\_stringval("embeddedInfo");
    example1.set\_allocated\_embeddedexample1(embeddedExample2);
 
    example1.add\_repeatedint32val(2);
    example1.add\_repeatedint32val(3);
    example1.add\_repeatedstringval("repeated1");
    example1.add\_repeatedstringval("repeated2");
 
    std::string filename = "single\_length\_delimited\_all\_example1\_val\_result";
    std::fstream output(filename, std::ios::out | std::ios::trunc | std::ios::binary);
    if (!example1.SerializeToOstream(&output)) {
        std::cerr << "Failed to write example1." << std::endl;
        exit(-1);
    }
 
    return 0;
}

```

编译命令如下：



```
g++ main.cpp  student.pb.cc `pkg-config --cflags --libs protobuf` -lpthread -std=c++11

```

生成动态链接库：



```
g++ -c main.cpp & g++ -c student.pb.cc -std=c++11
g++ ./main.o student.pb.o -o main_test.out `pkg-config --cflags --libs protobuf`

```

`CMakeList.txt`编译：



```
cmake_minimum_required(VERSION 3.5)

project(agv_cmake LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS "${CMAKE\_CXX\_FLAGS} -lprotobuf -std=c++11 -lpthread ")  
#设置编译带的参数

find_package(Protobuf REQUIRED)  #查找protobuf包

include_directories(${Protobuf\_INCLUDE\_DIRS})  #添加头文件
include_directories(${CMAKE\_CURRENT\_BINARY\_DIR})  #添加头文件

include_directories(/usr/local/include)  
link_directories(/usr/local/lib)    #链接库文件

# Depend on custom defined lib

add_executable(agv_cmake main.cpp
                         nebulalink.servercwaveii.pb.cc)
target_link_libraries(agv_cmake  ${PROTOBUF\_LIBRARIES})  #一定要连接库文件！！！！
qmake编译：
TEMPLATE = app
CONFIG += console c++11
CONFIG -= app_bundle
CONFIG -= qt

INCLUDEPATH += \
    /opt/ros/kinetic/include \  #ros的头文件
    /usr/local/include \        #proto所在头文件
    /usr/include/google/protobuf \

LIBS += -L/opt/ros/kinetic/lib \
        -L/usr/lib \     #proto所在库
        -L/usr/local/lib \  #proto所在库
        -L/usr/lib/x86_64-linux-gnu \
        -lpthread \
        -lprotobuf \   #proto所在库
        -lroscpp -lrospack -lrosconsole -lroscpp\_serialization \
        -lrostime -lroslib -lrosconsole\_log4cxx -ltf -ltf2 -ltf2\_ros -lactionlib \
        -ldynamic\_reconfigure\_config\_init\_mutex -limage\_transport -lcv\_bridge -lclass\_loader \
        -lboost\_system -lboost\_filesystem -lboost\_thread -lboost\_timer -lboost\_date\_time \

SOURCES += \
        main.cpp \
        student.pb.cc \

```

### 4. 其他


官方文档以及网上很多文章提到 `ProtoBuf` 可类比 `XML` 或 `JSON`。


那么 ProtoBuf 是否就等同于 XML 和 JSON 呢，它们是否具有完全相同的应用场景呢？


个人认为如果要将 ProtoBuf、XML、JSON 三者放到一起去比较，应该区分两个维度。一个是数据结构化，一个是数据序列化。这里的数据结构化主要面向开发或业务层面，数据序列化面向通信或存储层面，当然数据序列化也需要“结构”和“格式”，所以这两者之间的区别主要在于面向领域和场景不同，一般要求和侧重点也会有所不同。数据结构化侧重人类可读性甚至有时会强调语义表达能力，而数据序列化侧重效率和压缩。


从这两个维度，我们可以做出下面的一些思考。


XML 作为一种扩展标记语言，JSON 作为源于 JS 的数据格式，都具有数据结构化的能力。  
 例如 XML 可以衍生出 HTML （虽然 HTML 早于 XML，但从概念上讲，HTML 只是预定义标签的 XML），HTML 的作用是标记和表达万维网中资源的结构，以便浏览器更好的展示万维网资源，同时也要尽可能保证其人类可读以便开发人员进行编辑，这就是面向业务或开发层面的数据结构化。  
 再如 XML 还可衍生出 RDF/RDFS，进一步表达语义网中资源的关系和语义，同样它强调数据结构化的能力和人类可读。


JSON 也是同理，在很多场合更多的是体现了数据结构化的能力，例如作为交互接口的数据结构的表达。在 MongoDB 中采用 JSON 作为查询语句，也是在发挥其数据结构化的能力。


当然，JSON、XML 同样也可以直接被用来数据序列化，实际上很多时候它们也是这么被使用的，例如直接采用 JSON、XML 进行网络通信传输，此时 JSON、XML 就成了一种序列化格式，它发挥了数据序列化的能力。但是经常这么被使用，不代表这么做就是合理。实际将 JSON、XML 直接作用数据序列化通常并不是最优选择，因为它们在速度、效率、空间上并不是最优。换句话说它们更适合数据结构化而非数据序列化。


扯完 XML 和 JSON，我们来看看 ProtoBuf，同样的 ProtoBuf 也具有数据结构化的能力，其实也就是上面介绍的 message 定义。我们能够在 .proto 文件中，通过 message、import、内嵌 message 等语法来实现数据结构化，但是很容易能够看出，ProtoBuf 在数据结构化方面和 XML、JSON 相差较大，人类可读性较差，不适合上面提到的 XML、JSON 的一些应用场景。


但是如果从数据序列化的角度你会发现 ProtoBuf 有着明显的优势，效率、速度、空间几乎全面占优，看完后面的 ProtoBuf 编码的文章，你更会了解 ProtoBuf 是如何极尽所能的压榨每一寸空间和性能，而其中的编码原理正是 ProtoBuf 的关键所在，message 的表达能力并不是 ProtoBuf 最关键的重点。所以可以看出 ProtoBuf 重点侧重于数据序列化而非数据结构化。


以上。





