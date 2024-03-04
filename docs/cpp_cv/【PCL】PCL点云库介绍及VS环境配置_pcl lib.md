








#### 文章目录


* + [PCL介绍](#PCL_1)
	+ [Windows PCL环境配置](#Windows_PCL_8)
	+ - [Ubuntu PCL环境配置](#Ubuntu_PCL_98)




### PCL介绍


PCL是跨平台点云处理库，用来点云可视化、分割、聚类等应用。


PCL官网在这：`https://pointclouds.org/`


Github库在这（这里用1.8.1）：`https://github.com/PointCloudLibrary/pcl/releases/tag/pcl-1.8.1`


### Windows PCL环境配置


这位[大佬](http://t.csdn.cn/i9m97)在两年前已经写得很明白了，这里复现一下。


ALLInOne安装：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a3f27e76fbd84de69e1650e7105e51f5.png)


第三方库安装（都装在3rdparty）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/902e7e965d564863b0a770069cecfbc3.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/83f660d53194455babf7514acfe631a3.png)


将pdb解包并拷贝到bin：


![在这里插入图片描述](https://img-blog.csdnimg.cn/16315d9919794c5daf82d32af68835f0.png)


添加环境变量：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d76e5339ca3d439590b8d9975b292598.png)


添加包含目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/44435e9c1d954d6dad81b8d02156a591.png)


添加库目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f973f5df600b499b97efc80925ef0a3a.png)


添加预处理器定义：


![在这里插入图片描述](https://img-blog.csdnimg.cn/02f9afc907b74092a55b875e6ae8e483.png)


添加附加依赖项（好多个项啊）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6512e326b78c42ef86c533ae497fb114.png)


将SDL检查设置为否，否则会出现C4996：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6fcc28fa0ebc4b618fc0932369aa1add.png)


复制代码到main中：



```
// pcl181

#include<iostream>
#include<pcl/io/io.h>
#include<pcl/io/pcd\_io.h> //pcd 读写类相关的头文件
#include<pcl/io/ply\_io.h>
#include<pcl/point\_types.h> //PCL中支持的点类型头文件
#include<pcl/visualization/cloud\_viewer.h>

using namespace std;

int user_data;

void viewerOneOff(pcl::visualization::PCLVisualizer& viewer) {
	viewer.setBackgroundColor(1.0, 0.5, 1.0);   //设置背景颜色
}

int main() 
{
	pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);

	char strfilepath[256] = "rabbit.pcd";
	// 判断pcd文件是否存在
	if (-1 == pcl::io::loadPCDFile(strfilepath, \*cloud)) {
		cout << "error input!" << endl;
		return -1;
	}

	cout << cloud->points.size() << endl;	//打印点云大小
	pcl::visualization::CloudViewer viewer("Cloud Viewer");     //创建viewer对象

	viewer.showCloud(cloud);	//显示点云
	viewer.runOnVisualizationThreadOnce(viewerOneOff);
	system("pause");
	return 0;
}


```

如果前面都配置好，运行还提示`缺少dll库`的话，应该是lib还没有加载到工程中，重启一下工程即可。


最后读取rabbit的效果如下（很漂亮）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7ab35eb7e5e545949a42504ab4677165.png)


到这里，环境终于配置好了，希望大家配置也一切顺利。（可算能开始敲代码了）


#### Ubuntu PCL环境配置


ubuntu上大家只要装了ros会默认安装pcl点云库，也可确认安装：`sudo apt install libpcl-dev`


然后在终端输入`pcl_viewer`有输出则表示安装没问题。


如果需要安装其他版本可以下载源码编译，暂时我还没这个需求，暂时够用。


以上。





