






由于LiDAR一次扫描只能得到局部点云信息，为了能获得全局点云信息（如一个房间、一个三维物体），就需要进行**多次连续扫描**，并进行**点云配准**。由于每次扫描得到的点云都有独立的坐标系，因此点云配准时要进行**坐标变换**（旋转、平移），将多帧不同坐标系下的点云整合到一个坐标系下。


### 点云配准方法


点云配准有粗配准和精配准两个阶段，**粗配准**是指在点云相对位姿完全未知的情况下进行配准，找到一个可以让两块点云相对近似的旋转平移变换矩阵，进而将待配准点云数据转换到统一坐标系内，可以为精配准提供良好的初始值；而**精配准**是指在粗配准的基础上，让点云之间的空间位置差异最小化，得到一个更加精准的旋转平移变换矩阵。


常见的精配准方法有**ICP**迭代最近点算法（Iterative Cloest Point）和**NDT**正态分布变换算法（Normal Distributions Transform）。配准的目标是求得坐标变换参数 \*\*R（ 旋转矩阵）\*\*和 **T（平移向量）**,使得多视角下测得的三维数据经坐标变换后的距离最小，以得到完整的数据模型。


点云配准具体实现步骤如下 :


1. 对数据按照同样的关键点选取标准，提取关键点（Keypoints）。
2. 对选择的所有关键点分别计算其特征描述因子（Feature Description）。
3. 结合特征描述因子在两个数据中的坐标的位置，以两者之间特征和位置的相似度为基础，估算它们的对应关系，初步估计对应点对（Correspondence）。
4. 数据有噪声的话，去除对配准有影响的错误的对应点对。（Filter Error）
5. 利用剩余的正确对应关系来估算刚体变换，完成配准。（R+T）


整个配准过程最重要的是关键点的提取以及关键点的特征描述，以确保对应估计的准确性和效率，这样才能保证后续流程中的刚体变换矩阵估计的无误性。


### NDT正态分布变换配准


用正态分布变换算法能确定两个大型点云（都超过 100 000个点）之间的刚体变换。（为Autoware的NDT建图和定位算法做个铺垫。）


下面用NDT处理2个房间点云数据的配准，代码如下：



```
 /\*
 使用正态分布变换进行配准的实验 。其中room\_scan1.pcd room\_scan2.pcd这些点云包含同一房间360不同视角的扫描数据
 \*/
#include <iostream>
#include <pcl/io/pcd\_io.h>
#include <pcl/point\_types.h>
#include <boost/thread/thread.hpp>
#define BOOST\_TYPEOF\_EMULATION //解决：点云pcl库有关于typeof\_impl.hpp的错误
#include <pcl/registration/ndt.h> //NDT(正态分布)配准类头文件
#include <pcl/filters/approximate\_voxel\_grid.h> //滤波类头文件 （使用体素网格过滤器处理的效果比较好）
#include <pcl/visualization/pcl\_visualizer.h>


int main(int argc, char\*\* argv)
{
	// 加载房间的第一次扫描点云数据作为目标
	pcl::PointCloud<pcl::PointXYZ>::Ptr target\_cloud(new pcl::PointCloud<pcl::PointXYZ>);
	if (pcl::io::loadPCDFile<pcl::PointXYZ>("data/room\_scan1.pcd", \*target_cloud) == -1)
	{
		PCL\_ERROR("Couldn't read file room\_scan1.pcd \n");
		return (-1);
	}
	std::cout << "Loaded " << target_cloud->size() << " data points from room\_scan1.pcd" << std::endl;

	// 加载从新视角得到的第二次扫描点云数据作为源点云
	pcl::PointCloud<pcl::PointXYZ>::Ptr input\_cloud(new pcl::PointCloud<pcl::PointXYZ>);
	if (pcl::io::loadPCDFile<pcl::PointXYZ>("data/room\_scan2.pcd", \*input_cloud) == -1)
	{
		PCL\_ERROR("Couldn't read file room\_scan2.pcd \n");
		return (-1);
	}
	std::cout << "Loaded " << input_cloud->size() << " data points from room\_scan2.pcd" << std::endl;

	//以上的代码加载了两个PCD文件得到共享指针，后续配准是完成对源点云到目标点云的参考坐标系的变换矩阵的估计，得到第二组点云变换到第一组点云坐标系下的变换矩阵
	// 将输入的扫描点云数据过滤到原始尺寸的10%以提高匹配的速度，只对源点云进行滤波，减少其数据量，而目标点云不需要滤波处理
	//因为在NDT算法中在目标点云对应的体素网格数据结构的统计计算不使用单个点，而是使用包含在每个体素单元格中的点的统计数据
	pcl::PointCloud<pcl::PointXYZ>::Ptr filtered\_cloud(new pcl::PointCloud<pcl::PointXYZ>);
	pcl::ApproximateVoxelGrid<pcl::PointXYZ> approximate_voxel_filter;
	approximate_voxel_filter.setLeafSize(0.2, 0.2, 0.2);
	approximate_voxel_filter.setInputCloud(input_cloud);
	approximate_voxel_filter.filter(\*filtered_cloud);
	std::cout << "Filtered cloud contains " << filtered_cloud->size()
		<< " data points from room\_scan2.pcd" << std::endl;

	// 初始化正态分布(NDT)对象
	pcl::NormalDistributionsTransform<pcl::PointXYZ, pcl::PointXYZ> ndt;

	// 根据输入数据的尺度设置NDT相关参数
	ndt.setTransformationEpsilon(0.01);   //为终止条件设置最小转换差异
	ndt.setStepSize(0.1);    //为more-thuente线搜索设置最大步长
	ndt.setResolution(1.0);   //设置NDT网格网格结构的分辨率（voxelgridcovariance）
	//以上参数在使用房间尺寸比例下运算比较好，但如果需要处理例如一个杯子的扫描之类更小的物体，需要对参数进行缩小

	//设置匹配迭代的最大次数，这个参数控制程序运行的最大迭代次数，一般来说这个限制值之前优化程序会在epsilon变换阀值下终止
	//添加最大迭代次数限制能够增加程序的鲁棒性阻止了它在错误的方向上运行时间过长
	ndt.setMaximumIterations(35);

	ndt.setInputSource(filtered_cloud);  //源点云
	// Setting point cloud to be aligned to.
	ndt.setInputTarget(target_cloud);  //目标点云

	// 设置使用机器人测距法得到的粗略初始变换矩阵结果
	Eigen::AngleAxisf init\_rotation(0.6931, Eigen::Vector3f::UnitZ());
	Eigen::Translation3f init\_translation(1.79387, 0.720047, 0);
	Eigen::Matrix4f init_guess = (init_translation \* init_rotation).matrix();

	// 计算需要的刚体变换以便将输入的源点云匹配到目标点云
	pcl::PointCloud<pcl::PointXYZ>::Ptr output\_cloud(new pcl::PointCloud<pcl::PointXYZ>);
	ndt.align(\*output_cloud, init_guess);
	//这个地方的output\_cloud不能作为最终的源点云变换，因为上面对点云进行了滤波处理
	std::cout << "Normal Distributions Transform has converged:" << ndt.hasConverged()
		<< " score: " << ndt.getFitnessScore() << std::endl;

	// 使用创建的变换对为过滤的输入点云进行变换
	pcl::transformPointCloud(\*input_cloud, \*output_cloud, ndt.getFinalTransformation());

	// 保存转换后的源点云作为最终的变换输出
	pcl::io::savePCDFileASCII("room\_scan2\_transformed.pcd", \*output_cloud);

	// 初始化点云可视化对象
	boost::shared_ptr<pcl::visualization::PCLVisualizer>
	viewer\_final(new pcl::visualization::PCLVisualizer("3D Viewer"));
	viewer_final->setBackgroundColor(0, 0, 0);  //设置背景颜色为黑色

	// 对目标点云着色可视化 (red).
	pcl::visualization::PointCloudColorHandlerCustom<pcl::PointXYZ>
	target\_color(target_cloud, 255, 0, 0);
	viewer_final->addPointCloud<pcl::PointXYZ>(target_cloud, target_color, "target cloud");
	viewer_final->setPointCloudRenderingProperties(pcl::visualization::PCL_VISUALIZER_POINT_SIZE,1, "target cloud");

	// 对转换后的源点云着色 (green)可视化.
	pcl::visualization::PointCloudColorHandlerCustom<pcl::PointXYZ>
	output\_color(output_cloud, 0, 255, 0);
	viewer_final->addPointCloud<pcl::PointXYZ>(output_cloud, output_color, "output cloud");
	viewer_final->setPointCloudRenderingProperties(pcl::visualization::PCL_VISUALIZER_POINT_SIZE,1, "output cloud");

	// 启动可视化
	viewer_final->addCoordinateSystem(1.0, "global");  //显示XYZ指示轴
	viewer_final->initCameraParameters();   //初始化摄像头参数

	// 等待直到可视化窗口关闭
	while (!viewer_final->wasStopped())
	{
		viewer_final->spinOnce(100);
		boost::this_thread::sleep(boost::posix_time::microseconds(100000));
	}

	return (0);
}

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ab29ce5aeb104f98913207713e3bc667.png)


以上。





