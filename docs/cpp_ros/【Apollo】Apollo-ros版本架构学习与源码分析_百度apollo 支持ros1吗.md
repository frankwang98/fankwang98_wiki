







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Apollo-ros版本架构学习与源码分析。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 架构学习](#blush2__29)
	+ [:satisfied:3. 源码学习](#satisfied3__33)




### 😏1. 项目介绍


Apollo1.0源码注释项目Github地址：`https://github.com/slam-code/apollo`


Apollo perception项目Github地址：`https://github.com/Tartisan/apollo_ros`


Apollo planning项目Github地址：`https://github.com/yufan25/apollo-trajectory-planning`


Apollo 是百度开发的自动驾驶软件平台，旨在提供完整的自动驾驶解决方案。它包括一套完整的软件和硬件系统，涵盖了感知、定位、规划、控制等关键领域。以下是 Apollo 软件的主要组成部分和特点：


1. 感知模块：Apollo 提供了多种传感器数据融合的算法和技术，包括摄像头、激光雷达和毫米波雷达等，以实时感知车辆周围的环境，并对障碍物、行人、交通标志等进行识别和跟踪。
2. 定位模块：Apollo 借助高精度地图和多种定位技术（如 GPS、惯性测量单元等），能够实现准确的车辆定位和导航功能，为自动驾驶系统提供必要的位置信息。
3. 规划和决策模块：Apollo 集成了先进的路径规划和决策算法，能够根据感知数据和行车情况生成最佳的驾驶路径，并做出智能的驾驶决策，例如车道保持、避障、超车等。
4. 控制模块：Apollo 利用先进的车辆控制算法和硬件设备，能够实现精确的车辆控制，包括油门、刹车和转向等，以保持稳定的行驶状态和响应紧急情况。
5. 数据录制和回放：Apollo 提供了数据录制和回放功能，可以记录车辆传感器数据、位置信息和控制指令等，用于测试和验证自动驾驶系统的性能和安全性。
6. 开发工具和平台：Apollo 提供了丰富的开发工具和平台，包括模拟器、调试器、仿真环境和数据管理工具，帮助开发人员进行自动驾驶系统的设计、测试和改进。
7. 安全性和可靠性：Apollo 强调安全性和可靠性，采用多层次的安全保护机制和备份措施，以确保自动驾驶的安全运行。


### 😊2. 架构学习


Apollo是国内比较开放、完整、安全的自动驾驶平台，1.0是基于`ros`通信的架构，后面自研了`cyberrt`；代码编译是基于`bazel`的，此外还用到了`protobuf、abseil、gtest、gflag`等库；部署一般建议`Docker`，也可源码安装；开放版本也提供了`Dreamviewer`交互界面和仿真器。


核心代码在modules目录，分别有`common、canbus、drivers、localizetion、perception、prediction、decision、planning、control、monitor、hmi、tools`等模块。


### 😆3. 源码学习


下面进行使用分析：


**common模块**中，使用glog作为日志库；`VehicleState`类是标识车辆状态信息的class，主要包含线速度、角速度、加速度、挡位状态、车辆坐标x，y，z；`EstimateFuturePosition`用于根据当前信息估计t时刻后的车辆位置；util/factory提供了工厂模式示例；time/time将chrono库作为时间管理工具，默认精度是ns；status/status定义了状态码用于标识车辆工作状态；monitor/monitor类收集并监控各模块的msg；math中提供了多个数学计算工具；adapter/adapter是提供将来自传感器的底层数据和Apollo各个模块交互的统一接口(c++ 适配器模式的示例)，将数据IO抽象，使得各模块不必强依赖ROS框架通信。


**localization模块**中，输入是GPS&IMU模块，输出以proto格式定义，主要是车辆的位置和位姿信息；localization主要实现模块名称显示、初始化、开启、停止等操作，并将模块注册到工厂实例；此外还实现CameraLocalization类的原型，继承自LocalizationBase，搭建了相机定位的接口。


**perception模块**处理所有传感器的输入，包含相机、雷达，并发布目标检测、交通灯检测等信息；Perception类也是对Name、Init、Start、Stop进行函数重写，并用到了common模块的adapter。


**decision模块**根据感知和地图信息，生成决策信息给下游规控模块，具体输入有障碍物、交通灯、地图和路由循迹、车辆状态信息，输出是决策指令，并生成虚拟障碍物。


**planning模块**根据给定的车辆定位和状态信息、感知预测决策信息，计算出一条安全且舒适的路径（轨迹点）给底层去到达。


**control模块**是根据规划和车辆状态信息，用不同的控制算法去生成好的驾驶行为，并将控制指令给到canbus车辆通信网络模块；filters提供了多种滤波器；controller提供了不同的横纵向控制算法。


**tools模块**提供了常用的各类工具，如标定、诊断、配置、画图、包录制等。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





