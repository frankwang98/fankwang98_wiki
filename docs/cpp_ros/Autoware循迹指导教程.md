# 基于Autoware的线控底盘循迹自动驾驶教程
2022.7.14-整理v1.0
2022.8.9-完善v2.0

## 1. 感知系统
感知系统包含环境感知、导航定位、网联感知。

感知系统的目标是提供给下游模块尽可能多和准确的定位和感知数据，包含但不限于自车位置和姿态、障碍物位置和姿态、有用信息的提取。

## 2. 决策系统
决策系统包含算法、应用软件、系统软件、芯片，这四个模块共同组成了决策大系统。

## 3. 执行系统
执行系统包含线控底盘各个部位的控制。

## 4. 数字孪生仿真系统
数字孪生仿真能测试算法，并给观者带来良好的体验。

## 5. 云控调度系统

## 一、准备工作
1.工控机安装Ubuntu18.04系统；
2.安装相应ROS版本ROS-Melodic；
3.工控机配置16线激光雷达网段ip，用于接收激光雷达UDP包；确认激光雷达安装角度是否为正方向，坐标系是否正确；
4.安装Autoware1.13版本，测试demo运行；
5.配置Linux-CAN驱动；
6.若需要gps定位，提前配置gps驱动；

## 二、构建点云地图
### 1.录制点云数据
1）首先启动runtime_manager
`source install/setup.bash && roslaunch runtime_manager runtime_manager.launch`
2）启动RoboSense 16线激光雷达(或其他）
`bash run_rslidar.sh`
这时可以通过命令rostopic echo /points_raw 查看是否有点云数据输出。
3）录制点云数据
点击runtime manager右下角[ROSBAG]按钮，在弹出的对话框中点击[Refresh]按钮刷新话题列表，找到/points_raw并勾选。
点击[Ref]，选择想要保存的文件路径。老司机开车，点击 [Start] 按钮，开始记录数据，等数据录完时，点击 [Stop]。
数据回放：rosbag play xxx.bag
### 2.创建pcd点云地图
进入[Simulaton]页面，点击界面右上方[Ref]按钮，加载之前录制的 bag文件，点击[Play]按钮播放数据，然后再点击[Pause]暂停播放。
1）设置从base_link到velodyne坐标系的TF
在[Setup]菜单中，确保[Localizer]下选项为 [Velodyne]，在[Baselink to Localizer] 中设置好各个参数之后点击TF按钮，其中x、y、z、yaw、pitch、roll表示真车雷达中心点与车身后轴中心点的相对位置关系（右手坐标系，真车后车轴为原点），此时可以点击[Vehicle Model]，如果[Vehicle Model]为空，那么会加载一个默认模型（在rviz显示时，如果有激光雷达数据，车辆会显示为黑色）。
2）设置world到map转换
点击[Map]页面，点击[TF]的[ref]选择tf.launch文件，这是加载默认world到map的坐标转换，打开tf.launch文件如下： 
<launch>
    <node pkg="tf" type="static_transform_publisher" name="world_to_map" args="0 0 0 0 0 0 /world /map 10" />
</launch>
 args的参数“0 0 0 0 0 0 /world /map 10”表示：从/world坐标系转换到/map坐标系的x, y, z, roll, pitch, yaw转换，且频率为10Hz。
点击 [TF] 按钮，控件变为蓝色即启动。
3）运行ndt_mapping算法
在[Compulting]菜单栏中找到[lidar_localizer]下的[Ndt_Mapping]选项，设置 [app]，并勾选Ndt_Mapping。
4）制作点云地图
注意：由于rviz会占用大量的系统资源，所以在建图过程中不需要打开rviz显示，只需要查看terminal上的显示即可。
terminal显示：(Process/Input）: (1688/1697），这两个数字前一个数字表示正在处理的点云帧数，后一个表示加载的点云帧数。如果两个数字相差过大，会出现运行错误。如果前后两个数字相差1000以上，就要按[Simulaton] 页面的[Pause] 按键，暂停加载，等待一下正在处理的数字，两个数字重新接近之后，可以再次按[Pause]按键运行。
运行过程中，可以多次保存点云地图（PCD OUTPUT）。运行停止，表示地图生成完毕。
5）保存点云地图
打开[Computing]下的[Ndt_Mapping]选项的 [app]，如下所示：
点击[ref]选择保存地图的路径，将 [Filter Resolution] 参数设置为0.2，点击[PCD OUTPUT]按钮，开始保存pcd文件。此时，可以在保存的路径里看到一个.pcd文件，该文件就是点云地图文件。
6）查看地图文件
选择runtime manager的[Map]菜单，点击[Point Cloud]按钮的 [ref]，加载刚才保存的.pcd文件，并点击[Point Cloud]按钮，进度条显示OK，则加载完毕，如下所示：
点击runtime manager 右下角的 [RViz] ，在RViz显示界面点击左下角的 [Add] 按钮，通过 [By Topic] 找到/points_map 并打开，在rviz中查看显示。
到此为止，点云地图制作完毕。

## 三、基于点云的定位
构建出地图后，应该测试点云地图定位效果，这里用到ndt的scan_matching方法，这是一种scan-to-map方法。
### 1.模拟定位
1）打开Autoware,在[Simulation]中播放Bag文件,然后点击[Pause]暂停播放。
2）在[Setup]下点击[TF]按钮,并确定Localizer选项位于[Velodyne]处,同时确保参数配置正确。
3）在[Map]中加载之前建立的PCD地图和TF文件并加载生效。
4）在[Sensing]下找到右边[voxel_grid_filter]选项并勾选。
5）找到[Computing]下的[ndt_matching]选项,打开[app],确保[topic:/config/ndt]选项处于[Initial_Pose]处,如果有GPU则把[Method_Type]更改为[pcl_anh_gpu],退出并勾选ndt_matching。
6）找到[Computing]下的[vel_pose_connect],打开[app]并确保选项[Simulation_Mode]没有被勾选,退出并勾选vel_pose_connect。
7）打开[Rviz],默认显示即可。
8）回到Autoware界面后,在[Simulation]下重新播放Bag数据。
9）在Rviz界面中会出现显示,如定位不准,点击rviz界面中的2D Pose Estimate,在地图上的正确位置帮助雷达定位。
模拟定位的问题多集中于TF,请确认rviz界面中左侧System下的TF信息提示,必要时重启Autoware。
### 2.实车定位
1）在确保之前的配置全部取消的情况下(主要保证没有数据在回放）,在[Setup]下点击[TF]按钮,并确定Localizer选项位于[Velodyne]处,同时确保参数配置正确。
2）在[Map]中加载之前建立的PCD地图和TF文件并加载生效。
3）[Sensing]下点击[Velodyne VLP_16]启动VLP16 Velodyne激光雷达（或其他雷达）。
4）[Sensing]下找到[voxel_grid_filter]选项并勾选。
5）找到[Computing]下的[ndt_matching]选项,打开[app],确保[topic:/config/ndt]选项处于[Initial Pose]处,如果有GPU则把[Method Type]更改为[pcl_anh_gpu],退出并勾选ndt_matching。
6）找到[Computing]下的[vel_pose_connect],打开[app]并确保选项[Simulation Mode]未被勾选,退出并勾选vel_pose_connect。
7）打开[Rviz],默认显示或使用自编辑保存的Rviz均可。
如果出现定位偏差,则用2D Pose Estimate箭头进行辅助定位,其中箭头尾部代表真实车辆的当前大概位置,箭头表示车辆的车头朝向。

## 四、雷达目标检测
### 1.雷达聚类检测
1）在上一节真实定位或者模拟定位的前提下,勾选[Computing]子界面下的[Lidar_euclidean_cluster_detect]。
2）打开[Rviz],默认显示或使用自编辑保存的Rviz均可。
3）在Rviz左下角点击[Add]添加一个名为:[BoundingBoxArray]的选项。
4）加载之后,将该选项展开栏下的[Topic]修改为:/bounding_boxes。
5）此时在Rviz界面中会出现右图所示方框,这就是利用激光雷达的聚类检测结果。

## 五、路径点录制
### 1.离线录制航迹点
航迹点用于引导车辆运行，autoware的每个航迹点包含x, y, z, yaw, velocity信息。
航迹点录制有两种方式，可以开车录制航迹点，也可以采集数据包，离线录制航迹点。
录制航迹点非常考验老司机的车技，车要开的稳，尤其是弯道，尽量要开成圆弧，而且要尽量远离马路牙。
注意：所有需要在 [Simulation] 菜单下加载的数据，都需要在所有操作之前操作，否则在RViz显示时，会出现frame_id错误。
在我们的项目中，航迹点是车在运动过程中，车载lidar与地图匹配形成的一系列的位置信息构成的。所以定位就是录制航迹点的基础。
1）打开runtime manager，进入[Simulaton]页面，点击界面右上方[Ref]按钮，加载录制用于定位的bag文件，点击 [Play] 然后点击 [Pause]暂停。
2）加载pcd地图，加载world到map以及base_link到velodyne的TF。
3）进入[Sensing]页面，开启[Points Downsampler]下的[voxel_grid_filter] 。
4）选择[Computing]左菜单栏下的[ndt_matching]。
5）启动[vel_pose_connect]
6）记录航迹点，在菜单栏[Computing]右边的[waypoint_maker]下的[waypoint_saver]。点击[app]按钮，在打开的界面中点击[Ref]指定保存路径和文件名后点击[OK]。
7）回放数据，打开[Rviz],回到runtime manager，进入[Simulaton]页面，点击 [Pause]开始。
8）在RViz显示waypoints记录过程。打开[Rviz]，在Rviz左下方找到[Add]。显示框中找到图示[By topic]下话题/waypoint_saver_marker的[MakerArray]并添加。
9）保存记录数据。车子停止后，回到runtime manager的[Computing]菜单栏下，取消[waypoint_saver]勾选（自动保存waypoint）。
注意：确保在此之前更改了文件名和保存路径，在指定的路径下将会看到对应文件。
### 2.实车录制航迹点
1）启动激光雷达，确认数据正常。
2）打开runtime manager，加载pcd地图，加载world到map以及base_link到velodyne的TF。
3）进入[Sensing]页面，开启[Points Downsampler]下的[voxel_grid_filter] 。
4）选择[Computing]左菜单栏下的[ndt_matching]。
5）启动[vel_pose_connect]
6）记录航迹点，在菜单栏[Computing]右边的[waypoint_maker]下的[waypoint_saver]。点击[app]按钮，在打开的界面中点击[Ref]指定保存路径和文件名后点击[OK]。
7）保存记录数据。老司机开车，到达目标后，回到runtime manager的 [Computing] 菜单栏下，取消 [waypoint_saver] 勾选（自动保存waypoint）。
在老司机开车的过程中，可以同时看到航迹点在地图上逐渐显示出来。
注意：确保在此之前更改了文件名和保存路径，在指定的路径下将会看到对应文件。

## 六、路径跟随测试
### 1.仿真循迹测试
采用循迹方式，首先要做的就是要确定车辆离航迹点集上的哪个点最近，然后通过control算法将车辆移动到该航迹点上。所以循迹的核心同样是定位。
1）打开Autoware,[Simulation]中打开之前录制的Bag文件,播放后点击暂停播放。
2）找到[Setup]并勾选[TF]选项,确保参数都配置正确。
3）点击[Setup]下的[Vehicle Model]可以加载车辆模型,不用选择任何文件即可。
4）找到[Map]并点击加载[Point Cloud]点云地图和[TF],确保文件加载正确。
5）[Sensing]下勾选[voxel_grid_filter]。
6）[Computing]下勾选[ndt_matching]和[vel_pose_connect],并确保ndt_matching配置中[topic:/config/ndt]位于[Initial Pos],vel_pose_connect没有选择[Simulation]模式。加载完后打开[Rviz]查看定位效果。
7）[Computing]下勾选右边[lane_rule],[lane_stop],[lane_select]三个选项。
8）[Computing]下勾选[astar_avoid]和[velocity_set]两个选项。
9）[Computing]下打开[waypoint_loader]右边的[app]选项,在弹出的对话框中选择[Ref]加载之前保存下来的路点文件,退出并勾选[waypoint_loader]。
10）[Computing]下,找到[pure_pursuit]并点击右边[app],默认设置。也可勾选[Dialog]选项,确保[Velocity](手动设置速度）选项值不会过大。[解释:此处用于路点跟随中的速度设置。[Waypoint]模式下执行路点文件中记录的车速,已经被写入路点文件中;而[Dialog]模式则可以手动设置车辆速度。] 勾选[twist_filter]，默认设置。
11）打开[Rviz],默认显示或使用自编辑保存的Rviz均可。
12）回到Autoware界面下的[Simulation]子页面中,取消暂停播放,回到Rviz中,界面会出现路径。
### 2.实车循迹测试
实车循迹测试是用真实的传感器数据和定位信息，通过对车辆航迹点的规划，实现让车自动行驶的效果。
1）打开Autoware,启动激光雷达。
2）找到[Setup]并勾选[TF]选项,确保参数都配置正确。
3）点击[Setup]下的[Vehicle Model]可以加载车辆模型,不用选择任何文件即可。
4）找到[Map]并点击加载[Point Cloud]点云地图和[TF],确保文件加载正确。
5）[Sensing]下勾选[voxel_grid_filter]。
6）[Computing]下勾选[ndt_matching]和[vel_pose_connect],并确保ndt_matching配置中[topic:/config/ndt]位于[Initial Pos],vel_pose_connect没有选择[Simulation]模式。加载完后打开[Rviz]查看定位效果。
7）[Computing]下勾选右边[lane_rule],[lane_stop],[lane_select]三个选项。
8）[Computing]下勾选[astar_avoid]和[velocity_set]两个选项。
9）[Computing]下打开[waypoint_loader]右边的[app]选项,在弹出的对话框中选择[Ref]加载之前保存下来的路点文件,退出并勾选[waypoint_loader]。
10）[Computing]下,找到[pure_pursuit]并点击右边[app],默认设置或自定义。勾选[twist_filter]，默认设置。
11）打开[Rviz],可以看到实车的定位和路径规划信息。
12）发送CAN数据与车辆通信，找到子页面[Interface]下的[Vehicle Gateway]并勾选,该项功能是将控制信息发送给实车。(本操作中控制命令通过Socket发给车辆。如需直接发送CAN消息可通过CAN卡/口自写节点发送。)驱动已写好，在这里执行 `bash run_can.sh` 即可。
解释：只要不断接收autoware_msgs的ctrl_cmd/twist_cmd实时的转向、油门、制动消息，并通过CAN通信下发给车辆控制器，就可以驱动车辆动起来。
编写ROS节点canCtrl，回调函数接收ctrl_cmd/twist_cmd实时消息，while(ros::ok(））{......}代码里实时发送线控指令。

## 七、基于GNSS的定位
前面我们采用ndt_matching的方法实现定位，但需要在rviz中，通过2D Pose Estimate指定初始位置。加入GNSS后，可以帮助ndt_matching找到初始位置，同时如果ndt_matching在运动过程中匹配失败，GNSS可以帮助自动重定位。
在/ndt_matching算法中，有两个条件使用GNSS重定位：
(1) 如果设置ndt_matching的config中设置了GNSS，而非Initial Pos，那么ndt_matching会在程序初始运行中采用GNSS作为初值；
(2) 在车辆运行过程中，当topic /ndt_stat中的score值大于600（与地图匹配程度，值越大匹配的越差），同样会使用GNSS重定位。
### 1.GNSS设备配置
1）安装串口驱动`sudo apt-get install ros-melodic-nmea-navsat-driver`
2）在Autoware的[Sensing]中设置为串口读入[Serial GNSS]，设置名称/dev/ttyS0,波特率115200
3）勾选Serial GNSS选项，通过查看topic /nmea_sentence，如果有输出，且在sentence中有内容，即GNSS设备数据读取成功。
4）nmea-0183主流的标准有$GPGGA、$GPGSA、$GPGSV、$GPRMC、$GPVTG、$GPGLL。不同的GNSS设备，sentence的中内容的标准会有不同。如果采用的传感器不是这些标准，所以需要自己写nmea的数据解析。如果想了解nmea，请参考博客：[https://blog.csdn.net/jickjiang/article/details/79086202]
### 2.数据解析
Autoware的数据解析程序为nmea2tfpose包，如果采用的设备不是标准协议，可以在$GPGGA的基础上做改进nmea2tfpose_core.cpp。
1）勾选[nmea2tfpose]。
2）[Computing]下打开[ndt_matching]右边的[app]选项,在/config/ndt中选择[GNSS]即可。

以上！
