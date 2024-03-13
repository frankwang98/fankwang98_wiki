







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍LCM的通讯。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  🥞喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. LCM通讯介绍](#smirk1_LCM_7)
	+ [:blush:2. LCM安装](#blush2_LCM_27)
	+ [:satisfied:3. LCM案例](#satisfied3_LCM_85)




### 😏1. LCM通讯介绍


机器人通讯中有许多流行的通讯中间件，如百度Apollo的`Cyber RT`，ROS1中的`TCPROS/UDPROS`通信机制，ROS2中使用的`DDS`等等。下面介绍另一种通讯模块：


LCM通讯是指使用`Lightweight Communications and Marshalling (LCM)`框架进行消息传递和数据编排的通信方式。LCM框架旨在提供一种快速、轻量级和灵活的消息传递机制，用于在实时应用程序中传输和处理数据。


LCM提供了简单易用的API，用于在进程之间发送和接收消息，并支持数据编排和序列化。开发人员可以使用简单的消息描述语言`（MDL）`来定义消息的结构和数据类型，且支持多种语言`（C/C++，C#，Java，Lua，MATLAB，Python）`。


LCM的一个关键特点是其高效性。它使用二进制协议进行消息传输，从而降低了处理开销并减少了网络带宽的使用。此外，LCM的模块化设计使其易于与现有代码库集成，因此广泛应用于机器人技术、航空航天等领域。


在机器人和自动驾驶系统中，LCM可以作为ROS的替代品，用于完成进程间、设备间的通讯。


LCM具有如下特性：



> 
> 1. 低延迟的进程间通信
> 2. 使用UDP组播的高效广播机制
> 3. 类型安全的消息编排
> 4. 用户友好的记录和回放工具
> 5. 没有集中的 "数据库 "或 “枢纽”–节点间直接通讯
> 6. 没有守护进程
> 7. 极少的依赖
> 
> 
> 


### 😊2. LCM安装


github地址：`https://github.com/lcm-proj/lcm/releases`


apt安装：`sudo apt install liblcm-dev`


源码安装：这里我选择`1.4.0`


安装依赖：`sudo apt-get install build-essential autoconf automake autopoint libglib2.0-dev libtool openjdk-8-jdk python-dev`


若安装了多个版本的JAVA，使用以下命令切换到version 8：`sudo update-alternatives --config java`


编译安装：



```
mkdir build
cd build
cmake ..
make
sudo make install

```

配置动态库链接地址：



```
export LCM\_INSTALL\_DIR=/usr/local/lib
sudo sh -c "echo $LCM\_INSTALL\_DIR > /etc/ld.so.conf.d/lcm.conf"
sudo ldconfig

```

定义通讯数据结构（使用lcm文件，c++为例），创建`example_t.lcm`：



```
package exlcm;
struct example_t
{
    int64_t  timestamp;
    double   position[3];
    double   orientation[4]; 
    int32_t  num_ranges;
    int16_t  ranges[num_ranges];
    string   name;
    boolean  enabled;
}
struct nihao_t
{
    int64_t  timest2amp;
    string   name1;
    boolean  enabl1ed;
}

```

执行命令生成头文件：`lcm-gen -x example_t.lcm`（执行lcm-gen后会为每一个结构体生成一个头文件。）


lcm常用命令：



```
lcm-logger //记录log
lcm-spy //查看当前数据
lcm-logplayer, lcm-logplayer-gui //log回放

```

### 😆3. LCM案例


发送代码示例：



```
// file: send\_message.cpp
//
// LCM example program.
//
// compile with:
// $ g++ -o send\_message send\_message.cpp -llcm
//
// On a system with pkg-config, you can also use:
// $ g++ -o send\_message send\_message.cpp `pkg-config --cflags --libs lcm`

#include <lcm/lcm-cpp.hpp>

#include "exlcm/example\_t.hpp"

int main(int argc, char \*\*argv)
{
    lcm::LCM lcm;
    if (!lcm.good())
        return 1;

    exlcm::example_t my_data;
    my_data.timestamp = 0;

    my_data.position[0] = 1;
    my_data.position[1] = 2;
    my_data.position[2] = 3;

    my_data.orientation[0] = 1;
    my_data.orientation[1] = 0;
    my_data.orientation[2] = 0;
    my_data.orientation[3] = 0;

    my_data.num_ranges = 15;
    my_data.ranges.resize(my_data.num_ranges);
    for (int i = 0; i < my_data.num_ranges; i++)
        my_data.ranges[i] = i;

    my_data.name = "example string";
    my_data.enabled = true;

    lcm.publish("EXAMPLE", &my_data);

    return 0;
}

```

编译：`g++ -o send_message send_message.cpp -llcm`


接收代码示例：



```
// file: listener.cpp
//
// LCM example program.
//
// compile with:
// $ gcc -o listener listener.cpp -llcm
//
// On a system with pkg-config, you can also use:
// $ gcc -o listener listener.cpp `pkg-config --cflags --libs lcm`

#include <stdio.h>

#include <lcm/lcm-cpp.hpp>
#include "exlcm/example\_t.hpp"

class Handler {
  public:
    ~Handler() {}
    void handleMessage(const lcm::ReceiveBuffer \*rbuf, const std::string &chan,
                       const exlcm::example_t \*msg)
    {
        int i;
        printf("Received message on channel %s :n", chan.c\_str());
        printf(" timestamp = %lld\\n", (long long) msg->timestamp);
        printf(" position = (%f, %f, %f)\\n", msg->position[0], msg->position[1],
               msg->position[2]);
        printf(" orientation = (%f, %f, %f, %f)\\n", msg->orientation[0], msg->orientation[1],
               msg->orientation[2], msg->orientation[3]);
        printf(" ranges:");
        for (i = 0; i < msg->num_ranges; i++)
            printf(" %d", msg->ranges[i]);
        printf("\\n");
        printf(" name = '%s'\\n", msg->name.c\_str());
        printf(" enabled = %d\\n", msg->enabled);
    }
};

int main(int argc, char \*\*argv)
{
    lcm::LCM lcm;

    if (!lcm.good())
        return 1;

    Handler handlerObject;
    lcm.subscribe("EXAMPLE", &Handler::handleMessage, &handlerObject);

    while (0 == lcm.handle()) {
        // Do nothing
    }

    return 0;
}

```

编译：`g++ -o listener listener.cpp -llcm`


log数据解析：



```
// file: read\_log.cpp
//
// LCM example program. Demonstrates how to read and decode messages directly
// from a log file in C++. It is also possible to use the log file provider --
// see the documentation on the LCM class for details on that method.
//
// compile with:
// $ g++ -o read\_log read\_log.cpp -llcm
//
// On a system with pkg-config, you can also use:
// $ g++ -o read\_log read\_log.cpp `pkg-config --cflags --libs lcm`

#include <stdio.h>

#include <lcm/lcm-cpp.hpp>

#include "exlcm/example\_t.hpp"

int main(int argc, char \*\* argv)
{
    if(argc < 2) {
        fprintf(stderr, "usage: read\_log <logfile>\n");
        return 1;
    }

    // Open the log file.
    lcm::LogFile log(argv[1], "r");
    if(!log.good()) {
        perror("LogFile");
        fprintf(stderr, "couldn't open log file %s\n", argv[1]);
        return 1;
    }

    while(1) {
        // Read a log event.
        const lcm::LogEvent \*event = log.readNextEvent();
        if(!event)
            break;

        // Only process messages on the EXAMPLE channel.
        if(event->channel != "EXAMPLE")
            continue;

        // Try to decode the message.
        exlcm::example_t msg;
        if(msg.decode(event->data, 0, event->datalen) != event->datalen)
            continue;

        // Decode success! Print out the message contents.
        printf("Message:\n");
        printf(" timestamp = %lld\n", (long long)msg.timestamp);
        printf(" position = (%f, %f, %f)\n",
                msg.position[0], msg.position[1], msg.position[2]);
        printf(" orientation = (%f, %f, %f, %f)\n",
                msg.orientation[0], msg.orientation[1], msg.orientation[2],
                msg.orientation[3]);
        printf(" ranges:");
        for(int i = 0; i < msg.num_ranges; i++)
            printf(" %d", msg.ranges[i]);
        printf("\n");
        printf(" name = '%s'\n", msg.name.c\_str());
        printf(" enabled = %d\n", msg.enabled);
    }

    // Log file is closed automatically when the log variable goes out of
    // scope.

    printf("done\n");
    return 0;
}

```

编译：`g++ -o read_log read_log.cpp -llcm`


参考：



```
https://mp.weixin.qq.com/s/P3TyCd7jYYd6a-EmSo8hAw
https://zhuanlan.zhihu.com/p/262727902

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





