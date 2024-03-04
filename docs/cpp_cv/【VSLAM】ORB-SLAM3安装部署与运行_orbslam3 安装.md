






**心口如一，犹不失为光明磊落丈夫之行也。——梁启超**  
 



#### 文章目录


* + [:smirk:1. ORB-SLAM3介绍](#smirk1_ORBSLAM3_2)
	+ [:blush:2. 代码安装部署](#blush2__8)
	+ - [1. 安装ros与opencv](#1_rosopencv_9)
		- [2. 安装Pangolin作为可视化和用户界面](#2_Pangolin_27)
		- [3. 安装Eigen3一个开源线性库，可进行矩阵运算](#3_Eigen3_42)
		- [4. 安装ORB-SLAM3](#4_ORBSLAM3_45)
	+ [:satisfied:3. 案例运行](#satisfied3__67)
	+ - [1. 运行数据集](#1__68)
		- [2. 用真实相机usb\_cam运行](#2_usb_cam_84)




### 😏1. ORB-SLAM3介绍


ORB-SLAM3是一种基于视觉传感器的**实时单目、双目和RGB-D SLAM系统**。


SLAM代表同时定位与地图构建，是指在未知环境下通过机器人上搭载的传感器获取数据并运用算法进行实时处理，从而在机器人运动中同时完成对机器人自身姿态的估计和构建三维环境地图。


ORB-SLAM3是由英国伯明翰大学开发的，是ORB-SLAM2的改进版本，加入了语义信息处理，能够更加准确地估计相机的位置和方向，并且可以识别场景中的物体和结构，实现更加智能化的SLAM过程。


### 😊2. 代码安装部署


#### 1. 安装ros与opencv


安装依赖：



```
sudo apt-get update
sudo apt-get install git cmake build-essential libglew-dev libgtk2.0-dev \
libavcodec-dev libavformat-dev libswscale-dev libjpeg-dev libpng-dev libtiff5-dev \
libopenexr-dev libeigen3-dev libboost-all-dev libprotobuf-dev protobuf-compiler \
libgoogle-glog-dev libgflags-dev libatlas-base-dev liblapack-dev libsuitesparse-dev \
libvtk6-dev python3-pip python3-dev python3-numpy python3-yaml


```

安装完ros后，会自带`opencv 3.2.0`版本。



```
pkg-config --modversion opencv

```

#### 2. 安装Pangolin作为可视化和用户界面


安装依赖：`sudo apt-get install libglew-dev libpython2.7-dev libgl1-mesa-dev libegl1-mesa-dev libwayland-dev libxkbcommon-dev wayland-protocols`


下载代码：`git clone https://github.com/stevenlovegrove/Pangolin.git`


编译安装：



```
cd Pangolin
mkdir build
cd build
cmake ..
make -j8
sudo make install

```

#### 3. 安装Eigen3一个开源线性库，可进行矩阵运算


安装eigen3：`sudo apt-get install libeigen3-dev`


#### 4. 安装ORB-SLAM3


下载代码：`git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git ORB_SLAM3`


构建ORB-SLAM3：



```
cd ORB_SLAM3
chmod +x build.sh
./build.sh

```

可能的问题：AR编译不通过，删除即可；se3库有问题，重新编译对应库。


构建ROS版本的ORB-SLAM3：



```
gedit ~/.bashrc
export ROS\_PACKAGE\_PATH=${ROS\_PACKAGE\_PATH}:~/ORB_SLAM3/Examples_old/ROS

chmod +x build_ros.sh
./build_ros.sh

```

可能的问题：改一下`CMakeList.txt`中的参数。


### 😆3. 案例运行


#### 1. 运行数据集


编译完成后会在ORB\_SLAM3/Examples文件夹下生成各种可执行文件。这里以单目相机为例运行：


![在这里插入图片描述](https://img-blog.csdnimg.cn/727b1a179dfe4da1b13dd03c5cf774c7.png)


1. 下载数据集：有TUM、KITTI、EuRoC三种数据集，这里使用TUM数据集，从`https://cvg.cit.tum.de/data/datasets/rgbd-dataset/download`下载序列并解压缩。
2. 执行命令，其中`PATH_TO_SEQUENCE_FOLDER`为数据集的存储路径，并将`tumx.yaml`与下载的数据集对应，比如`TUM1.yaml,TUM2.yaml 和TUM3.yaml`分别对应 freiburg1, freiburg2 和 freiburg3，根据自己的情况对路径（相对路径或绝对路径）做修改。



```
./Examples/Monocular/mono_tum /home/dev/ORB_SLAM3/Vocabulary/ORBvoc.txt /home/dev/ORB_SLAM3/Examples/Monocular/TUM1.yaml /home/dev/ORB_SLAM3/dataset/rgbd_dataset_freiburg1_xyz

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ba56d0e3f714436ba1cf795c714fd889.png)


#### 2. 用真实相机usb\_cam运行


ORB\_SLAM3自带了ros的版本，在Examples\_old/ROS中，编译完成后，先运行usb相机：



```
roslaunch usb_cam usb_cam-test.launch

```

然后执行单目vslam命令（注意将订阅的topic改成对应相机的）：



```
rosrun ORB_SLAM3 Mono /home/dev/ORB_SLAM3/Vocabulary/ORBvoc.txt /home/dev/ORB_SLAM3/Examples_old/ROS/ORB_SLAM3/Asus.yaml

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2222fa2f1ffa430cb43edb396d7c64b1.png)


以上。





