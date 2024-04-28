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


**男儿志兮天下事，但有进步不有止，言志已酬便无志。——梁启超**

### 😏1. 为什么要发展自动驾驶

机器人与人工智能技术的进步，催生了自动驾驶的再一次技术高潮。

科技在进步，汽车新技术不断兴起，从电动化到智能化，从高级辅助驾驶到自动驾驶，都是为了解决最根本的能源、环境和安全问题。

总之，自动驾驶技术的发展既能推动道路交通绿色、安全、智能化的进程，也能对社会经济发展产生重要的推动作用。

### 😊2. 自动驾驶是什么

自动驾驶，也称无人驾驶或自主驾驶，是指车辆或其他交通工具在没有人类干预的情况下进行行驶和控制的技术。它可以由传感器、摄像头、雷达、激光雷达、GPS等设备实现对周围环境的感知和识别，通过机器学习、人工智能等算法进行决策和行动，从而实现完全自主的驾驶。自动驾驶的发展被认为有望提高道路交通的安全性、效率和便利性。

完全自动驾驶的实现还有很长一段路要走，目前大多数公司是用ADAS和AD两条路线走，包括Tesla等企业。

### 😆3. 自动驾驶如何入门

读者可能是学生，也可能是已经工作的工程师，大家可以从自己擅长的东西出发，比如之前是做算法的，就可以从了解自动驾驶相关算法入手；学习人工智能的，就可以从AI在自动驾驶上的应用开始；车辆相关的，可以从学习车辆动力学，了解`pnc`算法开始；最后，没有方向的，我觉得可以从了解工具链开始，先学习`ros`，了解自动驾驶软硬件是如何工作的开始……

这里列出一个自动驾驶学习路线，供参考：

> 1. 基础知识：学习计算机科学、机器人学、控制理论、传感器技术、信号处理等基础知识。
> 2. 编程语言：掌握至少一种编程语言，如C++、Python，熟悉算法和数据结构。
> 3. 自动驾驶原理：了解自动驾驶的原理、架构和组成部分，包括远程通信、定位和地图、传感器信息、机器视觉和系统控制等。
> 4. 感知与决策系统：学习感知与决策系统，包括视觉传感器解析、行为决策制定，深度学习算法等。
> 5. 仿真环境：使用autoware、Apollo等自动驾驶开源平台，使用Gazebo、CARLA等仿真工具进行仿真环境搭建和测试。
> 6. 真实车辆实验：基于上述仿真环境进行逐步演进，进行真实车辆实验，反馈调整措施，不断完善系统。
> 7. 自动驾驶法律法规：了解国内外自动驾驶相关法规，保证同步整改和升级。
> 8. 进一步提升和实践：学习和实践最新的自动驾驶技术，如自主学习、稳定性控制、故障诊断、边缘计算等，深入理解整个系统的构成和性能，提高自主开发能力。

自动驾驶中间件是连接整个自动驾驶系统的核心组件，其作用是在自动驾驶各模块之间、软件模块与硬件之间提供硬件抽象、驱动管理、通信机制和常用工具，常用的有`ros、dds、autosar`等。（ros是一个开源的机器人软件平台，提供了一个通用的机器人系统架构，已经被广泛应用于自动驾驶领域。autosar是基于ecu的汽车开放系统架构，规定了基本系统功能、功能接口和开发方法，有cp和ap两个版本，类似于单片机的rtos，已成为汽车嵌入式的标准。）


ros有ros1和ros2两个版本，主要区别如下：

| ros1 | ros2 |
| --- | --- |
| 初始版本，事实标准，开发灵活性好 | 更新版本，实时性拓展性更强 |
| 基于发布/订阅和服务的通信模式 | 基于DDS（Data Distribution Service）协议，支持更多的QoS（Quality of Service）机制 |
| 支持广泛的硬件平台，已被广泛应用，有了大量的代码库和算法库，可加速应用开发 | 有更好的实时性能和更高的稳定性，可以支持更多的应用领域 |


ros能解决的软件开发问题主要有：

1. 分布式计算  
 如：一个机器人有多个控制器，分别执行不同的任务；一个机器人有一个控制器，每个任务抽象为单独的模块；多个机器人基于通信协同完成一个任务；用户通过远程指令控制机器人。
2. 软件复用  
 随着机器人研究的发展，诞生了一批应用于导航、建图、路径规划等通用任务的算法，可以很方便的去调用这些接口并将精力放在新算法的设计与实现上。


一些公司还会自研中间件，如基于`DDS协议+Autosar`平台等，实际上是为了让整车各功能间更好的交互，以后可能是整车以`SOA`（Service Oriented Architecture）的架构来设计，实现了解决方案间的集成、系统组件的集成、业务实现的集成，甚至可能以后也会往微服务架构去发展，彻底实现功能或者业务的组件化，当然这都是互联网到汽车领域的舶来品，因此，工作经验到哪都有用，加油少年。


下面就从ros学习出发，入门自动驾驶参考：

> 1. 学习ros：了解ROS的基本概念和工具，掌握ROS的基本命令、ROS节点和ROS话题的概念，学习如何使用ROS来构建机器人应用程序。
> 2. ros硬件驱动：了解常见的ros驱动程序，如sensor、actuator等，如何与底层串口或者CAN通信，并在硬件或仿真环境中实现。
> 3. ros自动驾驶开源平台：使用autoware、Apollo等自动驾驶开源平台，使用Gazebo、CARLA等仿真工具进行仿真环境搭建和测试。
> 4. 路径规划和控制：学习如何使用ROS的路径规划和控制库，实现机器人的运动控制，包括PID控制、最优控制、A\*、D\*、lattice planner、EM planner、hybrid A\*等。
> 5. 地图定位和感知：学习视觉opencv、点云pcl，了解数据处理与滤波算法实现；了解高精地图创建，如NDT、LOAM等；学习如何将视觉传感器和车辆控制相结合，开发机器人视觉控制和感知算法，实现机器人的自主导航、定位、识别和跟踪等任务。
> 6. 深度学习和自主决策：学习如何将深度学习算法应用于自动驾驶中，如CNN、LSTM等常用算法；了解感知融合，如MV3D、AVOD、PointPainting等；了解自主决策，如有限状态机，故障处理等。
> 7. 系统架构：了解硬件选型、底层硬件与抽象、系统组件、常用通信方式、业务逻辑等。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea548b7c42bb4707adcf4f4738ef5b8d.png)

# Jetson系列嵌入式AI平台

## 一、前言
Jetson系列是Nvidia推出的**一套边缘计算平台**。

提到英伟达，大家的第一反应可能是“这是一家芯片公司，主要做GPU的”，因为我们电脑上用到的显卡普遍都是他们家的。

那么为什么一家做显卡的公司能跟边缘计算平台联系到一块呢？这两者之间有什么联系，又跟我们的汽车智能化有什么关系？这时我们想要重点了解的。

### 人工智能
大家都知道，人工智能这个概念很早就被提出来了，但真正能用到实践上的时间并不长，归根结底还是因为以前的算力跟不上，就算你有很好的理论、算法，但没有支撑它实现的平台还是不行的。

而英伟达是较早发现这一市场需求的人，它很早就开始布局自己的GPU平台，当然早期还是用于计算机图形处理、3D游戏这些对图形图像处理速度要求高的领域，然后慢慢的，在实践的检验下英伟达的GPU平台也慢慢成熟，也正好赶上了AI发展的有一个高潮，于是，**大数据、强算力、优算法**这三者一起成为了AI领域的三大法宝。

### 机器人
与AI一样，机器人技术也是人类一直以来想要突破的一个技术领域。但两者还是有所不同，AI主要是软件算法的技术，而机器人是软硬件一体化的平台，包含传感器、控制器、执行器，背后的技术还有机器人运动学、动力学、SLAM、导航规划等。

广义上来说，机器人和自动驾驶汽车是有一些相通的地方的，机器人技术可以说是为自动驾驶技术作了铺垫，而现在市场上确实有提出车辆机器人这种概念的，比如RoboCar、Robotaxi、Robobus等等。

### 自动驾驶汽车
自动驾驶汽车和机器人广义上都属于物联网的范畴，都是软硬件一体化平台。和机器人不同的是，自动驾驶汽车的执行器是相对统一的，都是在原来传统车辆的基础上进行线控改装；传感器也是相对稳定的，包含相机、激光雷达、组合导航等；那么剩下的控制器其实才是自动驾驶的核心，也可以说是自动驾驶大脑，它上面承载着一系列功能软件，包含着任务调度功能，保障着自动驾驶系统的可靠运行。

那么今天我们学习的Jetson nano和ROS对自动驾驶控制器的研究有什么好处呢？

## 二、Jetson nano硬件架构
Jetson系列及nano在其中的地位

CPU

GPU

开发套件与模组

## 三、Jetson nano软件堆栈
Jetpack

CUDA

cuDNN

TensorRT

## 四、Jetson nano应用
Jetbot

## 五、从Jetson看边缘AI平台
Jetson AGX

华为MDC

地平线智能计算平台

## 其他资源

[Jetson AI kit](http://spotpear.cn/public/index/study/detail/id/219.html)

# 决策规划
大家对决策规划部分可能会有些陌生，这是自动驾驶的大脑部分（计算平台）主要解决的事。

这里引用一段大佬的定义：	
```
Cruise自动驾驶决策规划&控制负责人Brandon Basso本科毕业于哥伦比亚大学，博士毕业于加州大学伯克利分校，主要研究决策、机器人系统设计和软件架构、机器学习、控制理论等。曾在3D Robotics、Uber自动驾驶公司工作多年，担任重要职位，在无人机和自动驾驶领域拥有丰富的经验。

Brandon Basso定义了一个好决策系统的特征和输出应该是怎样的。

好决策系统的特征：

    1.及时性（要能及时的做出决策）
    2.交互决策（考虑自车的动作对其他交通参与者的影响、考虑其他道路交通参与者的动作）
    3.可靠性和可重复性（如果自动驾驶系统在同样的场景做出的决策不同，说明系统稳定性不好）

好决策系统的输出：**安全、舒适、老司机般的体验** ！
```
因此，决策规划部分是自动驾驶系统的相对核心，前面连接感知系统的输出，后面对接控制系统的输入，决策规划的效果与其他两者密不可分。

## pure_pursuit（纯跟踪算法）
首先感性的去认识一下纯追踪算法，简单理解就是，无人车关注轨迹中某个点目标goal，然后去追踪它。

在无人地面载具（UGV）场景下，这里的goal是Planning子系统所规划轨迹上的一些点，称为路点（Way Points）。如果轨迹是连续的，就在轨迹上采样多个点。如果轨迹是离散的，那么就可以直接使用。

[链接](http://t.csdn.cn/I9l7N)
[2](http://t.csdn.cn/Pf5D7)

## LatticePlan（五阶轨迹规划）
[1](http://t.csdn.cn/6F1b7)

## 导航路径规划
[1](http://t.csdn.cn/w0s3g)

## 轨迹规划
[1](http://t.csdn.cn/EpOUO)

## 匈牙利匹配
[1](http://t.csdn.cn/6Wxpn)

## 蒙特卡罗-马尔科夫链
[1](http://t.csdn.cn/Y9U5v)

## ROS高级应用

### ROS与开发板

ROS一般安装在工控机上，而工控机自然也需要与下层的各种控制板进行通信，ROS通信机制在其中发挥了很大的作用。如ROS与[Arduino](https://www.ncnynl.com/category/ros-arduino/)、ROS与STM32。

### ROS与相机

ROS中可以基于usb-cam包很方便的调用相机图像。


### ROS与激光雷达

###### 单线-rplidar

RPLIDAR是低成本的二维雷达解决方案，由SlamTec思岚科技公司的RoboPeak团队开发。

它能扫描360°，6米半径的范围。

它适合用于构建地图，SLAM和建立3D模型。

1.安装驱动

2.查看串口权限

3.启动launch文件实现雷达数据可视化

4.gmapping slam

5.hector slam

6.karto slam

7.cartographer slam

###### 多线机械-速腾16线

1.安装驱动

2.网口配置

3.启动launch文件实现雷达数据可视化

4.Lego-loam slam

5.雷达3D点云数据转2D激光数据（pointcloud_to_laserscan）

6.rosbag录制与回放数据包



### ROS与IMU

razor-imu-9dof是串口IMU，可以用ros-noetic-serial包读取数据。

https://github.com/ncnynl/razor_imu_m0_driver

ublox-GPS是串口GPS。

https://github.com/ncnynl/ublox

bosch_imu_driver博世IMU：

https://gitee.com/ncnynl/bosch_imu_driver

### ROS与外设

###### G29方向盘/手柄

usb接口		
sudo apt-get install ros-melodic-pacmod*	
sudo apt-get install ros-melodic-joy*	
可读取G29方向盘实时数据！

获取G29力反馈信号：	
https://github.com/ncnynl/ros-g29-force-feedback.git

### ROS与深度学习

Openvino是Intel基于视觉的平台。

Jetson是Nvidia的软硬件平台。

Darknet是yolo检测的平台。

### ROS无人系统

[创客智造](https://www.ncnynl.com/)里包含大量ROS入门、开发板、开发语言的教程，推荐作为参考！

###### Race_Car

用RC底盘改造ROS无人车，可以用Jetson TX1

起源于MIT的RACECAR挑战赛。

###### Ailibot_QT

基于ROS_Qt实现话题的发布订阅，查看librviz，移动和显示地图，调用对话框界面等。

[ROS_QT入门教程](https://www.ncnynl.com/archives/201903/2863.html)

[ROS_QT人机交互-老蒋](https://www.ncnynl.com/archives/202006/3774.html)

###### Pixhawk无人机

###### ArduSub无人潜艇

###### 自己搭建ROS小车（两种方案）

参考：小白学移动机器人

######### ROS-arduino小车平台

https://www.ncnynl.com/archives/201612/1203.html

######### ROS-stm32小车平台

https://www.ncnynl.com/archives/201703/1414.html




以上。



