







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍CGAL计算几何库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__53)




### 😏1. 项目介绍


项目Github地址：`https://github.com/CGAL/cgal`


CGAL（Computational Geometry Algorithms Library）是一个开源的计算几何算法库，它提供了一套丰富的数据结构和算法来解决各种计算几何问题。它是一个功能强大、可靠、高效且易于使用的库。


CGAL 提供了广泛的计算几何算法和数据结构，包括但不限于以下领域：



> 
> 1.2D 和 3D 几何：CGAL 提供了各种数据结构和算法，用于处理二维和三维的点、线段、多边形、曲线、曲面等几何对象。它支持凸包计算、点定位、包围盒计算、空间分割等操作。
> 
> 
> 



> 
> 2.2D 和 3D 三角剖分：CGAL 实现了多种高质量的、高效的三角剖分算法。它支持 Delaunay 三角剖分、Voronoi 图计算、网格重构、约束三角剖分等操作。
> 
> 
> 



> 
> 3.2D 和 3D 网格生成与处理：CGAL 提供了用于生成和处理网格的算法和数据结构。它支持网格生成、网格布尔运算、网格修复、网格优化、封闭表面重构等操作。
> 
> 
> 



> 
> 4.几何优化：CGAL 实现了多个几何优化算法，用于求解几何优化问题，如最小凸包、最小旋转包、最长空间线段等。
> 
> 
> 



> 
> 5.多边形和非封闭曲线处理：CGAL 支持进行多边形布尔运算、多边形修复、多边形拟合、轮廓计算等操作。它还提供了对非封闭曲线的操作和处理。
> 
> 
> 



> 
> 6.曲面重建：CGAL 提供了多个用于重建曲面的算法，包括点云重建、隐函数重建、流形重建等。这些算法可用于从离散的点集生成平滑的曲面模型。
> 
> 
> 



> 
> 7.拓扑关系和空间搜索：CGAL 支持计算几何对象之间的拓扑关系，如相交、包含、相交点等。它还提供了用于空间搜索的数据结构和算法，如 kd-树、R 树等。
> 
> 
> 


CGAL 使用 C++ 编写，具有良好的可扩展性和可移植性。它还与其他库和工具集成，在计算机图形学、计算机辅助设计、计算机辅助制造、机器人学、仿真和科学计算等领域得到了广泛应用。


### 😊2. 环境配置


下面进行环境配置：


apt安装的是老版本4.x，建议源码安装，这里我选的5.1.1.



```
# apt安装
sudo apt install libcgal-dev
# 源码安装
# 依赖
sudo apt install build-essential libboost-all-dev libgmp-dev libmpfr-dev libopencv-dev
从 `https://github.com/CGAL/cgal/releases/tag/v5.1.1` 下载zip
mkdir build
cd build
cmake -DCGAL\_HEADER\_ONLY=OFF -DCMAKE\_BUILD\_TYPE=Release -DCMAKE\_INSTALLED\_PREFIX=../install ..
make
sudo make install

```

编译运行：



```
g++ -o main main.cpp -lCGAL -lgmp
./main

```

### 😆3. 使用说明


下面进行使用分析：


计算点集的凸包算法示例：



```
#include <iostream>
#include <vector>
#include <CGAL/Exact\_predicates\_inexact\_constructions\_kernel.h>
#include <CGAL/convex\_hull\_2.h>

typedef CGAL::Exact_predicates_inexact_constructions_kernel K;
typedef K::Point_2 Point;
typedef std::vector<Point> PointVector;

int main()
{
    // 创建点向量
    PointVector points, result;

    // 添加一些二维点到点向量中
    points.push\_back(Point(1, 1));
    points.push\_back(Point(2, 3));
    points.push\_back(Point(4, 2));
    points.push\_back(Point(3, 1));
    points.push\_back(Point(2, 2));
    points.push\_back(Point(3, 3));
    points.push\_back(Point(3, 2));
    points.push\_back(Point(5, 4));
    points.push\_back(Point(5, 1));
    points.push\_back(Point(4, 3));
    points.push\_back(Point(4, 4));

    // 输出点向量
    std::cout << "点集 Points:" << std::endl;
    for (const auto &p : points)
    {
        std::cout << "(" << p.x() << ", " << p.y() << ")" << std::endl;
    }

    // 计算点集的凸包
    CGAL::convex\_hull\_2(points.begin(), points.end(), std::back\_inserter(result));

    // 确定绘制区域的边界框
    double min_x = result[0].x(); 
    double max_x = result[0].x();
    double min_y = result[0].y();
    double max_y = result[0].y();

    // 输出凸包的点坐标
    std::cout << "凸包点 Convex Hull Points:" << std::endl;
    for (const auto &p : result)
    {
        std::cout << "(" << p.x() << ", " << p.y() << ")" << std::endl;
        min_x = std::min(min_x, p.x());
        max_x = std::max(max_x, p.x());
        min_y = std::min(min_y, p.y());
        max_y = std::max(max_y, p.y());
    }

    // 在终端用ASCII字符简单绘制
    int width = static\_cast<int>(max_x - min_x) + 1;
    int height = static\_cast<int>(max_y - min_y) + 1;

    // 创建并初始化绘制区域
    std::vector<std::vector<char>> canvas(height, std::vector<char>(width, '.'));

    // 在绘制区域上绘制点
    for (const auto& p : result)
    {
        int x = static\_cast<int>(p.x() - min_x);
        int y = static\_cast<int>(p.y() - min_y);
        canvas[y][x] = '#';
    }

    // 输出绘制结果
    std::cout << "绘制结果 #为凸包点: " << std::endl;
    for (int y = height - 1; y >= 0; --y)
    {
        for (int x = 0; x < width; ++x)
        {
            std::cout << canvas[y][x];
        }
        std::cout << std::endl;
    }

    return 0;
}

```

结果：



```
点集 Points:
(1, 1)
(2, 3)
(4, 2)
(3, 1)
(2, 2)
(3, 3)
(3, 2)
(5, 4)
(5, 1)
(4, 3)
(4, 4)
凸包点 Convex Hull Points:
(1, 1)
(5, 1)
(5, 4)
(4, 4)
(2, 3)
绘制结果 #为凸包点: 
...##
.#...
.....
#...#

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





