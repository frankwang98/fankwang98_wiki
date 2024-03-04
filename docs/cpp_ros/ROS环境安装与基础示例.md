






**心口如一，犹不失为光明磊落丈夫之行也。——梁启超**  
 



#### 文章目录


* + [:smirk:1. ros环境安装](#smirk1_ros_2)
	+ [:blush:2. ros命令与工具](#blush2_ros_55)
	+ [:satisfied:3. helloworld节点创建](#satisfied3_helloworld_96)
	+ [:smirk:4. 日志消息](#smirk4__214)
	+ [:smirk:5. ros程序打包](#smirk5_ros_243)




### 😏1. ros环境安装


官网：`https://www.ros.org/`  
 中文wiki：`http://wiki.ros.org/cn`  
 ubuntu安装ros：`http://wiki.ros.org/cn/melodic/Installation/Ubuntu`


ros安装最常见的是在ubuntu系统中，有`amd64`和`arm64`两种，安装流程如下：



> 
> 1. 配置Ubuntu软件仓库
> 2. 设置sources.list
> 3. 设置密钥
> 4. 安装ros-distro
> 5. 初始化 rosdep（包含rosdep init和rosdep update）
> 6. 设置环境
> 7. 构建工厂依赖
> 
> 
> 


之前常见的是在第5步出现问题卡住，原因是ros的github仓库国内网络访问缓慢，有这几种解决方法：



> 
> 1. 用手机热点更新：rosdep依赖下载的就是从`https://github.com/ros/rosdistro`这个repo里的yaml文件，大家知道，手机一般是能访问github的，因此`rosdep update`这步用手机热点一般是能更新成功。
> 2. 将rosdistro下载到本地：手动改URL路径，把`https://raw.githubusercontent.com/ros`替换为`file:///home/xxx/Software`
> 3. 将rosdistro转存到gitee仓库：将`https://raw.githubusercontent.com/ros`替换为`https://gitee.com/ros`
> 
> 
> 


后来，一位机器人领域的大佬自制了一个安装工具，让我们终于不用花时间在安装上了，简直是太良心，不用浪费生命了。这就是**小鱼一键安装ROS工具**：



```
wget http://fishros.com/install -O fishros && . fishros

```

包含了换源、选择ROS版本等操作选项，极为方便，从此安装ROS不再烦恼！大佬网址在这：`http://fishros.com/#/fish_home`


当然如果确实想用rosdep工具，也可用以下方法试试：



```
# 首先下载20-default.list（和运行sudo rosdep init是一样的效果）
sudo mkdir -p /etc/ros/rosdep/sources.list.d/
sudo curl -o /etc/ros/rosdep/sources.list.d/20-default.list https://mirrors.tuna.tsinghua.edu.cn/github-raw/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
# 然后更新配置
echo 'export ROSDISTRO\_INDEX\_URL=https://mirrors.tuna.tsinghua.edu.cn/rosdistro/index-v4.yaml' >> ~/.bashrc
source ~/.bashrc
# 最后重新执行
rosdep update

```

ros安装完成后，可通过在命令行输入`roscore`查看主节点启动信息；


![在这里插入图片描述](https://img-blog.csdnimg.cn/3b04dc50f5e6418d92bf8e79e979da39.png)


然后启动小乌龟节点：`rosrun turtlesim turtlesim_node`


![在这里插入图片描述](https://img-blog.csdnimg.cn/d85cfd86ff7b4fbf8cb5a3ffb2ff089a.png)  
 最后启动键盘控制节点：`rosrun turtlesim turtle_teleop_key`


![在这里插入图片描述](https://img-blog.csdnimg.cn/7ded8846248c466d97b18b3129804756.png)


以上是ros的简单示例，通过这个有趣的例子让大家能对ros有基本的认知。


### 😊2. ros命令与工具


随着ros的流行，相关的学习媒介也越来越多，除了官网和书籍外，还有古月居、创客智造、赵虚左等都有ros的相关教程，这里就不放链接了，网上很容易找到。


ros的基本组成单元是节点。节点之间的通信有消息通信（`发布/订阅，msg定义数据结构`）、服务通信（`请求/响应，srv定义数据结构`）、动作通信三种，但最常用的是消息通信，通信的形式一般是话题即`/topic`。消息传递的理念是：当节点想要分享消息时，可以发布(`publish`)消息到对应话题；当节点想要接收消息时，可以订阅(`subscribe`)所需要的话题。


节点之间的话题可以用bag的形式存储下来，并可以重播。当节点内有些参数需要配置时，可以使用参数服务器`rosparam`来配置。


常用命令：



```
roscore	# 主节点，会启动节点管理器
rosrun package_name node_name	# 启动节点
# 显示设置节点名称 \_\_name:=node-name
# 指定命名空间 \_\_ns:=namespace
roslaunch package_name xxx.launch	# 启动多节点
rosnode list	# 查看node列表 rqt\_graph 查看节点计算图
rosnode kill	# 终止节点 rosnode cleanup 终止后清除
rostopic list	# 查看topic列表
rosservice list	# 查看service列表
rosservice call # 调用服务
rosmsg list		# 查看msg列表
rosparam list	# 查看param列表
rosbag record/play	# 数据包记录/重播
rospack find	# 找到软件包目录
rosls	# 查看目录下的文件
roscd 	# 切换到软件包目录
rostopic hz	# 发布频率（每秒发布的消息数量）
rostopic bw # 发布带宽（每秒消息所占字节数）
roslaunch # 多节点启动（有多种参数可调）

```

常用工具：



```
rviz
rqt
rosrun rqt_publisher rqt_publisher	# rqt界面手动发送数据
gazebo

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/11816188487148fb93da377c2733558a.png)


### 😆3. helloworld节点创建


创建ros工程的一般流程如下：


1. 创建工作空间：`mkdir catkin_ws && cd catkin_ws`
2. 创建源文件目录：`mkdir src && cd src`
3. 使用`catkin_create_pkg`命令来创建一个名为`beginner_tutorials`的新程序包。 这个程序包依赖于`std_msgs、roscpp和rospy`： `catkin_create_pkg beginner_tutorials std_msgs rospy roscpp`
4. 返回catkin工程目录并编译：`cd ~/catkin_ws && catkin_make`


![在这里插入图片描述](https://img-blog.csdnimg.cn/ccf1ffa48bd148bbab225b93ae610093.png)


C++创建ros节点helloworld：


定义msg消息：



```
Header header
int64 num
string child_frame_id
geometry_msgs/PoseWithCovariance pose
geometry_msgs/TwistWithCovariance twist

```

找到`CMakeLists.txt`和`package.xml`添加消息的构建与运行（这个要练习并熟悉）。


基于msg消息，编写发布器节点和订阅器节点，实现话题通信。


发布器talker.cpp：



```
/\* 
 \* 代码思路如下：
 \* 1.初始化ROS系统
 \* 2.在ROS网络内广播我们将要在chatter topic上发布std\_msgs/String消息
 \* 3.以每秒10次的频率在chatter上发布消息
\*/ 
##include "ros/ros.h"
##include "std\_msgs/String.h"

##include <sstream>

/\*\*
 \* This tutorial demonstrates simple sending of messages over the ROS system.
 \*/
int main(int argc, char \*\*argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  ros::Rate loop\_rate(10);	// ros::Rate 可以考虑循环中其他部分消耗的时间
  
  int count = 0;
  while (ros::ok())
  {
    std_msgs::String msg;
    std::stringstream ss;
    ss << "hello ! ROS world !" << count;
    msg.data = ss.str();
    ROS\_INFO("%s", msg.data.c\_str());
    chatter_pub.publish(msg);

    ros::spinOnce();
    loop_rate.sleep();
    ++count;
  }

  return 0;
}

```

订阅器listener.cpp：



```
/\*
 \* 1.初始化ROS系统
 \* 2.订阅chatter topic
 \* 3.进入自循环，等待消息的到达
 \* 4.当消息到达，调用chatterCallback()函数
\*/
##include "ros/ros.h"
##include "std\_msgs/String.h"

/\*\*
 \* This tutorial demonstrates simple receipt of messages over the ROS system.
 \*/
// 回调函数
void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
  ROS\_INFO("I heard: [%s]", msg->data.c\_str());
}

int main(int argc, char \*\*argv)
{
  ros::init(argc, argv, "listener");

  ros::NodeHandle n;
  ros::Subscriber sub = n.subscribe("chatter", 1000, chatterCallback);	// &可选，获取函数指针

  ros::spin();      // 自循环，等待并执行回调函数，相当于在while循环里调用ros::spinOnce()

  return 0;
}

```

然后运行`catkin_make`编译，会生成talker和listener两个可执行文件，存储到devel/lib目录。


服务通信类似，用到的时候再学习就行；另外，如果是python玩家，也可以用python实现。


参数服务器（给节点设置参数）参考：



```
http://t.csdn.cn/pYEpb
http://t.csdn.cn/zyL12

```

另外，默认ros的编译是以debug模式编译的，如果想打包`release`，运行命令：



```
catkin build --cmake-args -DCMAKE_BUILD_TYPE=Release

```

或者在cmake中设置：`SET(CMAKE_BUILD_TYPE "Release")`


### 😏4. 日志消息


程序中用于排查问题的简短文本字符流，即日志消息，根据严重级别，可分为`DEBUG INFO WARN ERROR FATAL`。输出指令分别为：



```
ROS\_DEBUG\_STREAM("")
ROS\_INFO\_STREAM("")
ROS\_WARN\_STREAM("")
ROS\_ERROR\_STREAM("")
ROS\_FATAL\_STREAM("")
// 若要生成一次性日志消息，加上ONCE
ROS\_INFO\_STREAM\_ONCE("")

```

rosout节点会默认生成日志文件保存到`~/.ros/log`中，一段时间后，日志会大于1G，我们可以通过以下命令查看日志大小和清除：



```
rosclean check
rosclean purge

```

ros默认会生成INFO级别以上的日志，可通过`rqt_logger_level`界面来设置日志级别，也可在C++代码中设置：



```
#include <log4cxx/logger.h>
. . .
log4cxx::Logger::getLogger(ROSCONSOLE_DEFAULT_NAME)->setLevel(
ros::console::g_level_lookup[ros::console::levels::Debug]
);
ros::console::notifyLoggerLevelsChanged();

```

### 😏5. ros程序打包


在实际工程中，为避免在机器人上直接放源码编译，最好还是打包成deb，再部署到环境中。


有两篇教程，使用bloon和fakeroot来打包程序：



```
https://juejin.cn/post/7136394709219409933
https://zhuanlan.zhihu.com/p/578951132

```

但在实际执行`bloom-generate rosdebian --os-name ubuntu --ros-distro melodic`的时候，遇到个问题，各位博友看有没有遇到过的？



```
==> Generating debs for ubuntu:bionic for package(s) ['test\_msgs']
No homepage set, defaulting to ''
Could not resolve rosdep key 'roscpp'
Try to resolve the problem with rosdep and then continue.
Continue [Y/n]? n
GeneratorError: Error running generator: Failed to resolve rosdep key 'roscpp', aborting.

```

另外，也有一种简单的打包方式，即通过`catkin_make install`的方式：



```
# 编译install（会生成install目录，包含include、lib和share等文件）
catkin_make install
# 然后要将devel中lib目录下的节点名目录拷贝到install中lib目录下
# 最后，如果有config配置文件的，放在install目录中即可
# 依次执行即可

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b190e200203c4a36b30aaad9f0493ceb.png)


以上！





