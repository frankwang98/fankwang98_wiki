






VoxelGrid滤波器是用**体素化网格方法**实现下采样的一种常用滤波方法，这里重点学习。


**下采样**是在保持点云形状的同时减少点云中点的数量；**VoxelGrid**是通过创建三维体素栅格，用每个体素中所有点的重心来近似表示体素中的其他点，来实现下采样。（与体素中心不同，VoxelGrid是用的体素重心，比体素中心法速度慢但准确率高）


### VoxelGrid滤波器相关函数：



```cpp
class  	pcl::VoxelGrid< PointT >
 	VoxelGrid assembles a local 3D grid over a given PointCloud, and downsamples + filters the data. More...
 
class  	pcl::VoxelGrid< pcl::PCLPointCloud2 >
 	VoxelGrid assembles a local 3D grid over a given PointCloud, and downsamples + filters the data. More...
 
class  	pcl::VoxelGridOcclusionEstimation< PointT >
 	VoxelGrid to estimate occluded space in the scene. More...

```

### 示例


通过这条代码创建体素网格：



```
sor.setLeafSize(0.01f, 0.01f, 0.01f);

```

完整代码：



```
#include <iostream>
#include <pcl/io/pcd\_io.h>
#include <pcl/point\_types.h>
#include <pcl/filters/voxel\_grid.h>

int main(int argc, char\*\* argv)
{

	pcl::PCLPointCloud2::Ptr cloud(new pcl::PCLPointCloud2());
	pcl::PCLPointCloud2::Ptr cloud\_filtered(new pcl::PCLPointCloud2());

	//点云对象的读取
	pcl::PCDReader reader;

	reader.read("data/table\_scene\_lms400.pcd", \*cloud);    //读取点云到cloud中

	std::cerr << "PointCloud before filtering: " << cloud->width \* cloud->height
		<< " data points (" << pcl::getFieldsList(\*cloud) << ").";

	/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
 创建一个voxel体素大小为1cm的pcl::VoxelGrid滤波器，
 \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
	pcl::VoxelGrid<pcl::PCLPointCloud2> sor;  //创建滤波对象
	sor.setInputCloud(cloud);            //设置需要过滤的点云给滤波对象
	sor.setLeafSize(0.01f, 0.01f, 0.01f);  //设置滤波时创建的体素体积为1cm的立方体
	sor.filter(\*cloud_filtered);           //执行滤波处理，存储输出

	std::cerr << "PointCloud after filtering: " << cloud_filtered->width \* cloud_filtered->height
		<< " data points (" << pcl::getFieldsList(\*cloud_filtered) << ").";

	pcl::PCDWriter writer;
	writer.write("data/table\_scene\_lms400\_downsampled.pcd", \*cloud_filtered,
		Eigen::Vector4f::Zero(), Eigen::Quaternionf::Identity(), false);

	return (0);
}

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ff503d6a0d1a437bbacb4f62fb708b48.png)


以上。





