







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Ipopt、OSQP、osqp-eigen常用求解器配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. Ipopt配置使用](#blush2_Ipopt_21)
	+ [:satisfied:3. OSQP、osqp-eigen配置使用](#satisfied3_OSQPosqpeigen_73)




### 😏1. 项目介绍


Ipopt项目Github地址：`https://github.com/coin-or/Ipopt`


OSQP项目Github地址：`https://github.com/osqp/osqp`


osqp-eigen项目Github地址：`https://github.com/robotology/osqp-eigen`


`Ipopt`（Interior Point OPTimizer）是一个强大的非线性优化求解器。它被广泛用于数学建模和优化问题，特别是连续优化问题。Ipopt基于内点法算法，可以高效地解决大规模非线性约束优化问题。它支持连续变量和离散变量，并能处理不等式约束、等式约束和混合约束。Ipopt是一个开源库，可以在商业和学术项目中免费使用。


ipopt文档：`https://coin-or.github.io/Ipopt/`


`OSQP`（Operator Splitting Quadratic Program）是一个快速的凸二次规划求解器。它基于ADMM（Alternating Direction Method of Multipliers）算法，能够高效地求解大规模凸二次规划问题。OSQP对于需要在实时或嵌入式系统中求解二次规划问题非常有用，因为它具有低内存占用和快速求解的特点。OSQP也是一个开源库，可以免费使用并适用于商业和学术项目。


`osqp-eigen`是一个与OSQP库集成的C++接口库。它将OSQP库与Eigen线性代数库相结合，使用户可以方便地在C++环境中使用OSQP进行凸二次规划求解。osqp-eigen提供了一个简单而直观的API，使用户可以轻松地定义问题并使用OSQP进行求解。通过osqp-eigen，您可以使用Eigen的矩阵和向量类型来定义问题，并且能够直接访问OSQP的高性能二次规划求解功能。


### 😊2. Ipopt配置使用


下面进行环境配置：



```
# apt安装
sudo apt install coinor-libipopt-dev

```

apt安装的，在使用头文件时，需要加上宏定义，否则会出现“不包含cstddef或stddef”的错误。



```
#define HAVE\_CSTDDEF
#include <coin/IpIpoptApplication.hpp>
#include <coin/IpSolveStatistics.hpp>
#undef HAVE\_CSTDDEF

```


```
# 源码安装
# 安装CppAd、blas、curses、coinor等依赖项
sudo apt-get install -y cppad gfortran libncurses5-dev libncursesw5-dev coinor-libipopt-dev libmetis-dev
# 下载编译
wget https://www.coin-or.org/download/source/Ipopt/Ipopt-3.12.7.zip 
unzip Ipopt-3.12.7.zip 
cd Ipopt-3.12.7
./configure
# 一定要看到配置成功
make
sudo make install
# 好像这个库安装会在本地，cmake文件引用一下绝对路径比较靠谱，也可以cp到系统路径

```

cmake引用示例：



```
include_directories(/usr/local/include/coin-or ./include /data/test_cpp/EMplanner/Ipopt/Ipopt-3.12.7/include/coin)
link_directories(/usr/local/lib ./lib /data/test_cpp/EMplanner/Ipopt/Ipopt-3.12.7/lib)

target_link_libraries(main ipopt lapack blas)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/975a3ba9d8fd46799d74b0de659149b6.png)


之前参考过的文章：



```
https://blog.csdn.net/qcxyliyong/article/details/103348632
https://zhuanlan.zhihu.com/p/520848641
https://blog.csdn.net/chentao1206/article/details/51280610
https://blog.csdn.net/weixin_34945803/article/details/118582379
https://www.chuxin911.com/IPOPT_intro_20210906/

```

### 😆3. OSQP、osqp-eigen配置使用


下面进行环境配置：


OSQP：



```
# 安装依赖
sudo apt install build-essential cmake git libeigen3-dev
git clone https://github.com/oxfordcontrol/osqp
cd osqp
mkdir build
cd build
cmake ..
make
sudo make install

```

osqp-eigen：



```
git clone https://github.com/robotology/osqp-eigen.git
cd osqp-eigen
mkdir build
cd build
cmake ..
make
sudo make install

```

cmakelists.txt中引用OSQP的示例：



```
find_package(Eigen3 REQUIRED)	# eigen3
find_package(osqp REQUIRED)
find_package(OsqpEigen REQUIRED)

target_link_libraries(main Eigen3::Eigen osqp::osqp OsqpEigen::OsqpEigen
 )

```

另外，`QPOASES` 也是一个高效的二次规划求解器，基于C++编写，适用于中小规模的问题，并且具有快速的求解速度和较低的内存消耗。



```
sudo apt-get install cmake g++ gfortran
git clone https://github.com/coin-or/qpOASES.git
cd qpOASES
mkdir build
cd build
cmake ..
make
sudo make install

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





