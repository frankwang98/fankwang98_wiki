







> 
> 前言：ROS的出现使得机器人软件开发更加快速和模块化，在此基础上，Autoware.ai开源项目可以让我们很容易地将一套完整的自动驾驶软件部署到我们的测试车辆上，并见证它跑起来！
> 
> 
> 




#### 文章目录


* + [1.Autoware简介](#1Autoware_3)
	+ [2.电脑软硬件配置要求](#2_12)
	+ [3.Ubuntu 18.04系统](#3Ubuntu_1804_26)
	+ [4.ROS Melodic安装](#4ROS_Melodic_29)
	+ - [ROS安装](#ROS_30)
		- [配置rosdep update](#rosdep_update_53)
	+ [5.Qt 5.12.0框架安装](#5Qt_5120_63)
	+ - [安装Qt](#Qt_64)
		- [配置系统路径](#_88)
	+ [6.Autoware 1.13自动驾驶软件安装](#6Autoware_113_118)
	+ - [安装系统软件依赖](#_121)
		- [建立工作空间](#_133)
		- [安装autoware软件依赖](#autoware_155)
		- [开始编译](#_162)
	+ [7.其他](#7_188)




### 1.Autoware简介


Autoware是一款`“一体化”开源自动驾驶软件`，能实现感知、决策、控制等功能，通过在Ubuntu中搭建Autoware开发环境和案例的运行，使大家对自动驾驶技术的实现有一个更清晰的认识。


![在这里插入图片描述](https://img-blog.csdnimg.cn/2f3749aeb6df447ea957935c6cef8ab1.png)


软件架构图如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c1c54f8df17c4c2cb4d50af6af62ca81.png)


### 2.电脑软硬件配置要求


* 硬件要求：
	+ 处理器-推荐i7，最低i5
	+ 运行内存-16G及以上
	+ 硬盘存储-100G及以上
	+ 显卡-暂无要求
* 软件要求：
	+ 操作系统-Ubuntu 18.04
	+ 框架&中间件-ROS Melodic
	+ 界面框架-Qt 5.12.0
	+ 自动驾驶软件-Autoware 1.13


由于大多数学习者电脑没有GPU，以下安装仅适用于`Autoware-cpu`版本。


### 3.Ubuntu 18.04系统


推荐安装双系统，安装方法不再赘述！


### 4.ROS Melodic安装


#### ROS安装


推荐使用“鱼香ROS”大佬的一键安装命令：  
 `wget http://fishros.com/install -O fishros && . fishros`


（注意：根据自身情况选择“是否更新源”、“ROS版本”、“桌面版/精简版”）  
 安装完成之后，通过 `roscore` 命令测试主节点，输出如下信息表示安装成功：



```
PARAMETERS
 * /rosdistro: melodic
 * /rosversion: 1.14.7

NODES

auto-starting new master
process[master]: started with pid [1215]
ROS\_MASTER\_URI=http://nx:11311/

setting /run_id to cb38e680-dee2-11ea-bae1-70665563e003
process[rosout-1]: started with pid [1228]
started core service [/rosout]

```

#### 配置rosdep update


rosdep update自动更新ros源的实现：


1. 下载脚本：`wget https://gitee.com/ncnynl/rosdep/raw/master/rosdep_update.sh` ;
2. 管理员给定执行权限：`sudo chmod +x ./rosdep_update.sh`;
3. 管理员运行脚本：`sudo ./rosdep_update.sh`
4. 出现这一行，代表成功：`all files replaced is finished, please continues run rosdecp update`
5. 然后依次执行：`sudo rosdep init` 和 `rosdep update`即可。  
 ------------**失效，可用小鱼工具[3]rosdepc**---------------




---


### 5.Qt 5.12.0框架安装


#### 安装Qt


打开浏览器，在地址栏输入下面地址：



```
http://mirrors.ustc.edu.cn/qtproject/archive/qt/5.12/5.12.0/qt-opensource-linux-x64-5.12.0.run

```

将会自动下载如下软件包：



```
qt-opensource-linux-x64-5.12.0.run

```

进入“下载”目录下，打开终端，改变执行权限并安装：  
 （**注意，安装Qt时请断开网络连接！安装路径请放置在/opt/Qt5.12.0,选择需要的Qt模块**）



```
sudo chmod +x qt-opensource-linux-x64-5.12.0.run

```


```
sudo ./qt-opensource-linux-x64-5.12.0.run

```

#### 配置系统路径


安装完成之后，需要配置系统路径，可解决找不到头文件、无法添加文件等问题。  
 打开终端，输入：



```
sudo gedit /etc/bash.bashrc

```

在文件末尾添加：



```
export QTDIR=/opt/Qt5.12.0/5.12.0/gcc_64
export PATH=$QTDIR/bin:$PATH
export LD\_LIBRARY\_PATH=$QTDIR/lib:$LD\_LIBRARY\_PATH

```

保存后在终端执行：



```
source /etc/bash.bashrc

```

要确认是否添加成功，可输入如下命令（输出Qt的路径表示配置成功）：



```
echo $PATH

```

至此，Qt creater安装完成，也可以进行qt开发。


### 6.Autoware 1.13自动驾驶软件安装


因为Autoware1.14版本有很多BUG，目前还没有修复，用1.14版本的有很多包都是从1.13版本移植过来的，但1.12版本又缺失了很多模块，因为取其中选择了1.13版。


#### 安装系统软件依赖



```
sudo apt-get update
sudo apt-get install -y python-catkin-pkg python-rosdep ros-melodic-catkin
sudo apt-get install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
pip3 install -U setuptools

```

（如果有错误用下面这条语句解决，无错请跳过！）  
 python3.6 -m pip install launchpadlib


#### 建立工作空间



```
mkdir -p autoware.ai/src
cd autoware.ai

```

下载源码或者用我给定的源码（替换掉src文件夹即可）：



```
wget -O autoware.ai.repos https://gitlab.com/autowarefoundation/autoware.ai/autoware/raw/1.13.0/autoware.ai.repos?inline=false
vcs import src < autoware.ai.repos

```



---


230921更新：


这里提供一份源码，需要自取：`https://gitee.com/frankwang98/autoware.ai.git`




---


#### 安装autoware软件依赖



```
rosdepc update
rosdepc install --from-paths src --ignore-src --rosdistro=melodic -y

```

#### 开始编译


编译cpu版本的autoware(注：如果更改了源码，即src文件夹，重新编译autoware工作区即可！)



```
colcon build --cmake-args -DCMAKE\_BUILD\_TYPE=Release

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8c44178149b8418cbb65836367e2365d.png)


（正常情况下，编译成功164个packages！）


启动autoware：



```
source install/setup.bash
roslaunch runtime_manager runtime_manager

```

界面如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f706197c976342acb63f7efc8227721d.png)


至此，Autoware 1.13安装完成！恭喜你在自动驾驶道路上又前进了一步！！！


### 7.其他


Q1：citysim编译报错  
 A1：电脑安装了其他protobuf版本，需要适配到protobuf3.0.0


以上。





