> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍carla仿真环境安装与运行。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. Carla介绍

`Carla`是一个开源的无人驾驶仿真平台，用于训练和测试自动驾驶算法。它提供高度可配置的场景和传感器设置，模拟城市环境和交通情况，以帮助开发者评估他们的自动驾驶系统在各种现实世界场景下的表现。


`Carla`的目标是为研究人员、工程师和学生提供一个真实的仿真环境，以便快速迭代和测试他们的自动驾驶算法。它支持基于Python的API，使用户能够轻松地与仿真环境进行交互，并控制车辆、获取传感器数据等。


`Carla`具有逼真的图形渲染和物理模拟能力，可以模拟车辆的运动、感知和决策过程。它还支持多个传感器类型，包括相机、激光雷达和雷达等，以提供丰富的感知信息。用户可以根据自己的需求配置传感器设置，从而模拟不同的传感器布局和性能。


`Carla`的主要特点和功能包括：


> 1.真实感城市环境：CARLA提供了一个高度详细、真实感的城市环境，包括城市街道、高速公路、交叉口、停车场等。这个城市环境基于OpenStreetMap数据生成，具有丰富的道路网络和多样化的交通场景。

> 2.丰富的车辆和传感器模型：CARLA支持各种类型的车辆模型，包括轿车、卡车、自行车等，并提供了多种传感器模型，如相机、激光雷达、雷达和GPS等。开发者可以选择适合其应用场景的车辆和传感器配置。

> 3.真实物理模拟：CARLA使用准确的物理模拟来模拟车辆的动力学行为和传感器的测量数据。这使得开发者可以在虚拟环境中进行高度真实的测试和评估，而无需实际车辆和传感器。

> 4.可扩展的API和脚本支持：CARLA提供了Python API和脚本支持，使开发者能够通过编写Python代码来控制和监视仿真场景。这使得开发者可以自定义算法、收集数据、进行模拟实验等。

> 5.高度可定制的场景和交通设置：CARLA允许开发者自定义仿真场景，包括交通流量、行人行为、天气条件等。这使得开发者可以模拟各种现实世界的交通场景，并进行自动驾驶算法的测试和评估。

除了提供仿真环境外，Carla还提供了一套丰富的API和工具，用于收集和分析仿真数据。


`Carla`相关学习资源：



```bash
官网：https://carla.org/
Github：https://github.com/carla-simulator/carla
Wiki：https://carla.readthedocs.io/en/latest/
中文站：https://www.carla.org.cn/#/
0.9.11-release版本：https://github.com/carla-simulator/carla/releases/tag/0.9.11

```

### 😊2. carla环境配置


直接安装`release 0.9.11`版本，方便又快捷！源码真的不知道要折腾多久。


#### Windows


首先安装`DirectX`，在该地址下载即可：`https://www.microsoft.com/en-us/download/confirmation.aspx?id=35`


然后下载0.9.11的windows release版本，如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/358eab274bd24cb4ba1794da99dce82b.png)


双击运行即可：


![请添加图片描述](https://img-blog.csdnimg.cn/4c7bd28141a14ee0a1ae2cedd6edc288.png)


这里在windows下演示，安装好`python3.7`版本，可通过`python -V`查看。


安装依赖的pip包：`pip install numpy pygame networkx`


安装`carlib`：



```bash
# 打开powershell
cd D:\WindowsNoEditor\PythonAPI\carla\dist
# 安装egg
easy_install .\carla-0.9.11-py3.7-win-amd64.egg

```

运行client端脚本（前提是server端要打开，也就是双击运行的那个）：



```bash
cd D:\WindowsNoEditor\PythonAPI\examples
python .\automatic_control.py	# 自动运行车辆控制

```

执行后的演示如下：


![请添加图片描述](https://img-blog.csdnimg.cn/7eee02e11cfc48f8b601b853e5494ae2.png)


#### Ubuntu


Ubuntu安装类似，也是去下载Ubuntu的`release`版本，然后运行即可。



```bash
#运行Carla
./CarlaUE4.sh
#加80个随机车辆
cd ~/CARLA_0.9.11/PythonAPI/examples
./spawn_npc.py -n 80
#控制天气变化
./dynamic_weather.py
#手动驾驶client
./manual_control.py

```

Q：出现carla的time-out of 2000ms while waiting for the simulator  
 A：在manual\_control.py加一句`client=carla.Client(host='127.0.0.1', port=2000)`，多个client时port+1


#### 二次开发


Carla支持`python`和`c++`二次开发，但好像用源码编译的支持更好点。


我试了下C++创建`client`示例，加了好多库和头文件，也没有调试好。


python的话二次开发会好点，因为提供了一些python的`example`，比如手动控制示例，是用键盘控制的，也可以读取其他输入设备比如方向盘、手柄等。用python读取输入设备示例如下：



```py
import pygame

# 初始化pygame和joystick
pygame.init()
pygame.joystick.init()

# 检查是否有游戏控制器连接
if pygame.joystick.get_count() > 0:
    # 初始化第一个控制器
    joystick = pygame.joystick.Joystick(0)
    joystick.init()

try:
    while True:
        pygame.event.pump()

        # 获取方向盘的轴、按钮或帽子开关的状态
        axis0 = joystick.get_axis(0)
        axis1 = joystick.get_axis(1)
        axis2 = joystick.get_axis(2)

        # 处理轴的状态
        print("Axis0 value: ", axis0)
        print("Axis1 value: ", axis1)
        print("Axis2 value: ", axis2)

        # 确保延迟以避免过度占用处理器
        pygame.time.wait(100)

except KeyboardInterrupt:
    # 清理并退出
    pygame.quit()

```

### 😆3. carla-ros-bridge安装与仿真


源码安装步骤如下：



```sh
mkdir -p ~/carla-ros-bridge/catkin_ws/src
cd ~/carla-ros-bridge
git clone https://github.com/carla-simulator/ros-bridge.git(0.9.11)
cd catkin_ws/src
ln -s ../../ros-bridge-0.9.11
source /opt/ros/melodic/setup.bash
cd ..

rosdep update
rosdep install --from-paths src --ignore-src -r

catkin_make

```

加入环境变量：



```sh
gedit ~/.bashrc
#### carla
export PYTHONPATH=$PYTHONPATH:/home/dev/CARLA_0.9.11/PythonAPI/carla/dist/carla-0.9.11-py2.7-linux-x86_64.egg

#### carla\_ros\_bridge
source ~/carla-ros-bridge/catkin_ws/devel/setup.bash

```

ros节点启动：



```sh
#### Option 1: start the ros bridge
roslaunch carla_ros_bridge carla_ros_bridge.launch

#### Option 2: start the ros bridge together with RVIZ
roslaunch carla_ros_bridge carla_ros_bridge_with_rviz.launch

#### Option 3: start the ros bridge together with an example ego vehicle
roslaunch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch

```

此外，也可以做autoware与carla的联合仿真：


1. carla-仿真引擎（服务端）
2. ros\_bridge-车辆模型和topic节点（客户端）
3. autoware-自动驾驶算法集合


仿真流程：



```sh
1.启动carla-./CarlaUE4.sh 
2.启动carla_ros_bridge-roslaunch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch
3.启动autoware-roslaunch runtime_manager runtime_manager.launch
4.调用autoware中的rviz，显示相机和雷达，及算法处理后的结果

```

可以实现的功能：



```
1.录制bag
2.验证lidar聚类算法
3.雷达建图
……

```

另外，autoware也可以与lgsvl联合仿真，autoware自带了接口。


> win启动lgsvl-2019.4，选择autoware车和ip，autoware启动lgsvl的bridge.launch  
>  能录制velodyne的点云bag，出现车悬空的问题，换了个场景好了


ros也能和lgsvl联合仿真，也要启动一个rosbridge：



```sh
roslaunch rosbridge_server rosbridge_websocket.launch

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





