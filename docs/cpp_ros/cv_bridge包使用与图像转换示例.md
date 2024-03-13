







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍cv\_bridge包使用与图像转换示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. cv\_bridge介绍](#smirk1_cv_bridge_7)
	+ [:blush:2. 环境安装与配置](#blush2__21)
	+ [:satisfied:3. 图像转换应用示例](#satisfied3__25)
	+ - [image\_pub节点](#image_pub_28)
		- [image\_sub节点](#image_sub_174)




### 😏1. cv\_bridge介绍


`cv_bridge`是一个用于在`ROS`（Robot Operating System）和`OpenCV`之间进行图像转换的库。它提供了方便的接口和功能，用于在ROS中将ROS图像消息（`sensor_msgs/Image`）与OpenCV图像格式之间进行相互转换。


在ROS中，`cv_bridge`通常与`sensor_msgs`包一起使用，用于处理图像消息，并使用OpenCV进行图像处理、计算机视觉算法和图像分析等操作。


以下是一些cv\_bridge库的主要功能：



> 
> 1.将ROS图像消息转换为OpenCV图像格式：cv\_bridge提供了方便的方法，可以将ROS图像消息转换为OpenCV的cv::Mat格式，方便在OpenCV中进行图像处理。
> 
> 
> 



> 
> 2.将OpenCV图像转换为ROS图像消息：cv\_bridge还提供了将OpenCV的cv::Mat图像转换为ROS图像消息的方法，以便将处理后的图像传递给其他ROS节点或话题。
> 
> 
> 



> 
> 3.支持不同的图像编码格式：cv\_bridge支持各种常见的图像编码格式，包括JPEG、PNG、BMP等。它可以在ROS和OpenCV之间进行透明的编码和解码操作。
> 
> 
> 



> 
> 4.进行图像数据的共享：cv\_bridge允许在ROS和OpenCV之间共享图像数据，而无需进行复制。这在处理大型图像时可以提高性能和效率。
> 
> 
> 


### 😊2. 环境安装与配置


正常情况下，安装完ros后可正常使用`cv_bridge`包。


`cv_bridge`一般要与OpenCV**版本对应**，比如`noetic`对应`OpenCV4`，`melodic`对应`OpenCV3`，如果在melodic环境下装了OpenCV4，可能会报错，可参考这篇来解决这个问题：`http://t.csdnimg.cn/Sbwji`


### 😆3. 图像转换应用示例


下面基于cv\_bridge包实现opencv读取视频并通过ros消息发布，然后订阅节点获取到图像后通过opencv进行显示。


#### image\_pub节点



```
// image\_pub.cpp
#include <ros/ros.h>
#include <opencv2/opencv.hpp>
#include <image\_transport/image\_transport.h>
#include <cv\_bridge/cv\_bridge.h>

int main(int argc, char \*\*argv)
{
    ros::init(argc, argv, "image\_pub");
    ros::NodeHandle nh;
    image_transport::ImageTransport it(nh);
    image_transport::Publisher image_pub = it.advertise("camera/image", 1);

    // 打开视频文件
    cv::VideoCapture cap("../test.mp4");
    if (!cap.isOpened())
    {
        ROS\_ERROR("Failed to open video file");
        return -1;
    }

    // 定义图像消息
    sensor_msgs::ImagePtr msg;

    ros::Rate loop\_rate(30); // 发布频率为30Hz
    while (ros::ok())
    {
        cv::Mat frame;
        cap >> frame; // 读取视频帧

        if (frame.empty())
        {
            ROS\_INFO("Video ended");
            break;
        }

        // 转换图像格式为ROS消息
        msg = cv_bridge::CvImage(std_msgs::Header(), "bgr8", frame).toImageMsg();

        // 发布图像消息
        image_pub.publish(msg);
        std::cout << "Published image" << std::endl;

        ros::spinOnce();
        loop_rate.sleep();
    }

    return 0;
}

```

CMakeLists.txt



```
cmake_minimum_required(VERSION 3.0.2)
project(image_pub)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find\_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS 
  roscpp 
  rospy
  std_msgs
  sensor_msgs
  image_transport
  cv_bridge
)

catkin_package(
  CATKIN_DEPENDS 
  roscpp 
  rospy 
  std_msgs 
  sensor_msgs 
  image_transport 
  cv_bridge
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin\_INCLUDE\_DIRS}
)

add_executable(${PROJECT\_NAME} src/image_pub.cpp)

## Specify libraries to link a library or executable target against
target_link_libraries(${PROJECT\_NAME}
  ${catkin\_LIBRARIES}
)

```

package.xml



```
<?xml version="1.0"?>
<package format="2">
  <name>image_pub</name>
  <version>0.0.0</version>
  <description>The image_pub package</description>

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
  <build_depend>image_transport</build_depend>
  <build_depend>cv_bridge</build_depend>

  <exec_depend>roscpp</exec_depend>
  <exec_depend>rospy</exec_depend>
  <exec_depend>std_msgs</exec_depend>
  <exec_depend>sensor_msgs</exec_depend>
  <exec_depend>image_transport</exec_depend>
  <exec_depend>cv_bridge</exec_depend>


  <!-- The export tag contains other, unspecified, tags -->
  <export>
    <!-- Other tools can request additional information be placed here -->

  </export>
</package>

```

#### image\_sub节点



```
// image\_sub.cpp
#include <ros/ros.h>
#include <opencv2/opencv.hpp>
#include <image\_transport/image\_transport.h>
#include <cv\_bridge/cv\_bridge.h>

void imageCallback(const sensor_msgs::ImageConstPtr &msg)
{
    try
    {
        // 将ROS图像消息转换为OpenCV图像
        cv::Mat image = cv_bridge::toCvShare(msg, "bgr8")->image;

        // 显示图像
        cv::imshow("Image", image);
        cv::waitKey(1);
    }
    catch (cv_bridge::Exception &e)
    {
        ROS\_ERROR("Failed to convert ROS image to OpenCV image: %s", e.what());
    }
}

int main(int argc, char \*\*argv)
{
    ros::init(argc, argv, "image\_sub");
    ros::NodeHandle nh;

    // 创建图像传输对象和订阅者
    image_transport::ImageTransport it(nh);
    image_transport::Subscriber sub = it.subscribe("camera/image", 1, imageCallback);

    // 创建opencv窗口本地显示
    cv::namedWindow("Image", cv::WINDOW_AUTOSIZE);
    
    ros::spin();

    cv::destroyAllWindows();

    return 0;
}

```

CMakeLists.txt和package.xml和发布节点基本一样，除了代码文件名不同。



```
# 运行节点
catkin_make # 编译
source devel/setup.bash
rosrun image_pub image_pub # 发布
rosrun image_sub image_sub # 订阅

```

这样就实现了ros图像与OpenCV图像之间的转换，以及使用opencv的VideoCapture类实现视频的读取与显示。


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





