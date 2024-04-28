### 1. 技术原理


轨迹跟踪模块主要负责控制车辆沿着规划的路径点行驶，即根据车辆当前的速度、位姿及路径点信息，计算出下一时刻车辆的控制参数（速度和转向），使车辆尽可能沿着规划的路径平稳行驶。


常用的跟踪控制算法有：`纯跟踪算法（pure pursuit）、PID、MPC等（由易到难）`。


纯跟踪算法（pure pursuit）的思想就是：把阿克曼转向的车辆抽象成自行车两轮模型，构建前轮转角和后轴曲率的约束关系，然后以车后轴为切点，车辆纵向车身为切线，控制车辆后轴中心经过轨迹上一系列的点。


![在这里插入图片描述](https://img-blog.csdnimg.cn/73389f13bf414790a2a40e3fc336b348.png)


在示意图中，`(Cx，Cy)`表示当前智能车的位置坐标，`(Gx, Gy)`表示跟踪轨迹的预瞄点的位置坐标，`Ld`为预瞄点到车辆后轴中心的距离即预瞄距离，`R`表示跟踪的曲率半径。根据pure pursuit算法计算出控制量前轮转角`δ`以及对应的车辆转向角`W`。


计算如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/4830520975b248339c8ba0d7d92623c8.png)


### 2. 代码实现


pure_pursuit_node.cpp（主函数，实例化对象并运行）



```
// ROS Includes
#include <ros/ros.h>
// User defined includes
#include <pure_pursuit/pure_pursuit_core.h>

int main(int argc, char** argv)
{
  ros::init(argc, argv, "pure_pursuit");
  waypoint_follower::PurePursuitNode ppn;
  ppn.run();

  return 0;
}

```

pure_pursuit_core.h（PurePursuitNode类定义）



```
// 纯跟踪节点运行
void PurePursuitNode::run()
{
  ROS_INFO_STREAM("pure pursuit start");
  ros::Rate loop_rate(LOOP_RATE_);
  while (ros::ok())
  {
    ros::spinOnce();
    if (!is_pose_set_ || !is_waypoint_set_ || !is_velocity_set_)
    {
      ROS_WARN("Necessary topics are not subscribed yet ... ");
      loop_rate.sleep();
      continue;
    }

    pp_.setLookaheadDistance(computeLookaheadDistance());
    pp_.setMinimumLookaheadDistance(minimum_lookahead_distance_);

    double kappa = 0;
    bool can_get_curvature = pp_.canGetCurvature(&kappa);

    publishTwistStamped(can_get_curvature, kappa);
    publishControlCommandStamped(can_get_curvature, kappa);
    health_checker_ptr_->NODE_ACTIVATE();
    health_checker_ptr_->CHECK_RATE("topic_rate_vehicle_cmd_slow", 8, 5, 1,
      "topic vehicle_cmd publish rate slow.");
    // for visualization with Rviz
    pub11_.publish(displayNextWaypoint(pp_.getPoseOfNextWaypoint()));
    pub13_.publish(displaySearchRadius(
      pp_.getCurrentPose().position, pp_.getLookaheadDistance()));
    pub12_.publish(displayNextTarget(pp_.getPoseOfNextTarget()));
    pub15_.publish(displayTrajectoryCircle(
        waypoint_follower::generateTrajectoryCircle(
          pp_.getPoseOfNextTarget(), pp_.getCurrentPose())));
    if (add_virtual_end_waypoints_)
    {
      pub18_.publish(
        displayExpandWaypoints(pp_.getCurrentWaypoints(), expand_size_));
    }
    std_msgs::Float32 angular_gravity_msg;
    angular_gravity_msg.data =
      computeAngularGravity(computeCommandVelocity(), kappa);
    pub16_.publish(angular_gravity_msg);

    publishDeviationCurrentPosition(
      pp_.getCurrentPose().position, pp_.getCurrentWaypoints());

    is_pose_set_ = false;
    is_velocity_set_ = false;
    is_waypoint_set_ = false;

    loop_rate.sleep();
  }
}

// 发布时间戳

// 发布控制指令

// 计算预瞄距离

// 计算速度

// 计算加速度

// 计算转向角

```

pure_pursuit.h（PurePursuit类定义）



```
// 计算曲率

// 线性插值

```

pure_pursuit_viz.h（marker显示定义）



```
// 显示目标点

// 生成轨迹

// 显示轨迹

```

### 3. 算法改进


使用后发现pure pursuit只能用于一些简单的场景，如直线道路上的循迹；对于一些复杂的路径跟踪效果较差，例如U型/S型等曲线路径。根据pure pursuit的原理可以知道，其跟踪效果很大程度上取决于前视距离`ld`的选择，设置固定的前视距离和路径曲率肯定无法适应不同的路径，因此就需要对前视距离的计算方法进行研究改进。


人类开车时会根据不同驾驶速度和不同路段，进行判断合适的视线跟踪点。因此，我们就可以将这个过程抽象出来，加以处理，形成一个选择前视距离的规则。


动态调整预瞄距离的计算规则：与车速的关系；与跟踪轨迹曲率的关系；跟踪轨迹曲率K计算。


在Autoware中，绿色的球体即为计算的跟踪预瞄点；红色的点为规划好的路径点；白色的轨迹为轨迹跟踪算法计算出的车辆将要运行的轨迹。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ac0b9bd724494751bcecb9ae455834d6.png)


之后要做的事：


* 如何做算法改进；
* 如何手撕算法；
* 针对具体问题，如做倒车场景下的轨迹跟踪。


参考：



```
http://t.csdn.cn/O6KCy
http://t.csdn.cn/J1nPl

```

以上。





