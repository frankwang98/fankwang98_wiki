








#### 文章目录


* + [1. OpenPlanner介绍](#1_OpenPlanner_1)
	+ [2. 相关代码](#2__10)




### 1. OpenPlanner介绍


OpenPlanner是Autoware种使用的一种运动规划算法，通过对全局路径采样后生成一系列的候选路径，结合矢量地图、传感器结构化数据以及碰撞、交通规则等约束和优化目标，选择出最优的运动轨迹。


OpenPlanner通用性强，只需相应的调整参数即可与任何的移动机器人/自动驾驶车辆车辆配合使用。


![在这里插入图片描述](https://img-blog.csdnimg.cn/df8cf5a4833949698258ac532332b43f.png)


op有全局规划、局部规划以及仿真和工具模块。


### 2. 相关代码


op\_global\_planner全局规划也用到了DP动态规划算法，生成全局路径如下：



```
bool GlobalPlanner::GenerateGlobalPlan(PlannerHNS::WayPoint& startPoint, PlannerHNS::WayPoint& goalPoint, std::vector<std::vector<PlannerHNS::WayPoint> >& generatedTotalPaths)
{
	std::vector<int> predefinedLanesIds;
	double ret = 0;

	ret = m_PlannerH.PlanUsingDP(startPoint, goalPoint, MAX_GLOBAL_PLAN_DISTANCE, m_params.bEnableLaneChange, predefinedLanesIds, m_Map, generatedTotalPaths);

	if(ret == 0)
	{
		std::cout << "Can't Generate Global Path for Start (" << startPoint.pos.ToString()
												<< ") and Goal (" << goalPoint.pos.ToString() << ")" << std::endl;
		return false;
	}


	if(generatedTotalPaths.size() > 0 && generatedTotalPaths.at(0).size()>0)
	{
		if(m_params.bEnableSmoothing)
		{
			for(unsigned int i=0; i < generatedTotalPaths.size(); i++)
			{
				PlannerHNS::PlanningHelpers::FixPathDensity(generatedTotalPaths.at(i), m_params.pathDensity);
				PlannerHNS::PlanningHelpers::SmoothPath(generatedTotalPaths.at(i), 0.49, 0.35 , 0.01);
			}
		}

		for(unsigned int i=0; i < generatedTotalPaths.size(); i++)
		{
			PlannerHNS::PlanningHelpers::CalcAngleAndCost(generatedTotalPaths.at(i));
			if(m_GlobalPathID > 10000)
				m_GlobalPathID = 1;

			for(unsigned int j=0; j < generatedTotalPaths.at(i).size(); j++)
				generatedTotalPaths.at(i).at(j).gid = m_GlobalPathID;

			m_GlobalPathID++;

			std::cout << "New DP Path -> " << generatedTotalPaths.at(i).size() << std::endl;
		}
		return true;
	}
	else
	{
		std::cout << "Can't Generate Global Path for Start (" << startPoint.pos.ToString() << ") and Goal (" << goalPoint.pos.ToString() << ")" << std::endl;
	}
	return false;
}

```

op\_local\_planner局部规划包含op\_trajectory\_generator轨迹生成、op\_trajectory\_evaluator轨迹评价、op\_behavior\_selector行为选择和op\_motion\_predictor运动预测。


op\_simulation\_package是关于op的仿真包。


op\_utilities是关于op相关工具，如op\_pose2tf位姿坐标转换、op\_data\_logger数据记录和op\_bag\_player包播放工具。


以上。





