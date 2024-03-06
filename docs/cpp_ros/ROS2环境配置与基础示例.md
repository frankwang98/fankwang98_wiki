> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ros2环境安装与基础入门。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. ros2介绍

ROS 2 (Robot Operating System 2)是一个开源的机器人操作系统，它是ROS（Robot Operating System）的下一代版本。它提供了一系列工具、库和约定，用于构建机器人应用程序。与ROS 1相比，ROS 2具有更强大的功能，更好的性能和更好的可靠性。

ROS 2采用分布式消息传递机制，可以在不同的计算机上进行通信，并支持多种编程语言，包括C++、Python、Java等。ROS 2还提供了更好的安全性和实时性，使其适用于更广泛的机器人应用场景。

ROS 2的核心组件包括：

> rclcpp：ROS客户端库，在C++中使用。 rclpy：ROS客户端库，在Python中使用。  
> rosidl：服务接口定义语言，用于描述ROS消息和服务。 rmw：ROS中间件，用于管理节点之间的通信。  
> ros2cli：命令行界面工具，用于管理ROS 2系统。

ROS2的一大特点是集成了DDS，支持的DDS有：

> Fast RTPS：该实现基于eProsima的Fast RTPS库，是ROS 2默认的DDS实现。Fast RTPS是一个高性能、可靠的DDS实现，采用了快速序列化机制（Fast Buffers）和动态类型支持（DynamicTypes），支持多种平台和编程语言。  
> Cyclone DDS：该实现由ADLINK开发，是另一个高性能、开源的DDS实现。Cyclone DDS支持多种平台和编程语言，并提供了一些高级功能，如分布式安全和QoS配置。  
> RTI Connext DDS：该实现由Real-Time Innovations公司开发，是一个商业级别的DDS实现。RTI Connext DDS提供了广泛的功能和工具，如实时监测、故障诊断和网络优化等。

### 😊2. ros2安装

Ubuntu 18.04可以安装`ROS 2 Dashing Diademata`和`ROS 2 Eloquent Elusor`版本。建议使用`Eloquent`版本，因为它是最新的长期支持版本，并提供了更多的功能和改进。

小鱼的安装命令：`wget http://fishros.com/install -O fishros && . fishros`

根据需求选择对应的ros2版本即可。

### 😆3. ros2基础使用

示例测试：

```bash
# 发布订阅
ros2 run demo_nodes_cpp listener
ros2 run demo_nodes_cpp talker
# 小乌龟
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
# CLI
ros2 pkg list
ros2 node list
ros2 node info <node_name>
ros2 pkg create <package-name>  --build-type  {cmake,ament_cmake,ament_python}  --dependencies <依赖名字> # 创建功能包
ros2 bag record /topic_name
rviz2
gazebo

```

代码模板：`https://github.com/mikeferguson/ros2_cookbook`

国内参考：`https://fishros.com/d2lros2foxy/#/codebook/README`

---

cmake工程引用rclcpp示例：

创建main.cpp，写一个hello_world_cpp节点示例：


```cpp
#include "rclcpp/rclcpp.hpp"
#include <iostream>

int main(int argc, char** argv)
{
    rclcpp::init(argc, argv);
    std::cout << "Hello ROS2!" << std::endl;
    rclcpp::spin(std::make_shared<rclcpp::Node>("hello_world_cpp"));
    return 0;
}

```

创建CMakeLists.txt，引用rclcpp头文件和链接库：

```bash
cmake_minimum_required(VERSION 3.11)
project(main)

find_package(rclcpp REQUIRED)
add_executable(main main.cpp)
target_link_libraries(main rclcpp::rclcpp)

```

然后编译即可：

```bash
mkdir build && cd build
cmake ..
make
./main

```

---

安装colcon编译工具并测试案例：

```bash
# 安装编译工具
sudo apt-get install python3-colcon-common-extensions
# 下载源码
mkdir colcon_test && cd colcon_test
git clone https://ghproxy.com/https://github.com/ros2/examples src/examples -b eloquent
colcon build
# 运行自己编译的节点
source install/setup.bash
ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
source install/setup.bash
ros2 run examples_rclcpp_minimal_publisher publisher_member_function

```

运行如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/988d48834bb643baa906931d7effcddc.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/b6da332b404249b099e8e5dbb9d3f4ed.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/1a17933fcec84990b19243323f70a1a8.png)


### 😆4. ros2节点编写

#### 简单的节点

下面演示一个ROS2节点的创建：

```bash
# 新建节点目录
mkdir -p ros2_ws/src
# 创建功能包（ament_cmake编译，rclcpp依赖）
cd ros2_ws/src
ros2 pkg create name_of_pack --build-type ament_cmake --dependencies rclcpp
# 创建编写main.cpp
touch name_of_pack/src/main.cpp
# 修改（末尾追加）CMakeLists.txt
add_executable(RosNode src/main.cpp)
ament_target_dependencies(RosNode rclcpp)
install(TARGETS
  RosNode 
  DESTINATION lib/${PROJECT_NAME}
)
# 编译运行
colcon build
source install/setup.bash
ros2 run name_of_pack RosNode
# 测试
ros2 node list
ros2 node info /RosNode_2

```

```cpp
#include "rclcpp/rclcpp.hpp"

int main(int argc, char **argv)
{
    /* 初始化rclcpp */
    rclcpp::init(argc, argv);
    /* 创建节点 */
    auto node = std::make_shared<rclcpp::Node>("RosNode_2");
    /* 打印日志 */
    RCLCPP_INFO(node->get_logger(), "ros2节点已经启动.");
    /* 运行节点，并检测退出信号 Ctrl+C */
    rclcpp::spin(node);
    /* 停止运行 */
    rclcpp::shutdown();
    return 0;
}

```

#### 发布者和订阅者

下面创建一个发布者和订阅者：

```bash
# 新建目录
mkdir -p mytest_ws/src
cd mytest_ws/src
ros2 pkg create subscribe_and_publish --build-type ament_cmake --dependencies rclcpp
touch subscribe_and_publish/src/publisher.cpp
touch subscribe_and_publish/src/subscriber.cpp
# CMakeLists.txt
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
add_executable(publisher src/publisher.cpp)
ament_target_dependencies(publisher rclcpp std_msgs)
add_executable(subscribe1 src/subscriber.cpp)
ament_target_dependencies(subscribe1 rclcpp std_msgs)

install(TARGETS
  publisher
  subscriber
  DESTINATION lib/${PROJECT_NAME}
)
# package.xml
  <depend>rclcpp</depend>
  <depend>std_msgs</depend>
# 编译运行
colcon build
ros2 run subscribe_and_publish publisher
ros2 run subscribe_and_publish subscriber

```


```cpp
// publisher.cpp
#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

class Publisher : public rclcpp::Node
{
public:
    // 构造函数,参数为节点名称
    Publisher(std::string name) : Node(name)
    {
        RCLCPP_INFO(this->get_logger(), "大家好，我是%s.", name.c_str());
        // 创建发布者
        subscribe_and_publish_publisher_ = this->create_publisher<std_msgs::msg::String>("subscribe_and_publish", 10);
        // 创建定时器，500ms为周期，定时发布
        timer_ = this->create_wall_timer(std::chrono::milliseconds(500), std::bind(&Publisher::timer_callback, this));
    }

private:
    void timer_callback()
    {
        // 创建消息
        std_msgs::msg::String message;
        message.data = "1234";
        // 日志打印
        RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
        // 发布消息
        subscribe_and_publish_publisher_->publish(message);
    }
    // 声名定时器指针
    rclcpp::TimerBase::SharedPtr timer_;
    // 声明话题发布者指针
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr subscribe_and_publish_publisher_;
};

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    /* 产生一个的节点 */
    auto node = std::make_shared<Publisher>("publisher");
    /* 运行节点，并检测退出信号 */
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}

```


```cpp
// subscriber.cpp
#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

class Subscribe : public rclcpp::Node
{
public:
    Subscribe(std::string name) : Node(name)
    {
        RCLCPP_INFO(this->get_logger(), "大家好，我是%s.", name.c_str());
        // 创建一个订阅者订阅话题
        subscribe_and_publish_subscribe_ = this->create_subscription<std_msgs::msg::String>("subscribe_and_publish", 10, std::bind(&Subscribe::command_callback, this, std::placeholders::_1));
    }

private:
    // 声明一个订阅者
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscribe_and_publish_subscribe_;
    // 收到话题数据的回调函数
    void command_callback(const std_msgs::msg::String::SharedPtr msg)
    {
        RCLCPP_INFO(this->get_logger(), "收到[%s]指令", msg->data.c_str());
    };
};

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    /*产生一个的节点*/
    auto node = std::make_shared<Subscribe>("subscriber");
    /* 运行节点，并检测退出信号*/
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





