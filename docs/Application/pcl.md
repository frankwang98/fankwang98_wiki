# pcl学习笔记

这里引用[PCL学习指南&资料推荐](./PCL学习指南&资料推荐.md)

## 一、PCL介绍和学习路径
### PCL介绍

	点云数据的处理可以采用获得广泛应用的Point Cloud Library (点云库，PCL库)。
	PCL库是一个最初发布于2013年的开源C++库。它实现了大量点云相关的通用算法和高效的数据管理。
	支持多种操作系统平台，可在Windows、Linux、Android、Mac OS X、部分嵌入式实时系统上运行。
	PCL是BSD授权方式，可以免费进行商业和学术应用。
**如果说OpenCV是2D信息获取与处理的技术结晶，那么PCL在3D信息获取与处理上，就与OpenCV具有同等地位**
	
下面是PCL架构图：

如图PCL架构图所示，对于3D点云处理来说，PCL完全是一个的模块化的现代C++模板库。其基于以下第三方库：Boost、Eigen、FLANN、VTK、CUDA、OpenNI、Qhull，实现点云相关的获取、滤波、分割、配准、检索、特征提取、识别、追踪、曲面重建、可视化等。

![pcl_structure](../picture/pcl.png)

每个模块都有依赖关系，依赖关系如下图（可以看出有四层），最基本的就是最底层的commom模块。	
箭头对应的是依赖关系，比如第二层的kdtree依赖于common；第四层的registration有四个箭头，分别是sample_consensus, kdtree, common, features。

![pcl_modules](../picture/pcl_modules.png)

### PCL学习路径
入门资料：		
视频：[bilibili-PCL点云库官网教程](https://space.bilibili.com/504859351/channel/series)		
github：[点云库PCL学习教程书籍每章总结](https://github.com/MNewBie/PCL-Notes)

代码实践：	
官方各模块demo-[wiki](https://pcl.readthedocs.io/projects/tutorials/en/latest/#)	
模块对应的对象和函数-[docs-modules](https://pointclouds.org/documentation/modules.html)		
具体模块的学习-有针对性（看双愚的系列文章	）		
pcl实践[黑马pcl-3d点云](https://robot.czxy.com/docs/pcl/)		
CSDN博主系列文章[PCL学习(64篇)](https://www.cnblogs.com/li-yao7758258/category/954066.html)

## 二、PCL安装配置
pcl源代码编译安装：[see this](https://robot.czxy.com/docs/pcl/env/pcl/)

ubuntu安装好ros后，还需要安装pcl-tools：`sudo apt install pcl-tools`，才能使用pcl_viewer等工具。

二进制安装：`ros-noetic-pcl-ros`，默认版本1.10

总结：源码编译会出现莫名其妙的错误，比如我就出现可视化的库找不到和一些操作符过期的问题。最终，大家都用ros的pcl就好了，兼容性特别好！

## 三、PCL各模块学习
### PCL中常用的PointT类型
PointXYZ——成员变量：float x,y,z;

PointXYZ是使用最常见的一个点数据类型，因为他之包含三维XYZ坐标信息，这三个浮点数附加一个浮点数来满足存储对齐，可以通过points[i].data[0]或points[i].x访问点X的坐标值
```
union
{
float data[4];
struct
{
float x;
float y;
float z;
};
};
```
PointXYZI——成员变量：float x,y,z,intensity

PointXYZI是一个简单的X Y Z坐标加intensity的point类型，是一个单独的结构体，并且满足存储对齐，由于point的大部分操作会把data[4]元素设置成0或1（用于变换），不能让intensity与XYZ在同一个结构体中，如果这样的话其内容将会被覆盖，例如：两个点的点积会把第四个元素设置为0，否则点积没有意义。
```
union{
float data[4];
struct
{
float x;
float y;
float z;
};
};
union{
struct{
float intensity;
};
float data_c[4];
};
```

通用编译方法：`mkdir build && cd build && cmake .. && make`

### 基础-pcd写入和读取

### kd-tree

### oc-tree 

## 四、PCL实践
