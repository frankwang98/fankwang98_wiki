### 😏1. pcl_ros介绍


`pcl_ros`是一个用于将`PCL`（点云库）与`ROS`（机器人操作系统）集成的软件包。它提供了用于在ROS环境中处理和可视化点云数据的工具和功能。


以下是`pcl_ros`的主要功能和组件：



> 
> 1.ROS节点：pcl_ros提供了一个ROS节点，用于订阅和发布点云数据。您可以使用该节点来接收来自传感器或其他节点的点云数据，并将处理后的点云数据发布到其他节点。
> 
> 
> 



> 
> 2.传感器接口：pcl_ros提供了与ROS传感器消息（如sensor_msgs::PointCloud2）之间的转换接口。您可以使用这些接口将ROS传感器消息转换为PCL点云对象（pcl::PointCloud），并进行进一步的处理。
> 
> 
> 



> 
> 3.可视化工具：pcl_ros提供了用于在ROS环境中可视化点云数据的工具。您可以使用rviz等ROS可视化工具来显示和分析点云数据。
> 
> 
> 



> 
> 4.过滤器和特征提取：pcl_ros包含了一系列的滤波器和特征提取功能，可以直接应用于ROS点云数据。您可以使用这些功能来对点云数据进行降噪、下采样、特征提取等操作。
> 
> 
> 



> 
> 5.点云转换：pcl_ros提供了点云坐标系之间的转换功能。您可以使用这些功能来将点云数据从一个坐标系转换到另一个坐标系，以适应不同传感器或机器人系统的需求。
> 
> 
> 



> 
> 6.ROS参数服务器：pcl_ros允许您使用ROS参数服务器来配置和调整点云处理的参数。您可以使用参数服务器来设置滤波器参数、特征提取参数等。
> 
> 
> 


通过将PCL和ROS相结合，`pcl_ros`使得在ROS环境中处理和操作点云数据更加方便和高效。它提供了丰富的功能和工具，使得点云数据的获取、处理和可视化变得更加容易。


### 😊2. 环境安装与配置


确认已经安装了ROS和PCL。



```
# pcl_ros包安装
sudo apt install ros-noetic-pcl-ros

rosrun pcl_ros xxx # 启动节点
# 默认包含以下节点
bag_to_pcd
convert_pcd_to_image
convert_pointcloud_to_image
pcd_to_pointcloud
pointcloud_to_pcd

```

### 😆3. 点云转换应用示例


下面基于pcl_ros包实现pcl读取pcd文件通过ros话题发布，以及ros订阅话题后通过pcl显示：


#### pcd_pub节点



```
pcd_pub.cpp
#include <ros/ros.h>
#include <sensor_msgs/PointCloud2.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>

int main(int argc, char** argv)
{
    // 初始化ROS节点
    ros::init(argc, argv, "pcd_publisher");
    ros::NodeHandle nh;

    // 创建一个ROS发布者，用于发布点云消息
    ros::Publisher pub = nh.advertise<sensor_msgs::PointCloud2>("pointcloud", 1);

    // 读取PCD文件
    pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
    if (pcl::io::loadPCDFile<pcl::PointXYZ>("../test.pcd", *cloud) == -1)
    {
        PCL_ERROR("Failed to read PCD filen");
        return -1;
    }

    // 创建一个ROS点云消息
    sensor_msgs::PointCloud2 output;
    pcl::toROSMsg(*cloud, output);
    output.header.frame_id = "base_link";  // 设置点云消息的坐标系

    ros::Rate loop_rate(10);
    while (ros::ok())
    {
        // 发布ROS点云消息
        pub.publish(output);
        std::cout << "Published a point cloud." << std::endl;

        ros::spinOnce();
        loop_rate.sleep();
    }

    return 0;
}

```

CMakeLists.txt



```
cmake_minimum_required(VERSION 3.0.2)
project(pcd_pub)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++14)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS 
  roscpp 
  rospy
  std_msgs
  sensor_msgs
  pcl_ros
)

find_package(PCL REQUIRED)

catkin_package(
  CATKIN_DEPENDS 
  roscpp 
  rospy 
  std_msgs 
  sensor_msgs 
  pcl_ros
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)

add_executable(${PROJECT_NAME} src/pcd_pub.cpp)

## Specify libraries to link a library or executable target against
target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
)

install(TARGETS pcd_pub
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

```

package.xml



```
<?xml version="1.0"?>
<package format="2">
  <name>pcd_pub</name>
  <version>0.0.0</version>
  <description>The pcd_pub package</description>

  <!-- One maintainer tag required, multiple allowed, one person per tag -->
  <!-- Example:  -->
  <!-- <maintainer email="jane.doe@example.com">Jane Doe</maintainer> -->
  <maintainer email="xxx@todo.todo">xxx</maintainer>


  <!-- One license tag required, multiple allowed, one license per tag -->
  <!-- Commonly used license strings: -->
  <!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 -->
  <license>TODO</license>

  <buildtool_depend>catkin</buildtool_depend>

  <build_depend>roscpp</build_depend>
  <build_depend>rospy</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>sensor_msgs</build_depend>
  <build_depend>pcl_ros</build_depend>

  <exec_depend>roscpp</exec_depend>
  <exec_depend>rospy</exec_depend>
  <exec_depend>std_msgs</exec_depend>
  <exec_depend>sensor_msgs</exec_depend>
  <exec_depend>pcl_ros</exec_depend>

  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- Other tools can request additional information be placed here -->

  </export>
</package>

```

#### pcd_sub节点



```
// pcd_sub.cpp
#include <ros/ros.h>
#include <sensor_msgs/PointCloud2.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/visualization/cloud_viewer.h>

pcl::visualization::CloudViewer viewer("PointCloud Viewer");

void cloudCallback(const sensor_msgs::PointCloud2ConstPtr& msg)
{
    pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
    pcl::fromROSMsg(*msg, *cloud);

    // 获取点云数据的大小（字节数）
    size_t pointCloudSize = msg->data.size();

    ROS_INFO("Received point cloud size: %zu bytes", pointCloudSize);

    if (!viewer.wasStopped())
        viewer.showCloud(cloud);
}

int main(int argc, char** argv)
{
    ros::init(argc, argv, "pcl_viewer");
    ros::NodeHandle nh;

    ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("pointcloud", 1, cloudCallback);

    ros::spin();

    return 0;
}

```

CMakeLists.txt和package.xml基本一致，除代码文件名不同外。



```
# 运行节点
catkin_make # 编译
source devel/setup.bash
rosrun pcd_pub pcd_pub # 发布
rosrun pcd_sub pcd_sub # 订阅

```

这样就实现了pcd点云与ros话题之间的转换，此外还可以利用ros的可视化工具如rviz进行查看。


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


## 一、PCL介绍和学习路径
### PCL介绍

	点云数据的处理可以采用获得广泛应用的Point Cloud Library (点云库，PCL库)。
	PCL库是一个最初发布于2013年的开源C++库。它实现了大量点云相关的通用算法和高效的数据管理。
	支持多种操作系统平台，可在Windows、Linux、Android、Mac OS X、部分嵌入式实时系统上运行。
	PCL是BSD授权方式，可以免费进行商业和学术应用。
**如果说OpenCV是2D信息获取与处理的技术结晶，那么PCL在3D信息获取与处理上，就与OpenCV具有同等地位**
	
下面是PCL架构图：

如图PCL架构图所示，对于3D点云处理来说，PCL完全是一个的模块化的现代C++模板库。其基于以下第三方库：Boost、Eigen、FLANN、VTK、CUDA、OpenNI、Qhull，实现点云相关的获取、滤波、分割、配准、检索、特征提取、识别、追踪、曲面重建、可视化等。

每个模块都有依赖关系，依赖关系如下图（可以看出有四层），最基本的就是最底层的commom模块。	
箭头对应的是依赖关系，比如第二层的kdtree依赖于common；第四层的registration有四个箭头，分别是sample_consensus, kdtree, common, features。

### PCL学习路径
入门资料：		
视频：[bilibili-PCL点云库官网教程](https://space.bilibili.com/504859351/channel/series)		
github：[点云库PCL学习教程书籍每章总结](https://github.com/MNewBie/PCL-Notes)

代码实践：	
官方各模块demo-[wiki](https://pcl.readthedocs.io/projects/tutorials/en/latest/#)	
模块对应的对象和函数-[docs-modules](https://pointclouds.org/documentation/modules.html)		
具体模块的学习-有针对性（看双愚的系列文章	）		
pcl实践[黑马pcl-3d点云](https://robot.czxy.com/docs/pcl/)		
CSDN博主系列文章[PCL学习(64篇)](https://www.cnblogs.com/li-yao7758258/category/954066.html)

## 二、PCL安装配置
pcl源代码编译安装：[see this](https://robot.czxy.com/docs/pcl/env/pcl/)

ubuntu安装好ros后，还需要安装pcl-tools：`sudo apt install pcl-tools`，才能使用pcl_viewer等工具。

二进制安装：`ros-noetic-pcl-ros`，默认版本1.10

总结：源码编译会出现莫名其妙的错误，比如我就出现可视化的库找不到和一些操作符过期的问题。最终，大家都用ros的pcl就好了，兼容性特别好！

## 三、PCL各模块学习
### PCL中常用的PointT类型
PointXYZ——成员变量：float x,y,z;

PointXYZ是使用最常见的一个点数据类型，因为他之包含三维XYZ坐标信息，这三个浮点数附加一个浮点数来满足存储对齐，可以通过points[i].data[0]或points[i].x访问点X的坐标值
```
union
{
float data[4];
struct
{
float x;
float y;
float z;
};
};
```
PointXYZI——成员变量：float x,y,z,intensity

PointXYZI是一个简单的X Y Z坐标加intensity的point类型，是一个单独的结构体，并且满足存储对齐，由于point的大部分操作会把data[4]元素设置成0或1（用于变换），不能让intensity与XYZ在同一个结构体中，如果这样的话其内容将会被覆盖，例如：两个点的点积会把第四个元素设置为0，否则点积没有意义。
```
union{
float data[4];
struct
{
float x;
float y;
float z;
};
};
union{
struct{
float intensity;
};
float data_c[4];
};
```

通用编译方法：`mkdir build && cd build && cmake .. && make`

### 基础-pcd写入和读取

### kd-tree

### oc-tree 

## 四、PCL实践

以上。



# MSF多传感器融合感知

## 一、前言

多传感器融合（Mult-Sensor Fusion，MSF）是指利用传感器技术、计算机和人工智能技术，将来自多源传感器的信息以一定的准则进行分析和综合，从而为决策和估计提供精准数据的一种必要的信息处理过程。与人类感知外界环境类似，多源传感器都有其不可替代的优势，因此有必要对多种传感器进行多层次的信息互补和优化处理，最终形成对探测环境的一致性认知。

多传感器融合的前提是多个传感器的时间和空间同步，有硬同步和软同步两种方法。

多传感器融合根据信息融合的级别可以分为数据级别融合（前融合）、特征级别融合和目标级别融合（后融合/决策融合），需要根据应用场景的不同选择适合的信息融合方式。

多传感器融合的主要应用方向是多目标检测，其优势在于不同传感器之间可以进行信息互补，提高感知结果的准确性和鲁棒性。

本文对一款混合固态激光雷达和一种低成本相机的感知融合，通过相机与激光雷达的联合标定实现时空同步，基于深度学习方法在车辆平台和路侧平台进行多目标融合检测，并与单一传感器检测效果进行对比，证明多传感器融合感知结果准确性和鲁棒性更高。

## 二、多传感器融合方法

多传感器融合的基本原理是将多种传感器进行多层次、多空间的信息互补和优化组合处理，最终形成对探测环境的一致性认知。

我们都知道，对传感器数据的处理流程一般有获得数据、提取特征、识别目标三个步骤，由此产生了多传感器信息融合的三种方法，即数据融合方法、特征融合方法和目标融合方法。

数据融合方法是指对融合后的多维数据进行统一感知算法处理的方法。

特征融合方法属于中间层的融合，即先从每个传感器提供的原始数据中提取代表性的特征，再把这些特征融合成单一的特征向量，其中选择合适的特征进行融合是关键，特征信息包括边缘、方向、速度、形状等。（数据融合和目标融合目前只是概念，实际算法设计其实是特征融合）

特征层融合根据融合特征的性质不同又可划分为两大类：目标状态融合（特征前融合）、目标特征融合（特征后融合）。

目标状态融合主要应用于多目标跟踪领域，融合系统首先对传感器数据进行预处理以完成数据配准，在数据配准后实现参数关联和状态估计。（数据配准）

目标特征融合就是特征层联合识别，它的实质就是模式识别问题，在融合前必须先对特征进行关联处理，再对特征矢量分类成有意义的组合。（特征匹配）

在目标融合方法中，每种传感器都会输出独立的感知结果，而目标融合就是指综合各传感器的目标级感知结果，再由主处理器进行数据融合的方法。

在融合的三个层次中，特征层融合技术发展较为完善，并且由于在特征层已建立了一整套的行之有效的特征关联技术，可以保证融合信息的一致性，此级别融合对计算量和通信带宽要求相对降低，但由于部分数据的舍弃使其准确性也有所下降。

总的来说，从数据级融合到目标级融合，对计算量和通信带宽的要求依次下降，同时感知结果的准确性也会逐级下降，因此，需要根据应用场景和主机性能的不同选择合适的信息融合方法。


## 三、多传感器联合标定

多传感器融合首先需要多个传感器的时间和空间同步，即时间戳和坐标系的统一。

时空同步主要有硬同步和软同步两种方法。硬同步是指硬件同步，即根据传感器状态信息，在硬件中同时发布数据采集指令，实现各传感器采集的时间同步，这种方案可以大大降低时间同步误差，但需要重新设计硬件，难度较大。软同步是指软件同步，又分为软件时间同步和软件空间同步，软件时间同步是指通过统一的主机确定各传感器的基准时间，各传感器的数据信息会自带时间戳信息，实现时间戳同步；软件空间同步是将不同传感器的数据通过坐标变换统一到同一坐标系下，在运动场景如车辆运动过程中还需要考虑当前速度下的帧内位移校准。

具体步骤如下：首先标定摄像头，获得其内参矩阵；然后用Autoware与RViz工具通过多组点云和图像的重投影解算，获得激光雷达坐标系与摄像头坐标系之间的旋转矩阵和平移矩阵，实现两者的外参标定，即空间同步；接着以激光雷达时间戳为基准，摄像头向激光雷达配准实现两者的时间同步。标定完成后，激光雷达点云能较好地投影到相机图像上。

## 四、基于车辆和路侧平台的多目标融合检测

基于多传感器路侧平台进行多目标检测对于车路协同很有意义，可以将计算量从车端转移到路端，同时由于路侧平台的视野范围更大，可以将更多的目标信息传递给车端。

多目标融合检测的基本思路是：首先需要将摄像头和激光雷达进行联合标定，获取两者坐标系的空间转换关系，然后把激光雷达投射到图像的坐标系中，建立图像的像素点，和激光雷达投影后的点之间做匹配；数据融合后图像上会显示激光雷达的深度信息；在多目标检测时，障碍物的检测可以使用激光雷达进行物体聚类，但是对于较远物体稀疏的激光线数聚类的效果较差，因此利用视觉图像信息进行目标检测，进而获取障碍物的位置，同时视觉还可以给出障碍物类别信息。

基于智能驾驶车辆平台和移动车路协同平台，设计了激光雷达与摄像头融合的环境感知算法，兼顾了检测精度和实时性。

具体方法如下：

激光雷达：PointPillars（设计一种轻量化点云分类网络）

使用雷达进行环境感知的传统步骤包括获取雷达数据、预处理（滤波、地面点去除）、创建地图（栅格地图/3D地图）、聚类（基于划分、密度、网格）、ROI提取（网格法聚类、栅格地图投影到图像、动态阈值放大远处物体、合并ROI）、识别（降低置信度，融合两者分类，据此准备数据集，设计点云分类网络）、跟踪（卡尔曼滤波求最优值，降低误差）。

摄像头：Yolo（改进定位、检测精度）

图像检测一般有传统方法和深度学习方法两种，前者一般是设计模型特征，进行分类，属于手动特征提取，而后者则是利用机器自我学习训练的思路。

融合感知：MV3D、AVOD（改进OC3D-AVOD）

融合方式选择：特征融合（首选成熟）、数据融合（车辆平台）、目标融合（路侧平台）

目标融合方法（特征后融合）：结合点云聚类和目标检测的优势，设计一种针对目标区域范围点云的分类神经网络，然后基于IOU进行图像和点云的目标匹配，将视觉检测与点云分类的结果进行软加权平均，得出最终检测结果。实验证明在车辆、行人检测中效果很好。

数据融合方法（特征前融合）：采用融合感知算法直接对相机图像和雷达点云进行实时处理。基于多视图即点云俯视图（BEV）、点云前视图和前摄像头图像进行特征融合。

实际上，车路协同（V2X）技术中一个关键点就是将车辆平台和路侧平台的感知数据对同一目标进行最终的结果融合，使融合后的结果具有唯一性且置信度高，真正实现了数据的决策融合感知和交互共享。基于车路协同的感知融合算法属于后融合算法。

## 五、与单一传感器检测结果对比

单一传感器检测结果

多传感器检测结果

## 六、总结

预期结果，多传感器检测结果更好。

融合感知算法将广泛应用于路端和车端设备，主要解决障碍物错检漏检以及夜间感知的问题。

未来可以使用更加快速、准确、健壮的多目标检测算法，并进行工程化部署。

V2X技术也将先从感知融合与交互上进行突破。

