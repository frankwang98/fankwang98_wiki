







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍matplotlib-cpp图表库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__23)
	+ [:satisfied:3. 使用说明](#satisfied3__47)
	+ - [最简单的示例：](#_50)
		- [一个三维图形示例：](#_98)
		- [读取txt文件中的xy两列数据并显示：](#txtxy_124)




### 😏1. 项目介绍


项目Github地址：`https://github.com/lava/matplotlib-cpp`


matplotlib-cpp 是一个用于 C++ 的简易接口，它允许你在 C++ 程序中使用 `Python 的 matplotlib 库`来绘制图表。这个库提供了一个`类似于 matplotlib 的 API`，使得在 C++ 中生成各种类型的图表变得更加简单和方便。


以下是 matplotlib-cpp 的一些主要特点和功能：



> 
> 1.轻量级：matplotlib-cpp 是一个轻量级的库，只包含少量的头文件，并且没有其他的依赖项。这使得它很容易集成到你的项目中。
> 
> 
> 



> 
> 2.简单易用：matplotlib-cpp 提供了与 matplotlib 类似的函数和方法，使得在 C++ 中绘制图表变得直观和易于理解。你可以使用类似于 Python 的语法来创建图表、设置图表属性和保存图表。
> 
> 
> 



> 
> 3.支持多种图表类型：matplotlib-cpp 支持绘制多种类型的图表，包括线图、散点图、柱状图、饼图、等高线图等。你可以选择适合你数据展示需求的图表类型。
> 
> 
> 



> 
> 4.支持自定义设置：你可以自定义图表的各种属性，如标题、标签、坐标轴范围、图例、颜色等。这样你可以根据具体需求来设计和美化图表。
> 
> 
> 



> 
> 5.与 Python 的无缝集成：使用 matplotlib-cpp，你可以在 C++ 代码中调用 Python 的 matplotlib 库来生成图表。这使得你可以利用 Python 在图表方面丰富的生态系统和强大的功能来扩展你的 C++ 应用程序。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# 安装python包
sudo apt install python3 python3-pip
pip3 install matplotlib numpy
# 源码编译
git clone https://github.com/lava/matplotlib-cpp.git
cd matplotlib-cpp
mkdir build && cd build
cmake ..
make
sudo make install

```

编译程序：



```
# ubuntu18
g++ -o main main.cpp -std=c++11 -I/usr/include/python2.7 -lpython2.7
# ubuntu20
g++ -o main main.cpp -std=c++11 -I/usr/include/python3.8 -lpython3.8

```

### 😆3. 使用说明


下面进行使用分析：


#### 最简单的示例：



```
#include "matplotlibcpp.h"
namespace plt = matplotlibcpp;
int main() {
    plt::plot({1,3,2,4});
    plt::show();
}

```

另一个复杂的示例，将图表保存为图片：



```
#include "matplotlibcpp.h"
#include <cmath>

namespace plt = matplotlibcpp;

int main()
{
    // 数据处理
    int n = 5000;
    std::vector<double> x(n), y(n), z(n), w(n,2);
    for(int i=0; i<n; ++i) {
        x.at(i) = i\*i;
        y.at(i) = sin(2\*M_PI\*i/360.0);
        z.at(i) = log(i);
    }

    // 设置分辨率
    plt::figure\_size(1200, 780);
    // Plot line from given x and y data. Color is selected automatically.
    plt::plot(x, y);
    // Plot a red dashed line from given x and y data.
    plt::plot(x, w,"r--");
    // Plot a line whose name will show up as "log(x)" in the legend.
    plt::named\_plot("log(x)", x, z);
    // 设置x轴
    plt::xlim(0, 1000\*1000);
    // 图表标题
    plt::title("Sample figure");
    // 添加图例
    plt::legend();
    // 保存为照片
    plt::save("./basic.png");
}

```

#### 一个三维图形示例：



```
#include "matplotlibcpp.h"

namespace plt = matplotlibcpp;

int main()
{
    std::vector<std::vector<double>> x, y, z;
    for (double i = -5; i <= 5;  i += 0.25) {
        std::vector<double> x_row, y_row, z_row;
        for (double j = -5; j <= 5; j += 0.25) {
            x_row.push\_back(i);
            y_row.push\_back(j);
            z_row.push\_back(::std::sin(::std::hypot(i, j)));
        }
        x.push\_back(x_row);
        y.push\_back(y_row);
        z.push\_back(z_row);
    }

    plt::plot\_surface(x, y, z);
    plt::show();
}

```

#### 读取txt文件中的xy两列数据并显示：



```
#include <iostream>
#include <fstream>
#include "matplotlibcpp.h"

namespace plt = matplotlibcpp;

int main() {
    std::vector<double> x_data, y_data;
    double x, y;

    std::ifstream file("data.txt");
    if (file.is\_open()) {
        while (file >> x >> y) {
            x_data.push\_back(x);
            y_data.push\_back(y);
        }
        file.close();
    } else {
        std::cerr << "Failed to open file." << std::endl;
        return 1;
    }

    // 绘制图表
    plt::plot(x_data, y_data);
    plt::show();
    
    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





