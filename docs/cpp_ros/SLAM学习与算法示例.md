### Cartographer与Cartographer_ros编译运行
创建工作空间 进入工作空间安装rosdep先决依赖
```sh
mkdir -p ~/cartographer_ws/src 
cd ~/cartographer_ws
sudo apt install python3-pip python-pip -y
```
安装配置rosdep（如果不管用，可以查看ros安装部分的rosdep更新方法）
```sh
##换源建议换阿里源+中科大ROS源
##安装rosdep 使用小鱼的rosdepc完美安装
pip3 install rosdepc
sudo rosdepc init
rosdepc update
```
安装编译cartographer的必要依赖
```sh
sudo apt install -y python-wstool python-rosdep ninja-build 
```
从gitee上下载cartographer和cartographer_ros的源码 我这里随便找了两个人的下载了一下
```sh
cd ~/cartographer_ws/src 
git clone https://gitee.com/c1h2/cartographer_ros.git
git clone https://gitee.com/LauZanMo/cartographer.git
```
进入安装abseil
```sh
sudo apt-get install stow -y
cd ~/cartographer_ws/src/cartographer/scripts
./install_abseil.sh

cd ~/cartographer_ws
##这里的话会把依赖都安装上包括lua glog protobuf等
sudo rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
##编译安装cartographer 这里等待10分钟即可安装成功
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```
到这里就完美安装成功了。

### 雷达测试与LeGO-LOAM运行

#### LeGO-LOAM
LeGO-LOAM是Tixiao Shan提出的⼀种**基于LOAM的改进激光SLAM框架**，主要是为了实现小车在多变地形下的定位和建图，其针对前端和后端都做了⼀系列的改进。目前已在台架上测试成功！

LeGO-LOAM全称为：Lightweight and Groud-Optimized Lidar Odometry and Mapping on Variable Terrain，从标题可以看出 LeGO-LOAM 为应对可变地面进行了地面优化，同时保证了轻量级。

LeGO-LOAM是**专门为地面车辆设计的SLAM算法**，要求在安装的时候Lidar能以水平方式安装在车辆上；如果是倾斜安装的话，也要进行位姿转换到车辆上。而LOAM对Lidar的安装方式没有要求，即使手持都没有关系。

##### 算法框架
LeGO_LOAM的软件系统输入 3D Lidar 的点云，输出 6 DOF 的位姿估计。
整个软件系统分为 5 个部分：
```
第一部分：Segmentation： 这一部分的主要操作是分离出地面点云；同时对剩下的点云进行聚类，滤除数量较少的点云簇。

第二部分：Feature Extraction： 对分割后的点云（已经分离出地面点云）进行边缘点和面点特征提取，这一步和LOAM里面的操作一样。

第三部分：Lidar 里程计： 在连续帧之间进行（边缘点和面点）特征匹配找到连续帧之间的位姿变换矩阵。

第四部分：Lidar Mapping： 对feature进一步处理，然后在全局的 point cloud map 中进行配准。

第五部分：Transform Integration： Transform Integration 融合了来自 Lidar Odometry 和 Lidar Mapping 的 pose estimation 进行输出最终的 pose estimate。
```

#### 测试
1. 雷达网络配置		
首先安装好雷达在平台上，供电准备好，雷达网口接终端，雷达本机ip是192.168.1.200，终端ip要配置成192.168.1.102，子网掩码255.255.255.0即可。
终端ping 192.168.1.200可以ping通则说明雷达通讯没问题。

2. 雷达驱动安装		
sdk？driver？应该都可以（先用sdk就可以）

3. 依赖包		
安装pcap：`sudo apt-get install -y libpcap-dev`
安装Yaml：`sudo apt-get install -y libyaml-cpp-dev` （若已安装ROS desktop-full, 可跳过）

4. 修改CMakeLists.txt和package.xml		
（1） 将文件顶部的`set(COMPILE_METHOD ORIGINAL)`改为`set(COMPILE_METHOD CATKIN)`		
（2） 将`set(POINT_TYPE XYZI)` 改为`set(POINT_TYPE XYZIRT)`		
（3） 将rslidar_sdk工程目录下的package_ros1.xml文件重命名为`package.xml`。		
   		修改 config.yaml 参数
rslidar_sdk只有一份参数文件 config.yaml， 储存于`rslidar_sdk/config`文件夹内。打开此文件，找到以下部分：
激光雷达型号默认RS128，修改为自己的激光雷达型号即可

5. 编译		
建立ros工作空间，在工作空间编译

6. GitHub下载rs雷达话题转velodyne雷达话题源码		
（已下载）编译，可放在同个工作空间编译

7. 编译LeGO-LOAM			
前面有讲，可放在同个工作空间编译
但是这里要修改LeGO-LOAM的launch文件：
将这句话的true改成false
`<param name="/use_sim_time" value="false"/>`

8. 运行所有节点和launch文件		
（1）先运行雷达：`roslaunch rslidar_sdk start.launch`		
（2）再运行rs_to_velodyne节点:`rosrun rs_to_velodyne rs_tovelodyne XYZIRT XYZIR `		
（这里要注意LeGO-LOAM需要的雷达点云是XYZIR格式的，话题还是velodyne_points）		
（3）最后运行lego_loam:`roslaunch lego_loam run.launch`		

9. 效果如下		
!!!

10. 修改话题的其他办法		
LeGO-LOAM的launch文件增加：
`<remap from="/rslidar_points" to="/velodyne_points"/>`
但是要修改雷达ROS驱动的CMakeLists.txt
将set(POINT_TYPE XYZI) 改为`set(POINT_TYPE XYZIR)`，并重新编译。

11. 保存数据		
录制地图包：`rosbag record -o mybag.bag out /laser_cloud_surround`
查看：`rosrun pcl_ros bag_to_pcd mybag.bag /laser_cloud_surround mypcd`
`pcl_viewer xxx.pcd` （这里是查看一帧的数据）
录制点云包：`rosbag record -o mybag.bag out /velodyne_points`

12. 和其他框架对比
A-LOAM框架室外跑的效果没有拍下来，录的bag也出了问题（血的教训，工控机别直接拔电关机，要把ROS下所有节点关闭后用指令poweroff，不然bag会有问题）
直观来看，在室外，LeGO-LOAM的效果要比A-LOAM要好。后面做完所有工作了，再对比不同的框架，比如固态雷达和机械式雷达对比，纯激光SLAM框架和LIO框架、lVIO框架对比，即对比多传感器和单传感器的效果。

##### 问题BUG
Q1. 解决error while loading shared libraries: libmetis-gtsam.so: cannot open shared object file
A1. $ sudo cp /usr/local/lib/libxxx.so usr/lib （找的位置不对，将这个库放到lib这里）

Q2. 遇到的问题：[mapOptmization-7] process has died
A2. 原因：可能是libmetis 库没有安装，安装libparmetis-dev可以解决
```
sudo apt-get update
sudo apt-get install libparmetis-dev
```
Q3. 遇到的问题：[ERROR] [1638358942.333987429]: Point cloud is not in dense format, please remove NaN points first!
A3. 找到utility.h将useCloudRIng设置为false，并重新编译。
以上。