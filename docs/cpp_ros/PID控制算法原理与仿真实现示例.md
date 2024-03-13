







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍PID及常用控制算法原理与实现。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. PID介绍](#smirk1_PID_7)
	+ [:blush:2. C++实现示例](#blush2_C_29)
	+ [:satisfied:3. ROS实现示例](#satisfied3_ROS_126)




### 😏1. PID介绍


PID（比例-积分-微分）算法是一种经典的控制算法，常用于控制系统中的反馈控制。它根据当前误差的大小和变化率，计算输出信号来调节控制器的行为，以使系统稳定并达到期望的目标。


PID 算法由三个部分组成：


1. **比例项**（Proportional）：该项与当前误差成正比，通过乘以一个比例系数来得到输出信号。比例项的作用是对系统的偏差进行直接补偿，但可能会导致过度调节和震荡。
2. **积分项**（Integral）：该项与误差的累积量成正比，通过乘以一个积分系数来得到输出信号。积分项的作用是消除系统的稳态误差，因为它积累了系统历史上产生的误差，并且可以持续地进行补偿。
3. **微分项**（Derivative）：该项与误差的变化率成正比，通过乘以一个微分系数来得到输出信号。微分项的作用是预测误差的未来变化趋势，以便提前调整控制器的输出，从而使系统响应更加平滑和稳定。


PID 算法的输出信号计算公式为：



```
Output = Kp * Error + Ki * ∫(Error) dt + Kd * d(Error)/dt

```

其中，Kp、Ki 和 Kd 分别是比例、积分和微分系数，Error 是当前误差，∫(Error) dt 是误差的积分，d(Error)/dt 是误差的导数。


PID 算法的主要思想是根据**当前误差、误差的变化以及误差的积分情况**来综合调节控制器的输出信号。通过合理选择和调整比例、积分和微分系数，可以使系统的响应快速而稳定地收敛到期望状态，并具有较小的超调和震荡。


实际工程中需要根据系统特性对参数进行调试和优化，以获得最佳的控制效果（稳定性、快速性和准确性）。


### 😊2. C++实现示例


PID控制原理C++实现示例：



```
#include <iostream>
//#include "matplotlibcpp.h" // 如果配了matplotlibcpp，可以画图表示

//namespace plt = matplotlibcpp;

class PIDController {
public:
    PIDController(double kp, double ki, double kd)
            : kp\_(kp), ki\_(ki), kd\_(kd), integral\_(0.0), previous\_error\_(0.0) {}

    /\*\*
 \* @brief 计算PID控制器的输出
 \* @param setpoint 设定值
 \* @param feedback 反馈值
 \* @param dt 时间间隔
 \* @return
 \*/
    double compute(double setpoint, double feedback, double dt) {
        double error = setpoint - feedback; // 误差，比例项使得控制系统能够迅速响应并逼近设定值
        integral_ += error \* dt; // 累积误差，积分项用于补偿系统的稳态误差，即长时间内无法通过比例项和微分项完全纠正的误差
        double derivative = (error - previous_error_) / dt; // 误差的导数，微分项帮助控制系统更快地响应变化，并减小超调和震荡
        double output = kp_ \* error + ki_ \* integral_ + kd_ \* derivative; // 控制量计算
        previous_error_ = error;
        return output;
    }

private:
    double kp_;
    double ki_;
    double kd_;
    double integral_;
    double previous_error_;
};

int main() {
    double setpoint = 10.0;
    double feedback = 0.0;
    double dt = 0.1;

    // 创建一个PID控制器对象
    PIDController pid(0.5, 0.2, 0.1);

    // 存储时间和控制量的向量
    //std::vector<double> time;
    //std::vector<double> control;

    // 模拟控制循环
    for (int i = 0; i < 100; ++i) {
        // 计算控制量
        double output = pid.compute(setpoint, feedback, dt);

        // 模拟反馈过程
        feedback += output \* dt;

        // 记录时间和控制量
        //time.push\_back(i \* dt);
        //control.push\_back(output);

        // 输出控制量和反馈值
        std::cout << "Control: " << output << " Feedback: " << feedback << std::endl;
    }

    // 绘制曲线
    //plt::plot(time, control);
    //plt::xlabel("Time");
    //plt::ylabel("Control");
    //plt::title("PID Control Output");
    //plt::show();

    return 0;
}

```


```
# CMakeLists.txt
cmake_minimum_required(VERSION 3.19)
project(clion)

set(CMAKE_CXX_STANDARD 11)

include_directories("D:/Miniconda3")
include_directories("D:/Miniconda3/include")
include_directories("D:/Miniconda3/Lib/site-packages")
include_directories("D:/Miniconda3/Lib/site-packages/numpy/core/include")
link_directories(./)

add_executable(clion main.cpp)

target_link_libraries(clion
        -LD:/Miniconda3 -lpython38
        )

```

### 😆3. ROS实现示例


为了便于验证，我找了一下github上基于`Turtlebot3`机器人的`PID`算法示例，链接：`https://github.com/kadupitiya/ROS-TurtleBot-PID`


核心计算跟上述C++示例差不多，代码如下：



```
double PIDImpl::calculate( double setpoint, double pv )
{
    // error
    double error = setpoint - pv;

    // Proportional portion
    double Pout = _Kp \* error;

    // Integral portion
    _integral += error \* _dt;
    double Iout = _Ki \* _integral;

    // Derivative portion
    double derivative = (error - _pre_error) / _dt;
    double Dout = _Kd \* derivative;

    // Total output
    double output = Pout + Iout + Dout;

    // Limit to max/min
    if( output > _max )
        output = _max;
    else if( output < _min )
        output = _min;

    // Save error to previous error
    _pre_error = error;

    return output;
}

```

代码实现效果如下：（原代码里PID参数设置会有画龙，我修改了一下D值，看着还行）



```
double dt = 0.1, maxT = M_PI, minT = -M_PI, Kp = 0.3, Ki = 0.05, Kd = 0.1; // modify Kd
double dtS = 0.1, maxS = maxSpeed, minS = 0.0, KpS = 0.08, KiS = 0.01, KdS = 0.005;

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/1b9bf57007214f27a0ba46958d0c8410.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/38d78665a1c347ed9a4783bb4cb97e7f.png#pic_center)


以上。





