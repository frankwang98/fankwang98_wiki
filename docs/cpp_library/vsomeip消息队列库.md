> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍vsomeip的使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. vsomeip介绍

`VSOMEIP` (Vehicle Service Oriented Architecture Message Exchange Protocol) 是一种基于 IP 网络的现代化汽车通信协议。它被设计用于在汽车中实现各种服务的高效通信，例如远程诊断、软件更新、车辆故障检测等。

VSOMEIP 不仅适用于汽车内部的通信，而且还可以让车辆与外部系统进行安全稳定的双向通信，如车载娱乐系统、智能交通系统等。它采用了现代化的网络技术和开放式架构，使得多个系统可以轻松地集成和交互，从而实现更高效的车辆连接和数据共享。

VSOMEIP 还提供了灵活的配置选项和可扩展性功能，使得开发人员可以根据需要快速构建自定义的汽车应用程序和服务。同时也支持多种编程语言，如 C ++、Java 和 Python 等，方便不同类型的开发人员使用。

### 😊2. vsomeip安装

github地址：`https://github.com/COVESA/vsomeip`

#### 安装依赖

```bash
sudo apt-get install libboost-system-dev libboost-thread-dev libboost-log-dev
sudo apt-get install asciidoc source-highlight doxygen graphviz

```

#### 安装google benchmark基准测试工具

安装完benchmark后，gtest会自动安装。

```bash
git clone https://github.com/google/benchmark.git
cd benchmark
cmake -E make_directory "build"
cmake -E chdir "build" cmake -DBENCHMARK\_DOWNLOAD\_DEPENDENCIES=on -DCMAKE\_BUILD\_TYPE=Release ../
cmake --build "build" --config Release
cmake -E chdir "build" ctest --build-config Release  #进行安装测试
sudo cmake --build "build" --config Release --target install  #在全局安装google benchmark

```

#### 编译安装vsomeip

```bash
mkdir build
cd build
cmake ..
make
sudo make instal
sudo ldconfig

```

### 😆3. vsomeip入门案例

进入`vsomeip/examples/hello_world`，编译安装：

```bash
mkdir build && cd build
cmake ..
make

```

创建server.sh

```bash
#!/bin/bash

env VSOMEIP\_CONFIGURATION=../helloworld-local.json \
VSOMEIP\_APPLICATION\_NAME=hello_world_service \
./hello_world_service

```

创建client.sh

```bash
#!/bin/bash

env VSOMEIP\_CONFIGURATION=../helloworld-local.json \
VSOMEIP\_APPLICATION\_NAME=hello_world_client \
./hello_world_client

```

参考链接：

```bash
https://zhuanlan.zhihu.com/p/405534988
https://zhuanlan.zhihu.com/p/545016054
http://t.csdn.cn/YKezX
http://t.csdn.cn/K1Khw
http://t.csdn.cn/HoGm2

```

以上。