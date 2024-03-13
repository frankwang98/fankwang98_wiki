







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ceres和g2o非线性优化库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__32)
	+ [:satisfied:3. 使用说明](#satisfied3__60)




### 😏1. 项目介绍


`ceres`项目Github地址：`https://github.com/ceres-solver/ceres-solver`


`g2o`项目Github地址：`https://github.com/RainerKuemmerle/g2o`


Ceres Solver和g2o都是用于求解非线性最小二乘问题的C++库，主要用于图优化等领域。它们有一些共同点，但也有一些区别。


Ceres Solver：


* Ceres Solver是一个功能强大的C++库，专门用于求解大规模稀疏和稠密非线性最小二乘问题。
* 它支持各种类型的误差函数，如光束法平差、非线性回归、SLAM、视觉定位等。
* Ceres Solver提供了多种优化算法，包括LM（Levenberg-Marquardt）、GN（Gauss-Newton）等，并且可根据问题特点进行自定义优化策略。
* 它具有灵活的接口和标准化的问题表示方式，可以轻松地与其他库进行集成。
* Ceres Solver支持自动求导，可以通过使用用户提供的误差函数的解析梯度或数值微分来计算导数。
* Ceres Solver是开源的，遵循BSD许可证。


g2o：


* g2o是一个通用的C++库，用于求解图优化问题，例如视觉SLAM、3D重建、机器人运动估计等。
* g2o支持稀疏矩阵和滤波器算法，并提供了灵活的接口和模块化设计。
* 它支持多种顶点和边类型，并允许用户自定义顶点、边类型和优化策略。
* g2o提供了多种优化算法，如GN（Gauss-Newton）、LM（Levenberg-Marquardt）等。
* g2o也是开源的，遵循BSD许可证。


Ceres Solver和g2o在SLAM、机器人运动估计等领域得到了广泛应用。


### 😊2. 环境配置


下面进行环境配置：


ceres：



```
# 安装依赖
sudo apt install cmake libgoogle-glog-dev libgflags-dev libatlas-base-dev libsuitesparse-dev -y
# ceres-1.14
wget ceres-solver.org/ceres-solver-1.14.0.tar.gz
tar -zxvf ceres-solver-1.14.0.tar.gz
cd ceres-solver-1.14.0
mkdir build && cd build
cmake .. && make
sudo make install

```

编译：`g++ -o main main.cpp -lceres -lglog && ./main`


g2o：



```
# 安装依赖
sudo apt-get install libeigen3-dev libsuitesparse-dev qt5-qmake libqglviewer-dev-qt5
git clone https://github.com/RainerKuemmerle/g2o.git
cd g2o
mkdir build && cd build
cmake .. && make
sudo make install

```

### 😆3. 使用说明


下面进行使用分析：


ceres：


构建代价函数Cost\_Functor：



```
// 定义一个实例化时才知道的类型T
template <typename T>

// 运算符()的重载，用来得到残差fi
bool operator()(const T\* const x, T\* residual) const {
     residual[0] = T(10.0) - x[0];
     return true;
   }

```

构建最小二乘问题problem：



```
Problem problem;
CostFunction\* cost_function = new AutoDiffCostFunction<CostFunctor, 1, 1>(new CostFunctor);
problem.AddResidualBlock(cost_function, NULL, &x);

```

求解器参数配置Solver：



```
Solver::Options options;
options.linear_solver_type = ceres::DENSE_QR;
options.minimizer_progress_to_stdout = true;
Solver::Summary summary;
Solve(options, &problem, &summary);
cout << summary.BriefReport() << "\n";//输出优化的简要信息

```

用Ceres Solver库解决一个简单的非线性最小二乘问题示例：



```
#include <iostream>
#include <ceres/ceres.h>

// 代价函数类定义
struct CostFunctor {
  template <typename T>
  bool operator()(const T\* const x, T\* residual) const {
    // 定义目标函数：f(x) = 10 - x
    residual[0] = T(10.0) - x[0];
    return true;
  }
};

int main(int argc, char\*\* argv) {
  // 初始化问题
  ceres::Problem problem;

  // 添加一个残差块
  double initial_x = 5.0;  // 初始值
  ceres::CostFunction\* cost_function =
      new ceres::AutoDiffCostFunction<CostFunctor, 1, 1>(new CostFunctor);
  problem.AddResidualBlock(cost_function, nullptr, &initial_x);

  // 配置求解器选项
  ceres::Solver::Options options;
  options.linear_solver_type = ceres::DENSE_QR;
  options.minimizer_progress_to_stdout = true;

  // 求解问题
  ceres::Solver::Summary summary;
  ceres::Solve(options, &problem, &summary);

  // 打印结果
  std::cout << summary.BriefReport() << "\n";
  std::cout << "Final x = " << initial_x << "\n";

  return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





