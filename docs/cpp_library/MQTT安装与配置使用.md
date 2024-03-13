







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍MQTT安装与配置使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. MQTT介绍](#smirk1_MQTT_7)
	+ [:blush:2. 环境安装](#blush2__37)
	+ [:satisfied:3. Mosquitto示例](#satisfied3_Mosquitto_76)




### 😏1. MQTT介绍


官网：`https://mqtt.org/`


`MQTT`是一个基于**客户端-服务器**的**消息发布/订阅传输协议**。


`MQTT` (Message Queuing Telemetry Transport) 是一种轻量级的消息传输协议，通常用于物联网设备和应用程序之间进行通信。它是基于**发布/订阅**模式设计的，其中消息发布者将消息发布到特定主题（Topic），然后订阅该主题的客户端将收到这些消息。MQTT 特别适合在网络带宽有限的情况下进行通信，因为它使用的数据包非常小。此外，它还提供了多种 `QoS` (Quality of Service) 级别来确保消息的可靠性和有效性。


`MQTT` 协议具有以下特点：



> 
> 1.轻量级：相对于 HTTP 等协议，MQTT 的数据包非常小，因此能够以较低的网络带宽进行通信。  
>  2.发布/订阅模式：通过订阅一个特定的主题，客户端能够接收和处理与该主题相关的所有消息。  
>  3.多种 QoS 级别：MQTT 提供了三种不同的 QoS级别，以满足不同场景下的需求。  
>  4.可扩展性：MQTT 的设计使得它能够方便地扩展到大规模系统中，并支持多种不同的连接方式，例如`TCP、WebSocket` 等。
> 
> 
> 


`MQTT`数据包结构如下：



> 
> 固定头（Fixed header），存在于所有MQTT数据包中，表示数据包类型及数据包的分组类标识；  
>  可变头（Variable header），存在于部分MQTT数据包中，数据包类型决定了可变头是否存在及其具体内容；  
>  消息体（Payload），存在于部分MQTT数据包中，表示客户端收到的具体内容；
> 
> 
> 


`MQTT` 支持三种不同级别的服务质量（Quality of Service，QoS），分别为 `QoS0、QoS1 和 QoS2`。



> 
> QoS0：**最多发送一次**，消息发送者只会将消息发布出去，但是并不保证接收者是否成功接收到该消息。这是最低级别的服务质量，也是最简单和最快速的传输方式。  
>  QoS1：**至少发送一次**，消息发送者确保至少将消息传输给接收者一次。如果接收者没有确认消息或者确认消息失败，则消息发送者会尝试重新发送，直到接收者成功地接收到消息为止。  
>  QoS2：**恰好发送一次**，消息发送者确保接收者恰好只能收到一次消息。在该级别下，消息发送者和接收者会进行两轮握手确认，以保证消息的可靠性和有效性。
> 
> 
> 


选择哪种服务质量级别取决于应用场景和对通信安全性的要求。需要注意的是，在选择高级别的服务质量时，会增加通信延迟和网络带宽的消耗。


目前mqtt的代理平台有：`Mosquitto、VerneMQ、EMQTT、Eclipse Paho`等。


### 😊2. 环境安装


Github：`https://github.com/eclipse/mosquitto`


下面在Ubuntu安装`Mosquitto`来体验mqtt的消息传递过程：



```
sudo apt-get install mosquitto	# 服务端
sudo apt install mosquitto-clients	# 客户端
sudo apt-get install libmosquitto-dev # 开发依赖包
g++ -o main main.cpp -lmosquitto && ./main # g++

```

启动/关闭mqtt服务：



```
mosquitto -v	# 启用所有日志记录类型
# 启动和关闭服务
sudo service mosquitto start
sudo service mosquitto stop
# 查看运行状态
sudo systemctl status mosquitto
# 查看帮助
mosquitto --help
#查看运行进程号：
ps -aux | grep mosquitto
#执行命令杀死进程：
kill -9 进程号

```

MQTT消息传输测试：



```
1、启动代理服务：mosquitto -v  # -v 详细模式 打印调试信息（启动一次就好）
2、订阅主题：mosquitto_sub -t 'test/topic' -v
3、发布内容：mosquitto_pub -t 'test/topic' -m 'hello world'

```

MQTT在线测试工具：`https://mqttx.app/zh`


### 😆3. Mosquitto示例


MQTT发布订阅示例：



```
#include <mosquitto.h>
#include <iostream>
#include <cstring>

// MQTT消息回调函数
void message\_callback(struct mosquitto\* mosq, void\* userdata, const struct mosquitto\_message\* message)
{
    std::cout << "Received message: " << (char\*)message->payload << std::endl;
}

int main()
{
    struct mosquitto\* mosq = nullptr;
    const char\* host = "localhost";
    int port = 1883;
    const char\* topic = "test/topic";
    const char\* message = "Hello, MQTT!";
    int keepalive = 60;
    bool clean_session = true;

    // 初始化mosquitto库
    mosquitto\_lib\_init();

    // 创建MQTT客户端
    mosq = mosquitto\_new(NULL, clean_session, NULL);
    if (!mosq)
    {
        std::cerr << "Failed to create MQTT client" << std::endl;
        return -1;
    }

    // 设置消息回调函数
    mosquitto\_message\_callback\_set(mosq, message_callback);

    // 连接到MQTT代理
    if (mosquitto\_connect(mosq, host, port, keepalive) != MOSQ_ERR_SUCCESS)
    {
        std::cerr << "MQTT connection failed" << std::endl;
        mosquitto\_destroy(mosq);
        mosquitto\_lib\_cleanup();
        return -1;
    }

    // 订阅主题
    if (mosquitto\_subscribe(mosq, NULL, topic, 0) != MOSQ_ERR_SUCCESS)
    {
        std::cerr << "Failed to subscribe to topic" << std::endl;
        mosquitto\_destroy(mosq);
        mosquitto\_lib\_cleanup();
        return -1;
    }

    // 发布消息
    if (mosquitto\_publish(mosq, NULL, topic, strlen(message), message, 0, false) != MOSQ_ERR_SUCCESS)
    {
        std::cerr << "Failed to publish message" << std::endl;
        mosquitto\_destroy(mosq);
        mosquitto\_lib\_cleanup();
        return -1;
    }

    // 循环处理MQTT消息
    while (mosquitto\_loop(mosq, -1, 1) == MOSQ_ERR_SUCCESS) {}

    // 断开MQTT连接和清理资源
    mosquitto\_disconnect(mosq);
    mosquitto\_destroy(mosq);
    mosquitto\_lib\_cleanup();

    return 0;
}

```

MQTT发布订阅C++风格，封装为函数编译调用示例：



```
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <mosquitto.h>
#include <string>

const std::string MQTT_BROKER_ADDRESS = "localhost";  // Mosquitto broker 的地址
const int MQTT_BROKER_PORT = 1883;  // Mosquitto broker 的端口号

// Mosquitto库初始化
void mosquittoInit(struct mosquitto\*& mosq) {
    mosquitto\_lib\_init();
    mosq = mosquitto\_new("mosq\_cpp\_example", true, nullptr);
    if (!mosq) {
        std::cerr << "Error: Unable to initialize Mosquitto library." << std::endl;
        exit(EXIT_FAILURE);
    }
}

// 连接到Mosquitto broker
void mosquittoConnect(struct mosquitto\* mosq) {
    int ret = mosquitto\_connect(mosq, MQTT_BROKER_ADDRESS.c\_str(), MQTT_BROKER_PORT, 60);
    if (ret != MOSQ_ERR_SUCCESS) {
        std::cerr << "Error: Unable to connect to Mosquitto broker. Return code: " << ret << std::endl;
        exit(EXIT_FAILURE);
    }
}

// 发布消息到指定主题
void mosquittoPublish(struct mosquitto\* mosq, const std::string& topic, const std::string& message) {
    int ret = mosquitto\_publish(mosq, nullptr, topic.c\_str(), message.size(), message.c\_str(), 0, false);
    if (ret != MOSQ_ERR_SUCCESS) {
        std::cerr << "Error: Unable to publish message. Return code: " << ret << std::endl;
    }
}

// 订阅指定主题
void mosquittoSubscribe(struct mosquitto\* mosq, const std::string& topic) {
    int ret = mosquitto\_subscribe(mosq, nullptr, topic.c\_str(), 0);
    if (ret != MOSQ_ERR_SUCCESS) {
        std::cerr << "Error: Unable to subscribe to topic. Return code: " << ret << std::endl;
    }
}

// 回调函数处理接收到的消息
void onMessage(struct mosquitto\* mosq, void\* userdata, const struct mosquitto\_message\* message) {
    std::string receivedMessage((char\*)message->payload, message->payloadlen); // char\* -> string
    std::cout << "Received message on topic: " << message->topic << ", Message: " << receivedMessage << std::endl;
}

int main() {
    struct mosquitto\* mosq = nullptr;

    mosquittoInit(mosq);
    mosquittoConnect(mosq);

    // 订阅主题
    mosquittoSubscribe(mosq, "test");

    // 发布主题
    while (true) {
        mosquittoPublish(mosq, "test", "Hello, Mosquitto!");
        std::cout << "Published message." << std::endl;
        usleep(1000 \* 1000);
    }
    
    // 设置消息接收回调函数
    mosquitto\_message\_callback\_set(mosq, onMessage);

    // 循环处理消息
    mosquitto\_loop\_forever(mosq, -1, 1);

    // 断开连接并清理资源
    mosquitto\_disconnect(mosq);
    mosquitto\_destroy(mosq);
    mosquitto\_lib\_cleanup();

    return 0;
}

```

基于MQTT的机器人项目示例：


项目Github地址：`https://github.com/horo2016/easyMQOS`


这个项目用MQTT代替我们常用的ROS，来对机器人的各个节点进行实现，webjs网页来控制，可以学习。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





