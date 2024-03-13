






A\*算法主要由两个部分组成：全局路径规划和局部路径规划。  
 



#### 文章目录


* + [1. A\*全局](#1_A_2)
	+ [2. A\*局部](#2_A_16)
	+ [3. 相关代码](#3__29)




### 1. A\*全局


全局路径规划的主要目标是在地图中找到从起点到目标点的最短路径。Autoware全局路径规划主要由以下两个部分组成：


1. 车辆当前位置和目标位置的获取：通过 ROS 中的 tf 库获取车辆当前的位置和目标位置的坐标。
2. A\*算法的实现：主要需要完成以下几个步骤：



```
- 定义节点：在 A* 算法中，节点包含了当前位置、到达当前位置的代价、到达当前位置的上一个节点等信息。
- 初始化起点：将起点设置为起始节点，并将其加入到开启列表中。
- 搜索路径：在开启列表中，选择 f 值最小的节点作为当前节点。然后，将当前节点从开启列表中移除，并将其加入到关闭列表中。接着，检查当前节点是否为目标节点，如果是，则搜索完成，否则，将当前节点周围的节点加入到开启列表中，并计算它们的 f 值。最后，将当前节点设置为上一个节点，重新选择 f 值最小的节点进行搜索。
- 生成路径：在搜索完成后，可以通过从目标节点向上查找每个节点的上一个节点，从而生成一条最短路径。

```

### 2. A\*局部


局部路径规划的主要目标是在当前位置和目标位置之间找到一条可行的、避开障碍物的路径。Autoware 中的局部路径规划主要由以下两个部分组成：


1. 障碍物检测：使用激光雷达或摄像头等传感器检测当前车辆周围障碍物。
2. A\* 算法的实现：主要需要完成以下几个步骤：



```
- 定义节点：在 A* 算法中，节点包含了当前位置、到达当前位置的代价、到达当前位置的上一个节点等信息。
- 初始化起点：将当前位置设置为起始节点，并将其加入到开启列表中。
- 搜索路径：在开启列表中，选择 f 值最小的节点作为当前节点。然后，将当前节点从开启列表中移除，并将其加入到关闭列表中。接着，检查当前节点是否为目标节点，如果是，则搜索完成，否则，将当前节点周围的节点

```

### 3. 相关代码


setStartNode()设置起始节点并将其加入到openlist：



```
bool AstarSearch::setStartNode(const geometry_msgs::Pose& start_pose)
{
  // Get index of start pose
  int index_x, index_y, index_theta;
  start_pose_local_.pose = start_pose;
  poseToIndex(start_pose_local_.pose, &index_x, &index_y, &index_theta);
  SimpleNode start\_sn(index_x, index_y, index_theta, 0, 0);

  // Check if start is valid
  if (isOutOfRange(index_x, index_y) || detectCollision(start_sn))
  {
    return false;
  }

  // Set start node
  AstarNode& start_node = nodes_[index_y][index_x][index_theta];
  start_node.x = start_pose_local_.pose.position.x;
  start_node.y = start_pose_local_.pose.position.y;
  start_node.theta = 2.0 \* M_PI / theta_size_ \* index_theta;
  start_node.gc = 0;
  start_node.move_distance = 0;
  start_node.back = false;
  start_node.status = STATUS::OPEN;
  start_node.parent = NULL;

  // set euclidean distance heuristic cost
  if (!use_wavefront_heuristic_ && !use_potential_heuristic_)
  {
    start_node.hc = calcDistance(start_pose_local_.pose.position.x, start_pose_local_.pose.position.y,
                                 goal_pose_local_.pose.position.x, goal_pose_local_.pose.position.y) \*
                    distance_heuristic_weight_;
  }
  else if (use_potential_heuristic_)
  {
    start_node.gc += start_node.hc;
    start_node.hc += calcDistance(start_pose_local_.pose.position.x, start_pose_local_.pose.position.y,
                                  goal_pose_local_.pose.position.x, goal_pose_local_.pose.position.y) +
                     distance_heuristic_weight_;
  }

  // Push start node to openlist
  start_sn.cost = start_node.gc + start_node.hc;
  openlist_.push(start_sn);

  return true;
}

```

search()搜索路径：



```
bool AstarSearch::search()
{
  ros::WallTime begin = ros::WallTime::now();

  // Start A\* search
  // If the openlist is empty, search failed
  while (!openlist_.empty())
  {
    // Check time and terminate if the search reaches the time limit
    ros::WallTime now = ros::WallTime::now();
    double msec = (now - begin).toSec() \* 1000.0;
    if (msec > time_limit_)
    {
      // ROS\_WARN("Exceed time limit of %lf [ms]", time\_limit\_);
      return false;
    }

    // Pop minimum cost node from openlist
    SimpleNode top_sn = openlist_.top();
    openlist_.pop();

    // Expand nodes from this node
    AstarNode\* current_an = &nodes_[top_sn.index_y][top_sn.index_x][top_sn.index_theta];
    current_an->status = STATUS::CLOSED;

    // Goal check
    if (isGoal(current_an->x, current_an->y, current_an->theta))
    {
      // ROS\_INFO("Search time: %lf [msec]", (now - begin).toSec() \* 1000.0);
      setPath(top_sn);
      return true;
    }

    // Expand nodes
    for (const auto& state : state_update_table_[top_sn.index_theta])
    {
      // Next state
      double next_x = current_an->x + state.shift_x;
      double next_y = current_an->y + state.shift_y;
      double next_theta = modifyTheta(current_an->theta + state.rotation);
      double move_cost = state.step;
      double move_distance = current_an->move_distance + state.step;

      // Increase reverse cost
      if (state.back != current_an->back)
        move_cost \*= reverse_weight_;

      // Calculate index of the next state
      SimpleNode next_sn;
      geometry_msgs::Point next_pos;
      next_pos.x = next_x;
      next_pos.y = next_y;
      pointToIndex(next_pos, &next_sn.index_x, &next_sn.index_y);
      next_sn.index_theta = top_sn.index_theta + state.index_theta;

      // Avoid invalid index
      next_sn.index_theta = (next_sn.index_theta + theta_size_) % theta_size_;

      // Check if the index is valid
      if (isOutOfRange(next_sn.index_x, next_sn.index_y) || detectCollision(next_sn))
      {
        continue;
      }

      AstarNode\* next_an = &nodes_[next_sn.index_y][next_sn.index_x][next_sn.index_theta];
      double next_gc = current_an->gc + move_cost;
      double next_hc = nodes_[next_sn.index_y][next_sn.index_x][0].hc;  // wavefront or distance transform heuristic

      // increase the cost with euclidean distance
      if (use_potential_heuristic_)
      {
        next_gc += nodes_[next_sn.index_y][next_sn.index_x][0].hc;
        next_hc += calcDistance(next_x, next_y, goal_pose_local_.pose.position.x, goal_pose_local_.pose.position.y) \*
                   distance_heuristic_weight_;
      }

      // increase the cost with euclidean distance
      if (!use_wavefront_heuristic_ && !use_potential_heuristic_)
      {
        next_hc = calcDistance(next_x, next_y, goal_pose_local_.pose.position.x, goal_pose_local_.pose.position.y) \*
                  distance_heuristic_weight_;
      }

      // NONE
      if (next_an->status == STATUS::NONE)
      {
        next_an->status = STATUS::OPEN;
        next_an->x = next_x;
        next_an->y = next_y;
        next_an->theta = next_theta;
        next_an->gc = next_gc;
        next_an->hc = next_hc;
        next_an->move_distance = move_distance;
        next_an->back = state.back;
        next_an->parent = current_an;
        next_sn.cost = next_an->gc + next_an->hc;
        openlist_.push(next_sn);
        continue;
      }

      // OPEN or CLOSED
      if (next_an->status == STATUS::OPEN || next_an->status == STATUS::CLOSED)
      {
        if (next_gc < next_an->gc)
        {
          next_an->status = STATUS::OPEN;
          next_an->x = next_x;
          next_an->y = next_y;
          next_an->theta = next_theta;
          next_an->gc = next_gc;
          next_an->hc = next_hc;  // already calculated ?
          next_an->move_distance = move_distance;
          next_an->back = state.back;
          next_an->parent = current_an;
          next_sn.cost = next_an->gc + next_an->hc;
          openlist_.push(next_sn);
          continue;
        }
      }
    }  // state update
  }

  // Failed to find path
  // ROS\_INFO("Open list is empty...");
  return false;
}

```

isGoal()判断是否是目标节点：



```
bool AstarSearch::isGoal(double x, double y, double theta)
{
  // To reduce computation time, we use square value for distance
  static const double lateral_goal_range =
      lateral_goal_range_ / 2.0;  // [meter], divide by 2 means we check left and right
  static const double longitudinal_goal_range =
      longitudinal_goal_range_ / 2.0;                                         // [meter], check only behind of the goal
  static const double goal_angle = M_PI \* (angle_goal_range_ / 2.0) / 180.0;  // degrees -> radian

  // Calculate the node coordinate seen from the goal point
  tf::Point p(x, y, 0);
  geometry_msgs::Point relative_node_point = calcRelativeCoordinate(goal_pose_local_.pose, p);

  // Check Pose of goal
  if (relative_node_point.x < 0 &&  // shoud be behind of goal
      std::fabs(relative_node_point.x) < longitudinal_goal_range &&
      std::fabs(relative_node_point.y) < lateral_goal_range)
  {
    // Check the orientation of goal
    if (calcDiffOfRadian(goal_yaw_, theta) < goal_angle)
    {
      return true;
    }
  }

  return false;
}

```

以上。





