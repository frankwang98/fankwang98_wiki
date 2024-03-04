








#### 文章目录


* + [1.OpenCV介绍](#1OpenCV_1)
	+ [2.Windows OpenCV环境配置（VS和MinGW）](#2Windows_OpenCVVSMinGW_14)
	+ - [VS编译](#VS_15)
		- [MinGW编译](#MinGW_45)
		- [测试](#_72)
	+ [3.Ubuntu OpenCV环境配置](#3Ubuntu_OpenCV_99)
	+ [4.卸载OpenCV4](#4OpenCV4_205)




### 1.OpenCV介绍


`OpenCV`是一个跨平台计算机视觉和机器学习软件库，可以运行在Linux、Windows、Android和Mac OS操作系统上。


`OpenCV`是用C++语言编写的，同时留有C ++（工程部署用）、Python（深度学习用）、Java和MATLAB（Matlab好多例子都调用的opencv）接口，为了学习（juan）和部署视觉类的应用，记录一下学习过程。


`OpenCV`的应用太广了，就不再赘述，这里我主要关心在车辆摄像头上的一些应用。


官网：`https://opencv.org/`


这里我主要看OpenCVxuetang贾老师的视频，然后再看一些比较好的书籍。


贾老师的学习代码如下：`https://gitee.com/opencv_ai/opencv_tutorial_data`


### 2.Windows OpenCV环境配置（VS和MinGW）


#### VS编译


首先安装Visual studio，这里我用的2017，可参考[安装](https://blog.csdn.net/wangzugenwy/article/details/81166955)；


下载OpenCV库，我用的4.5.4，放在[这里](https://pan.baidu.com/s/1mOTz-dJ1s-DMOtjINjN9Tw)，提取码`0121`；


首先，新建工程，设置Release/x64：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f7f74e085a6d4f09be561d78aa36041a.png)


将下载好的OpenCV库解压到D盘，命名为opencv-454（防止版本多了乱）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c0c694fd0a144bb5a925beb45e179337.png)


打开属性管理器，配置Release/x64的属性：


![在这里插入图片描述](https://img-blog.csdnimg.cn/accecb59bc974accabf1c6b60cd1c025.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a6790746577f47f5be3325d2e64ab45b.png)


包含目录配置如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ce917c5f8bb349bb831e9a6370cb6494.png)


库目录配置如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/af425dd7c3684e60b49096902e602f50.png)


附加依赖项配置如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/049e6ac0095a4c1fbb31df0453988228.png)


#### MinGW编译


除了VS，也可以用`MinGW`编译器来链接OpenCV库，首先下载源码并安装好`cmake-gui`，然后配置选好我们mingw的地址，并勾选`WITH_QT`和`WITH_OPENGL`，其他自己看需要。


编译生成后，进入terminal，编译和安装：



```
mingw32-make
mingw32-make install # 生成install目录，这是我们需要的include和lib

```

当然除了自己去编译，也可以下载网上别人编译好的`opencv-mingw`包（**推荐**），因为自己的环境很可能有一些奇奇怪怪的问题。


然后可以在CLion的`CMakeLists`里添加：



```
set(OpenCV_DIR "D:/develop/opencv341\_mingw/x64/mingw/lib")

find_package(OpenCV 3 REQUIRED)
include_directories(${OpenCV\_INCLUDE\_DIRS})
link_directories(${OpenCV\_LIBRARIES})

add_executable(test main.cpp )

target_link_libraries(${PROJECT\_NAME}
        ${OpenCV\_LIBS}
        )

```

#### 测试


配置完成后，写入以下代码（读取图像）：



```
// opencv454学习

#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main()
{
	Mat src = imread("D:/images/test.png");
	imshow("input", src);
	waitKey(0);
	destroyAllWindows();
	return 0;
}

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/4c5a0d24e8dc4ae3ba7287a326f64601.png)


### 3.Ubuntu OpenCV环境配置


如果安装了ros，会自带opencv3.2.0版本，可通过命令查看版本：



```
pkg-config --modversion opencv

```

如果需要用到OpenCV4，可先从[官网](https://opencv.org/releases/)或[Github](https://github.com/opencv/opencv)下载源码。


由于编译过程中一些资源无法正常下载，因此先改几个地方：



```
# ippicv\_2020\_lnx\_intel64\_20191018\_general.tgz解决办法
cd opencv-xxx/3rdparty/ippicv
vim ippicv.cmake
找到https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV\_COMMIT}/ippicv/
在链接前加上github的代理地址：https://ghproxy.com/（后续一样）
# face\_landmark\_model.dat解决办法
cd opencv-xxx/opencv_contrib-4.5.1/modules/face
vim CMakeLists.txt
找到"https://raw.githubusercontent.com/opencv/opencv\_3rdparty/${\_\_commit\_hash}/"
添加代理地址
# .i文件解决办法
cd /opencv-xxx/opencv_contrib-xxx/modules/xfeatures2d/cmake
将cmake文件夹下两文件的下载路径都加上代理地址
这样在编译的时候就可以正常下载以上资源了。

```

下载完成并解压后：



```
mkdir build && cd build
cmake -D CMAKE_INSTALL_PREFIX=/usr/local -D CMAKE_BUILD_TYPE=Release -D OPENCV_GENERATE_PKGCONFIG=ON -D   -D OPENCV_ENABLE_NONFREE=True ..
make
sudo make install

```

然后进行环境配置：



```
sudo gedit /etc/bash.bashrc
# 文件末尾添加以下内容 并保存
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH
# 更新
sudo updatedb
source /etc/bash.bashrc

```

添加动态库：



```
# 打开文件
sudo gedit /etc/ld.so.conf.d/opencv.conf 
# 添加lib路径
/usr/local/lib
# 更新链接库
sudo ldconfig

```

查看安装情况：



```
pkg-config --modversion opencv4 #查看版本号
pkg-config --libs opencv4 #查看libs库

```

源码里有sample示例，可以先学习。


如果要编译带cuda的opencv，可以参考：



```
# 先安装cuda（我选择11.5），选择对应的系统环境安装即可
https://developer.nvidia.com/cuda-11-5-0-download-archive
# 例如，WSL ubuntu
wget https://developer.download.nvidia.com/compute/cuda/11.5.0/local_installers/cuda_11.5.0_495.29.05_linux.run
sudo sh cuda_11.5.0_495.29.05_linux.run
sudo gedit ~/.bashrc
export LD\_LIBRARY\_PATH=$LD\_LIBRARY\_PATH:/usr/local/cuda-11.5/lib64
export PATH=$PATH:/usr/local/cuda-11.5/bin
export CUDA\_HOME=$CUDA\_HOME:/usr/local/cuda-11.5
source ~/.bashrc
nvcc -V  # 验证版本
# 卸载的话
To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.5/bin

```

替换上面编译opencv的选项：



```
cmake \
-D CMAKE\_BUILD\_TYPE=RELEASE \
-D CMAKE\_INSTALL\_PREFIX=/usr/local \
-D INSTALL\_PYTHON\_EXAMPLES=OFF \
-D INSTALL\_C\_EXAMPLES=OFF \
-D OPENCV\_ENABLE\_NONFREE=ON \
-D BUILD\_TIFF=OFF \
-D OPENCV\_EXTRA\_MODULES\_PATH=~/opencv_contrib-4.2.0/modules \
-D BUILD\_EXAMPLES=OFF \
-D CUDA\_ARCH\_BIN='8.0' \
-D WITH\_CUDA=ON \
-D WITH\_CUDNN=ON \
-D WITH\_FFMPEG=ON \
-D WITH\_V4L=ON \
-D WITH\_QT=ON \
-D OPENCV\_DNN\_CUDA=ON \
-D WITH\_CUBLAS=ON \
-D OPENCV\_GENERATE\_PKGCONFIG=YES \
-D CUDA\_nppicom\_LIBRARY=stdc++ \
..

```

### 4.卸载OpenCV4


首先删除opencv4.conf：



```
cd /etc/ld.so.conf.d/
sudo rm opencv4.conf

```

然后进入编译文件夹卸载（源代码编译完先不要删）：



```
cd ./OpenCV-xxx/build
sudo make uninstall

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/8658f1621ec34d3ebace9e313ed5d2c0.png#pic_center)


以上。





