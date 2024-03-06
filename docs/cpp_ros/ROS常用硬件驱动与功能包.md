**红日初升，其道大光。——梁启超**  

ROS的特点就是开源、社区生态好，许多产品都根据ros编写了驱动并贡献到了社区中，也方便了我们的学习。 

### 😏1. 传感器驱动

#### 相机

相机是自动驾驶最常用、也是相对廉价的一种传感器，有`usb、gmsl`等接口。

* usb相机  
 apt安装：`sudo apt install ros-meodic-usb-cam`  
 运行：`roslaunch usb_cam usb_cam-test.launch`  
 源码：`https://github.com/ros-drivers/usb_cam`
* opencv视频流  
 源码：`https://github.com/ros-drivers/video_stream_opencv`
* gscam基于GStreamer视频流  
 源码：`https://github.com/ros-drivers/gscam`
* openni2-camera  
 源码：`https://github.com/ros-drivers/openni2_camera`
* zed双目相机  
 运行：`roslaunch zed_wrapper zed.launch`  
 源码：`https://github.com/stereolabs/zed-ros-wrapper`


#### 激光雷达


激光雷达是近年来自动驾驶最火的传感器之一，由于其强大的感知效果（高分辨率）获得了大家的青睐，也随着自动驾驶行业的发展，技术得到完善，成本得以降低。


* velodyne  
 apt安装：`sudo apt-get install ros-melodic-velodyne`  
 运行：`roslaunch velodyne_pointcloud VLP16_points.launch`  
 源码：`https://github.com/ros-drivers/velodyne`
* slamtec  
 源码：`https://github.com/Slamtec/rplidar_sdk`
* rslidar  
 源码：`https://github.com/RoboSense-LiDAR/rslidar_sdk`  
 转换rslidar\_to\_velodyne：`https://github.com/EpsAvlc/rslidar_to_velodyne`  
 转二维：`https://github.com/RoboSense-LiDAR/rslidar_laserscan`
* ouster  
 运行：`roslaunch ouster_ros sensor.launch`  
 源码：`https://github.com/ouster-lidar/ouster-ros`
* innovusion
* lslidar  
 源码：`https://github.com/Lslidar/Lslidar_ROS1_driver`
* 点云显示：`pcl_viewer xxx.pcd`
* bag转pcd：`rosrun pcl_ros bag_to_pcd <input_file.bag> <topic> <output_directory>`


有个repo总结了关于lidar的厂商、数据集等资料，Github地址：`https://github.com/szenergy/awesome-lidar`


#### 组合惯导


组合惯导包括卫星导航和惯性导航，是室外定位最常见的传感器，已获得广泛应用。


* nmea  
 apt安装：`sudo apt-get install ros-kinetic-nmea-navsat-driver libgps-dev`  
 运行：`roslaunch nmea_navsat_driver nmea_serial_driver.launch`  
 源码：`https://github.com/ros-drivers/nmea_navsat_driver`
* gps坐标转换&rviz显示  
 源码：`https://www.ncnynl.com/archives/202210/5474.html`


### 😊2. 执行器驱动


#### 串口


串口通讯是许多执行器的通讯形式，包含usb、rs232、rs485等。


* rosserial  
 源码：`https://github.com/ros-drivers/rosserial`
* joystick手柄  
 源码：`https://github.com/ros-drivers/joystick_drivers`


#### CAN口


CAN通信在汽车领域应用广泛，需掌握CAN通信协议。


* usb-can  
 配置：



```
sudo modprobe gs_usb
sudo ip link set can0 up type can bitrate 500000
ifconfig -a
sudo apt install can-utils
candump can0  //receiving data from can0
cansend can0 001#1122334455667788 //send data to can0

```

apt安装：`sudo apt-get install ros-melodic-socketcan-bridge`  
 源码：`https://github.com/wanghuohuo0716/ros_can_driver`


### 😆3. 其他功能包


#### 音频


* audio  
 apt安装：`sudo apt-get install ros-kinetic-audio-common libasound2 mplayer`  
 运行：`rosrun sound_play soundplay_node.py` `rosrun sound_play say.py "Hello World."`  
 源码：`https://github.com/ros-drivers/audio_common`


#### android应用


* android  
 源码：`https://github.com/ros-drivers/rosserial-experimental`


#### 开源机器人功能包


松灵机器人：`https://github.com/westonrobot/scout_ros`


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/ba78d1cb395a4c77be86f57b2497d915.png#pic_center)


以上。





