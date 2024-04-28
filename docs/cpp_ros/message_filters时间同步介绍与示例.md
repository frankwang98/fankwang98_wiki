

### 😏1. message_filters时间同步介绍


`message_filters` 是 ROS（机器人操作系统）中的一个功能包，用于实现多个传感器数据或消息的时间同步。它提供了一个简单而灵活的接口，可以方便地对不同话题发布的消息进行时间戳的同步，以确保数据在处理时具有一致的时间对齐。


在 ROS 中，不同传感器或节点通常以异步方式发布其数据，这意味着它们可能以不同的时间频率或时间延迟发布消息。当需要将这些数据进行融合或联合处理时，时间同步就变得至关重要。`message_filters` 提供了两种常见的时间同步策略：


1. `ApproximateTime`：该策略通过近似匹配不同话题发布的消息的时间戳来实现同步。它会维护一个滑动窗口，在给定的时间窗口内寻找最接近的时间戳，并将相应的消息进行同步。该方法适用于相对短暂的延迟或较小的时间偏差，并且具有较好的实时性能。
2. `ExactTime`：该策略要求不同话题发布的消息具有完全相同的时间戳才能进行同步。只有当所有待同步的消息在相同的时间戳下同时到达时，才会触发同步操作。这种策略对于精确的时间对齐要求比较严格，适用于相对较少的数据或对时间同步要求较高的场景。


使用 message_filters 进行时间同步的一般步骤如下：



> 
> 1.创建一个 message_filters::Subscriber 对象来监听需要同步的话题，并指定消息类型。
> 
> 
> 



> 
> 2.创建一个 message_filters::Cache 对象来管理接收到的消息，并指定缓存的大小和策略（例如 ApproximateTime 或 ExactTime）。
> 
> 
> 



> 
> 3.创建一个回调函数，用于处理同步后的消息。该回调函数会在同步消息到达时被触发，并接收同步后的消息作为参数。
> 
> 
> 



> 
> 4.使用 message_filters::Synchronizer 类将订阅者、缓存和回调函数组合在一起，并设置同步的时间窗口大小等参数。
> 
> 
> 



> 
> 5.调用 Synchronizer 对象的 registerCallback() 函数来注册回调函数。
> 
> 
> 



> 
> 6.在主循环中调用 ROS 的 spin() 函数或使用回调队列来处理消息。
> 
> 
> 


此外，在需要精确时间同步的场景，`TimeSynchronizer`更为适用，它也属于 `message_filters` 功能包的一部分，但应确保所有订阅的话题应具有相同的时间戳来源。如果不同话题的时间戳不一致，可能需要对其进行预处理或使用其他方法来实现时间同步。


### 😊2. 应用示例


项目Github地址：`https://github.com/JackJu-HIT/learning_messages_filters`


该项目演示了使用message_filters时间同步的示例，学习一下：


gps和imu的消息一般需要同步才能获得精确的定位消息，订阅两个消息并同步的示例：



```
#include <message_filters/subscriber.h>
#include <message_filters/synchronizer.h>
#include <message_filters/sync_policies/exact_time.h>
#include <sensor_msgs/Image.h>
#include <sensor_msgs/CameraInfo.h>
#include <math.h>
#include "sensor_msgs/LaserScan.h"
#include "sensor_msgs/NavSatFix.h"
#include "sensor_msgs/Imu.h"
using namespace std;

using namespace sensor_msgs;
using namespace message_filters;
void callback(const sensor_msgs::Imu::ConstPtr& imu, const sensor_msgs::NavSatFix::ConstPtr& gps)
{
  // Solve all of perception here...?>
  cout<<"传感器数据同步..."<<endl;
  cout<<"imu:"<<imu->linear_acceleration.x<<endl;
  cout<<"gps:"<<gps->latitude<<endl;
}
 

int main(int argc, char** argv)
{
  ros::init(argc, argv, "vision_node");
 
  ros::NodeHandle nh;
  // message_filters::Subscriber<Image> image_sub(nh, "image", 1);
  message_filters::Subscriber<sensor_msgs::Imu> imu_sub(nh, "/imu", 1);
  //message_filters::Subscriber<CameraInfo> info_sub(nh, "camera_info", 1);
  message_filters::Subscriber<sensor_msgs::NavSatFix> gps_sub(nh, "/fix", 1);
  typedef sync_policies::ExactTime<Imu, NavSatFix> MySyncPolicy;
  // ExactTime takes a queue size as its constructor argument, hence MySyncPolicy(10)
  Synchronizer<MySyncPolicy> sync(MySyncPolicy(10), imu_sub, gps_sub);
  sync.registerCallback(boost::bind(&callback, _1, _2));
  ros::spin();
 
  return 0;
}

```

同步之后发布出去：



```
#include "sensor_msgs/NavSatFix.h"
#include "sensor_msgs/Imu.h"
#include <ros/ros.h>

using namespace std;

int main(int argc, char **argv)
{
    // ROS节点初始化
    ros::init(argc, argv, "IMU_GPS_publisher");

    // 创建节点句柄
    ros::NodeHandle n;
    // 创建一个Publisher，发布名为/odom_info的topic，消息类型为learning_topic::Person，队列长度10
    ros::Publisher IMU_info_pub = n.advertise<sensor_msgs::Imu>("/imu", 10);
    ros::Publisher GPS_info_pub=n.advertise<sensor_msgs::NavSatFix>("/fix", 10);

    // 设置循环的频率
    ros::Rate loop_rate(1);
    ROS_INFO("The data of IMU already published!");
    int count = 0;
    while (ros::ok())
    {
        // 初始化learning_topic::Person类型的消息
    	sensor_msgs::Imu msg;
        sensor_msgs::NavSatFix msg2;
        msg.linear_acceleration.x=10;
        msg2.latitude=132;      
         // 发布消息
        IMU_info_pub.publish(msg);
        GPS_info_pub.publish(msg2);
        // ROS_INFO("The data of IMU: x:%d y:%d z:%d w:%d", 
		// msg.x, msg.y, msg.z,msg.w);
		loop_rate.sleep();
	}
    return 0;
}

```

### 😆3. 其他示例


其他用法参考官方及这篇翻译：`http://t.csdn.cn/GibgB`


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





