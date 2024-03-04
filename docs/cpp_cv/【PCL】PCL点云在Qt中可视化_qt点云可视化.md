






二进制版的vtk第三方库不支持Qt，需要重新下载vtk并用cmake编译，注意要版本对应，这里我用pcl1.8.1，对应vtk8.0，在[这里](https://gitlab.kitware.com/vtk/vtk/tree/v8.0.0)下载。




#### 文章目录


* + [编译VTK-8.0](#VTK80_3)
	+ [Qt测试demo](#Qtdemo_64)




### 编译VTK-8.0


可以参考[这篇](https://blog.csdn.net/whb1815/article/details/110353988)。


将下载好的vtk source解压到pcl安装目录下的3rdparty，将原来的VTK备份一下，然后再源文件下创建build文件夹，编译后的文件会放在这里：


![在这里插入图片描述](https://img-blog.csdnimg.cn/5556f2a38d684a169ea77827f4bf09e8.png)


将其他文件放入src中，然后打开cmake，根据自己的配置来，点击Configure：


![在这里插入图片描述](https://img-blog.csdnimg.cn/926869a64d964b31bc6b2efd9d9884e0.png)


勾选这几项：


![在这里插入图片描述](https://img-blog.csdnimg.cn/27461d048cbd4a00807cdb491041a32d.png)  
 会这样报错，属于正常：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f67b346ba84c47ce8abb233e9738ad59.png)


配置这两项后再点击Configure：


![在这里插入图片描述](https://img-blog.csdnimg.cn/85f1b7f7b6104e71b541fb4274e8f79a.png)  
 有一点要特别注意，这个`Qt5_DIR`一定要设置正确，否则一直出错（经验）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2b0c0854c6e14ab7ae26f44f2241671c.png)


然后点击“Generate”生成VS项目文件。


![在这里插入图片描述](https://img-blog.csdnimg.cn/1f21d517a3b84020a47f2082129a848d.png)


（×备选项）  
 在build目录下打开终端，在VS2017编译器下，输入`cmake .. -G "Visual Studio 15 2017" -A x64`，会编译生成：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b3f2814ca03c4946b7ae333e2fb40b0c.png)


进入到build目录下，使用VS打开VTK.sln


首先在Debug x64下，右键ALL\_BUILD生成编译，再右键INSTALL生成；  
 然后在Release x64下，右键ALL\_BUILD生成编译，再右键INSTALL生成；（全编译生成的时间有点长，可以只生成那个QVTK模块）


![在这里插入图片描述](https://img-blog.csdnimg.cn/44a1f8bf13d4459a93c1fec61d29b1c0.png)


将3rdParty\VTK\plugins\designer下的QVTKWidgetPlugin.dll拷贝到QT\5.12\msvc2017\_64\plugins\designer下，这样Qt里面就有了QVtk的控件了。


![在这里插入图片描述](https://img-blog.csdnimg.cn/97140b8e7000447a8777b467e3a7634d.png)


（bug）  
 后来我在编译的时候一直生成不了QVTKWidgetPlugin.dll，老是报错，因为我用的Qt编译器是mingw64，不知道是不是这个的原因。（2022.11.2更新，最好用msvc编译器，不要用mingw，用msvc成功了）


![在这里插入图片描述](https://img-blog.csdnimg.cn/0da55a54b5bc428a897ec9b998da4a82.png)


至此PCL在windows下的环境已经搭配好了，可以选择重启让环境变量生效。


最后打开qt设计师，应该是可以看到QVTK这个插件的。


![在这里插入图片描述](https://img-blog.csdnimg.cn/d71c458168ad4cef95eff6a0a3a3ff2d.png)


### Qt测试demo


新建ui文件，将QVTK拖入窗体中，然后创建pclvisualizer.cpp和.h文件：


pclvisualizer.h



```
#ifndef PCLVISUALIZER\_H //防卫式声明
#define PCLVISUALIZER\_H
 
#include <vtkAutoInit.h> //导入vtk必须导入，否则出错
VTK\_MODULE\_INIT(vtkRenderingOpenGL2);
VTK\_MODULE\_INIT(vtkInteractionStyle);
 
#include <QtWidgets/QMainWindow>
#include <pcl/io/pcd\_io.h> //输入输出
#include <pcl/point\_types.h> //点云类型
#include <pcl/visualization/pcl\_visualizer.h> //可视化
#include "ui\_pclvisualizer.h"
 
class PCLVisualizer : public QMainWindow
{
	Q_OBJECT
public:
	PCLVisualizer(QWidget \*parent = 0);	//实例化
	~PCLVisualizer();
 
private:
	Ui::PCLVisualizerClass ui;
	//点云数据存储
	pcl::PointCloud<pcl::PointXYZ>::Ptr cloud;	//实例化cloud
	boost::shared_ptr<pcl::visualization::PCLVisualizer> viewer;	//viewer
	
	//初始化vtk部件
	void initialVtkWidget();
 
private slots:
	//创建打开槽
	void onOpen();
	void exit();
	//void setcolor();
};
 
#endif // PCLVISUALIZER\_H

```

pclvisualizer.cpp



```
#include <QFileDialog>
#include <iostream>
#include <vtkRenderWindow.h> //vtk渲染
#include "pclvisualizer.h"
#include <QColorDialog>
#pragma execution\_character\_set("utf-8") //编码
 
PCLVisualizer::PCLVisualizer(QWidget \*parent)
	: QMainWindow(parent)
{
	ui.setupUi(this);
	//初始化
	initialVtkWidget();
	//连接信号和槽
	connect(ui.actionOpen, &QAction::triggered, this, &PCLVisualizer::onOpen);
	connect(ui.actionExit, &QAction::triggered, this, &PCLVisualizer::exit);
}
 
PCLVisualizer::~PCLVisualizer()
{
 
}

void PCLVisualizer::initialVtkWidget()
{
	cloud.reset(new pcl::PointCloud<pcl::PointXYZ>);	//reset cloud
	viewer.reset(new pcl::visualization::PCLVisualizer("viewer", false));	//reset viewer
	viewer->addPointCloud(cloud, "cloud");	//添加点云
 
	ui.qvtkWidget->SetRenderWindow(viewer->getRenderWindow());	//设置渲染
	viewer->setupInteractor(ui.qvtkWidget->GetInteractor(), ui.qvtkWidget->GetRenderWindow());	//设置交互
	ui.qvtkWidget->update();	//update
}

//读取点云数据
void PCLVisualizer::onOpen()
{
	//打开PCD文件
	QString fileName = QFileDialog::getOpenFileName(this,
		tr("Open PointCloud"), ".",
		tr("Open PCD files(\*.pcd)"));
 
 	//判断是否存在
	if (!fileName.isEmpty())
	{
		std::string file_name = fileName.toStdString();
		//sensor\_msgs::PointCloud2 cloud2;
		pcl::PCLPointCloud2 cloud2;
		//pcl::PointCloud<Eigen::MatrixXf> cloud2;
		Eigen::Vector4f origin;
		Eigen::Quaternionf orientation;	//四元数
		int pcd_version;
		int data_type;
		unsigned int data_idx;
		int offset = 0;
		pcl::PCDReader rd;	//实例化rd读取对象
		rd.readHeader(file_name, cloud2, origin, orientation, pcd_version, data_type, data_idx);
 
 		//判断数据类型
		if (data_type == 0)
		{
			pcl::io::loadPCDFile(fileName.toStdString(), \*cloud);
		}
		else if (data_type == 2)
		{
			pcl::PCDReader reader;
			reader.read<pcl::PointXYZ>(fileName.toStdString(), \*cloud);
		}
 
		viewer->updatePointCloud(cloud, "cloud");
		viewer->resetCamera();
		ui.qvtkWidget->update();
	}
}

void PCLVisualizer::exit()	//exit
{
	this->close();
}

```

实际上，只是把上面的代码拷贝进来就直接运行不了的，会报各种各样的错误，归根结底是PCL和VTK库编译的问题，比如编译时选择的OpenGL还是OpenGL2，有没有把VTK库的Debug和Release版本都编译一遍，VS+Qt的编译环境是Debug x64还是Release x64，所以，一直要每一步都正确，最终主要是核对这两个属性表的配置是否正确。


![在这里插入图片描述](https://img-blog.csdnimg.cn/05ae2a52d383435f8f527fd20c88eb66.png)


配置好环境后，建议先创建一个空的Qt环境，加入QVTK控件试一下是否能正常生成，如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f10f6f29a7aa4af5a8a6264059214df0.png)


我在复现的时候，还是遇到了问题，点云pcd打不开，主要是这一步的问题：


![在这里插入图片描述](https://img-blog.csdnimg.cn/25d53163e51f437db90596335bd46406.png)


复现代码如下：



```
pcl_test.h

```


```
#pragma once

#include <QtWidgets/QMainWindow>

#include <QFileDialog>
#include <QColorDialog>
#include <iostream>

#include <pcl/io/pcd\_io.h> //输入输出
#include <pcl/point\_types.h> //点云类型
#include <pcl/visualization/pcl\_visualizer.h> //可视化

#include <vtkRenderWindow.h> //vtk渲染
#include <vtkRenderer.h>
#include <vtkInteractorStyleTrackballCamera.h>
#include <vtkGenericOpenGLRenderWindow.h>
#include <vtkRenderWindowInteractor.h>
#include "ui\_pcl\_test.h"

#include <vtkAutoInit.h> //导入vtk必须导入，否则出错
VTK\_MODULE\_INIT(vtkRenderingOpenGL2);
VTK\_MODULE\_INIT(vtkInteractionStyle);
VTK\_MODULE\_INIT(vtkRenderingFreeType);

class pcl\_test : public QMainWindow
{
    Q_OBJECT

public:
    pcl\_test(QWidget \*parent = Q_NULLPTR);
	~pcl\_test();

private:
    Ui::pcl_testClass ui;

	//点云数据存储
	pcl::PointCloud<pcl::PointXYZ>::Ptr cloud;	//实例化cloud
	boost::shared_ptr<pcl::visualization::PCLVisualizer> viewer;	//viewer
	//pcl::visualization::PCLVisualizer::Ptr viewer;

	//初始化vtk部件
	void initialVtkWidget();

private slots:
	//创建打开槽
	void onOpen();
	void onExit();
	//void setcolor();
};


```


```
pcl_test.cpp

```


```
#include "pcl\_test.h"
#pragma execution\_character\_set("utf-8") //编码

/\*
问题：addPointCloud添加点云时出错！
\*/

pcl_test::pcl\_test(QWidget \*parent)
    : QMainWindow(parent)
{
    ui.setupUi(this);

	//初始化
	//initialVtkWidget();
	//连接信号和槽
	connect(ui.actionOpen, &QAction::triggered, this, &pcl_test::onOpen);
	connect(ui.actionExit, &QAction::triggered, this, &pcl_test::onExit);

}

pcl\_test::~pcl\_test()
{

}

void pcl_test::initialVtkWidget()
{
	cloud.reset(new pcl::PointCloud<pcl::PointXYZ>);	//reset cloud
	viewer.reset(new pcl::visualization::PCLVisualizer("viewer", false));	//reset viewer
	ui.qvtkWidget->SetRenderWindow(viewer->getRenderWindow());	//设置渲染

	//viewer->addPointCloud<pcl::PointXYZ>(cloud, "cloud"); //添加点云（出错）
	//viewer->setupInteractor(ui.qvtkWidget->GetInteractor(), ui.qvtkWidget->GetRenderWindow()); //设置交互
	ui.qvtkWidget->update();	//update
}

//读取点云数据
void pcl_test::onOpen()
{
	//打开PCD文件
	QString fileName = QFileDialog::getOpenFileName(this,
		tr("Open PointCloud"), ".",
		tr("Open PCD files(\*.pcd)"));

	//判断是否存在
	if (!fileName.isEmpty())
	{
		std::string file_name = fileName.toStdString();
		//sensor\_msgs::PointCloud2 cloud2;
		pcl::PCLPointCloud2 cloud2;
		//pcl::PointCloud<Eigen::MatrixXf> cloud2;
		Eigen::Vector4f origin;
		Eigen::Quaternionf orientation;	//四元数
		int pcd_version;
		int data_type;
		unsigned int data_idx;
		int offset = 0;
		pcl::PCDReader rd;	//实例化rd读取对象
		rd.readHeader(file_name, cloud2, origin, orientation, pcd_version, data_type, data_idx);

		//判断数据类型
		if (data_type == 0)
		{
			pcl::io::loadPCDFile(fileName.toStdString(), \*cloud);
		}
		else if (data_type == 2)
		{
			pcl::PCDReader reader;
			reader.read<pcl::PointXYZ>(fileName.toStdString(), \*cloud);
		}

		viewer->updatePointCloud(cloud, "cloud");
		viewer->resetCamera();
		ui.qvtkWidget->update();
	}
}

void pcl_test::onExit()	//exit
{
	this->close();
}


```

结果：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7f794e5b760f47c6aa1bc70d32ae41db.png)


这两天使用下来，感觉VS+Qt+VTK太难用了，可能还是太菜吧，这个代码一直复现不出来，哪位大佬能解决的话欢迎留言。


以上。





