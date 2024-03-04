







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍fsd\_algorithm架构学习与源码分析（致敬）。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 架构学习](#blush2__19)
	+ [:satisfied:3. 源码学习](#satisfied3__48)
	+ - [perception](#perception_51)
		- [estimation](#estimation_55)
		- [planning](#planning_59)
		- [control](#control_63)




### 😏1. 项目介绍


------------------ 叮叮叮！！！ ------------------


大学生无人驾驶方程式有两支很牛的车队，国外苏黎世联邦理工的`AMZ-Driverless`和国内北理的`Smart Shark-BITFSD`，大佬们的无人驾驶车和算法做的很顶（这里就不方便放视频了），贴一下他们的网站和Github（大佬们将稳定的算法和仿真平台开源了并分享在github，给我等学习mobai）：


AMZ官网：`https://www.amzracing.ch/`


AMZ Github地址：`https://github.com/AMZ-Driverless/fssim`


BITFSD官网：`http://www.bitfsd.com/`


BITFSD Github地址：`https://github.com/bitfsd/fsd_algorithm`


### 😊2. 架构学习


------------------ 架构很棒 ------------------


环境配置建议选择：**Ubuntu 18.04 and ROS Melodic**。


`fsd_algorithm`算法仓库包含ros和tools。


tools提供了ros模板的生成，可通过py脚本`generate.py`自助选择生成对应语言（C++/Python）、对应节点名（包名、对象名和类名）的ros节点。


ros中包含了fsd的核心算法，如`perception`包、`estimation`包、`planning`包、`control`包和与仿真器连接的`interface_fssim`包。


环境配置过程如下：



```
# 1.clone，将ros下的包cp到自己的catkin\_ws/src中
# 进入ros/control/controller/script，安装cppad和ipopt两个优化库
# 编译 catkin build

# 2.clone fssim仿真仓库到另一个catkin中
# 安装依赖，下载gazebo的models等
# 编译 catkin build

# 3.进入仿真项目环境，启动仿真环境 roslaunch fssim auto\_fssim.launch
# 然后启动算法包里的仿真接口 roslaunch fssim\_interface fssim\_interface only\_interface.launch
# 最后运行相关算法：
# roslaunch fsd\_common\_meta trackdrive.launch
# roslaunch fsd\_common\_meta skidpad.launch
# roslaunch fsd\_Common\_meta acceleration.launch

```

### 😆3. 源码学习


------------------ 代码写的也不错 ------------------


#### perception


**perception模块**包含YOLO-ROS（darknet\_ros）的目标检测包，用coco数据集训练，可配置参数文件在`darkned_ros/config/ros.yaml`，订阅的话题是`/camera/rgb/image_raw`，发布的话题有`/darknet_ros/found_object`、`/darknet_ros/bounding_boxes`和`/darknet_ros/detection_image`，此外还有动作发送`/darknet_ros/check_for_objects`，以此实现目标检测结果的获取；激光雷达聚类包`Lidar Cluster`，基于PCL，订阅的是威力登的点云`/velodyne_points`，发布的是聚类结果`/perception/lidar_cluster`，参数配置在`./config/lidar_cluster.yaml`，在lidar\_cluster中，用`preprocessing`先对点云进行过滤，再用`ClusterProcessing`进行聚类处理。


#### estimation


**estimation模块**主要是`loam`建图定位和`robot_localization`定位包，用扩展卡尔曼和无损卡尔曼等方法获取车辆的精确位置和位姿信息。


#### planning


**planning模块**有边界检测、线检测、8字检测和路径生成这几个包。边界检测boundary\_detector的核心思想是搜索和选择，基于OpenCV3，订阅`/local_map`，发布`/planning/boundary_detections`和其他几个显示话题，基于地图边界信息生成最优路径和边界结果；线检测line\_detector用到了霍夫变换，订阅雷达聚类结果`/perception/lidar_cluster`，发布全局路径`/planning/global_path`，可以看到本仓库的算法的模板是`getNodeRate + loadParameters + subscribeToTopics + publishToTopics + run + sendMsg 和一个callback函数`，这个包核心在于`createPath`创建全局路径这里；线生成Path Generator包应该是进行路径优化，会根据不同的任务生成不同的参考路径，如直线加速是根据目标点参数，8字是根据转换矩阵，循迹任务是根据地图信息，最后生成控制指令并发布。


#### control


**control模块**用到了`cppad`和`ipopt`依赖，因为控制中用到了许多数值优化的方法，主要是根据slam地图状态和参考轨迹信息，计算出安全且舒适的控制指令发布到底层，也是分了3种工况。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





