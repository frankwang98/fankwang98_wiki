







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍决策规划模块解析。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 决策规划模块介绍](#smirk1__7)
	+ [:blush:2. lattice\_planner模块](#blush2_lattice_planner_16)
	+ [:satisfied:3. 应用示例](#satisfied3__135)




### 😏1. 决策规划模块介绍


决策规划模块是自动驾驶系统的关键部分，负责根据感知和定位信息规划出车辆的行驶轨迹并在行驶中进行运动规划和决策。


Autoware的决策规划模块主要包括以下几个重要组件：  
 waypoint\_maker：即路点生成模块，包含waypoint\_saver、waypoint\_loader和waypoint\_marker\_publisher等节点，用于路径点的保存、加载和发布等功能。


waypoint\_planner：即路点规划模块，包含astar\_avoid、velocity\_set节点，用于局部路径规划和速度设置等功能。


lattice\_planner：lattice规划模块，包含lattice\_trajectory\_gen、lattice\_twist\_convert、path\_select和lattice\_velocity\_set节点，用于局部轨迹生成，速度设置，指令转换等功能。


### 😊2. lattice\_planner模块


Lattice Planner 是一种基于栅格地图的规划算法，通过搜索和优化实现路径规划的目的。Lattice Planner 的核心思想是将路径规划问题转化为一系列离散化的决策问题，通过搜索和优化得到最优路径（多条路径撒点）。


与传统的A\*算法不同，Lattice Planner 能够考虑车辆的动力学约束、道路限制和障碍物等因素，生成更加平滑且安全的路径。实际应用中，Lattice主要用于自主停车、避障等功能。算法示意图如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6fb30c1ddcb94159a387b13ef9386916.png)


`waypointTrajectory`根据当前位姿、速度、路径点等信息生成预测轨迹：



```
static union Spline waypointTrajectory(union State veh, union State goal, union Spline curvature, int next_waypoint)
{
    curvature.success=TRUE;  
    bool convergence=FALSE;
    int iteration = 0;
    union State veh_next;
    double dt = step_size;
    veh.v=goal.v;

    // While loop for computing trajectory parameters
    while(convergence == FALSE && iteration<4)
    {
        // Set time horizon
        double horizon = curvature.s/veh.vdes;
        ROS\_INFO\_STREAM("vdes: " << veh.vdes);
        ROS\_INFO\_STREAM("horizon: " << horizon);

        // Run motion model
        veh_next = motionModel(veh, goal, curvature, dt, horizon, 0);
        
        // Determine convergence criteria
        convergence = checkConvergence(veh_next, goal);

        // If the motion model doesn't get us to the goal compute new parameters
        if(convergence==FALSE)
        {
            // Update parameters
            curvature = generateCorrection(veh, veh_next, goal, curvature, dt, horizon);
            iteration++;

            // Escape route for poorly conditioned Jacobian
            if(curvature.success==FALSE)
            {
                ROS\_INFO\_STREAM("Init State: sx "<<veh.sx<<" sy " <<veh.sy<<" theta "<<veh.theta<<" kappa "<<veh.kappa<<" v "<<veh.v);
                ROS\_INFO\_STREAM("Goal State: sx "<<goal.sx<<" sy " <<goal.sy<<" theta "<<goal.theta<<" kappa "<<goal.kappa<<" v"<<goal.v);
                break;
            }
        }    
    }

    if(convergence==FALSE)
    {
      ROS\_INFO\_STREAM("Next State: sx "<<veh_next.sx<<" sy " <<veh_next.sy<<" theta "<<veh_next.theta<<" kappa "<<veh_next.kappa<<" v "<<veh_next.v);
      ROS\_INFO\_STREAM("Init State: sx "<<veh.sx<<" sy " <<veh.sy<<" theta "<<veh.theta<<" kappa "<<veh.kappa);
      ROS\_INFO\_STREAM("Goal State: sx "<<goal.sx<<" sy " <<goal.sy<<" theta "<<goal.theta<<" kappa "<<goal.kappa);
      curvature.success= FALSE;
    }

    else
    {
        ROS\_INFO\_STREAM("Converged in "<<iteration<<" iterations");

        #ifdef LOG\_OUTPUT
        // Set time horizon
         double horizon = curvature.s/v_0;
        // Run motion model and log data for plotting
        veh_next = motionModel(veh, goal, curvature, 0.1, horizon, 1);
        fmm_sx<<"0.0 \n";
        fmm_sy<<"0.0 \n";
        #endif
    }

    return curvature;
}

```

`lattice_twist_convert`订阅车辆状态和规划输出，并发布到twist\_cmd命令：



```
  // Publish the following topics:
  // Commands
  ros::Publisher cmd_velocity_publisher = nh.advertise<geometry\_msgs::TwistStamped>("twist\_raw", 10);

  // Subscribe to the following topics:
  // Curvature parameters and state parameters
  ros::Subscriber spline_parameters = nh.subscribe("spline", 1, splineCallback);
  ros::Subscriber state_parameters = nh.subscribe("state", 1, stateCallback);

```

`lattice_velocity_set`可根据不同的路段和情况设置不同的车辆速度。


`path_select`用来实现换道：



```
#include <ros/ros.h>
#include "autoware\_msgs/Lane.h"
#include <iostream>

static ros::Publisher _pub;

void callback(const autoware_msgs::Lane &msg)
{
    _pub.publish(msg);
}


int main(int argc, char \*\*argv)
{
    ros::init(argc, argv, "path\_select");

    ros::NodeHandle nh;
    ros::Subscriber twist_sub = nh.subscribe("temporal\_waypoints", 1, callback);
    _pub = nh.advertise<autoware\_msgs::Lane>("final\_waypoints", 1000,true);

    ros::spin();

    return 0;
}

```

### 😆3. 应用示例


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





