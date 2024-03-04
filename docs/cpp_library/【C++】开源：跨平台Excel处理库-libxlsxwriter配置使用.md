







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Excel处理库-libxlsxwriter配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__50)




### 😏1. 项目介绍


项目Github地址：`https://github.com/jmcnamara/libxlsxwriter`


Wiki地址：`https://libxlsxwriter.github.io/`


libxlsxwriter 是一个用于创建 Microsoft Excel XLSX 文件的C库。它提供了一系列功能，可以让您通过编程方式生成包含单元格、图表、格式化等内容的 Excel 文件。下面是 libxlsxwriter 的一些特点和功能：



> 
> 1.跨平台性：libxlsxwriter 可以在多个操作系统上工作，包括 Linux、macOS 和 Windows。
> 
> 
> 



> 
> 2.创建 XLSX 文档：它允许您创建 XLSX 格式的 Excel 文档，支持 Excel 2007 及更高版本。
> 
> 
> 



> 
> 3.丰富的功能：libxlsxwriter 支持创建工作表、单元格、公式、图表、条件格式化、数据筛选等功能。
> 
> 
> 



> 
> 4.高性能：该库被设计为具有高性能，在大型数据集的情况下生成速度快。
> 
> 
> 



> 
> 5.支持多种格式和样式：您可以设置单元格的格式、字体、颜色、边框、背景等属性，以及应用数值格式、日期格式、公式和函数等。
> 
> 
> 



> 
> 6.支持图表：libxlsxwriter 允许您创建各种 Excel 图表，如条形图、饼图、折线图等，并支持自定义图表的样式和属性。
> 
> 
> 


但是要注意，`libxlsxwriter` 只能用于创建 XLSX 文件，不支持读取或修改现有的 Excel 文件。（可以通过c++自带的文件处理来读取，处理后的数据再手动导入到最后的表中）


### 😊2. 环境配置


下面进行环境配置：



```
# 安装依赖
sudo apt-get install libxslt-dev
# 下载源码
git clone https://github.com/jmcnamara/libxlsxwriter
# 编译
cd libxlsxwriter
make
sudo cp lib/libxlsxwriter.so.5 /usr/lib/libxlsxwriter.so.5
cd cmake
cmake ..
make 
sudo make install

```

编译运行：



```
g++ -o main main.cpp -lxlsxwriter && ./main

```

### 😆3. 使用说明


下面进行使用分析：


创建excel并写入示例：



```
#include "xlsxwriter.h"

int main() {

    /\* Create a new workbook and add a worksheet. \*/
    lxw_workbook  \*workbook  = workbook\_new("demo.xlsx");
    lxw_worksheet \*worksheet = workbook\_add\_worksheet(workbook, NULL);

    /\* Add a format. \*/
    lxw_format \*format = workbook\_add\_format(workbook);

    /\* Set the bold property for the format \*/
    format\_set\_bold(format);

    /\* Change the column width for clarity. \*/
    worksheet\_set\_column(worksheet, 0, 0, 20, NULL);

    /\* Write some simple text. \*/
    worksheet\_write\_string(worksheet, 0, 0, "Hello", NULL);

    /\* Text with formatting. \*/
    worksheet\_write\_string(worksheet, 1, 0, "World", format);

    /\* Write some numbers. \*/
    worksheet\_write\_number(worksheet, 2, 0, 123,     NULL);
    worksheet\_write\_number(worksheet, 3, 0, 123.456, NULL);

    /\* Insert an image. \*/
    worksheet\_insert\_image(worksheet, 1, 2, "logo.png");

    workbook\_close(workbook);

    return 0;
}

```

参考某博主，生成指定个数的uuid示例：



```
#include <chrono>
#include <condition\_variable>
#include <ctime>
#include <curl/curl.h>
#include <curl/easy.h>
#include <fstream>
#include <functional>
#include <future>
#include <iostream>
#include <iomanip>
#include <map>
#include <memory>
#include <mutex>
#include <random>
#include <stdio.h>
#include <string>
#include <uuid/uuid.h>
#include <vector>
#include "xlsxwriter.h"

// ref: https://www.cnblogs.com/Fred1987/p/17442487.html
// 编译： g++ -o main main.cpp -lxlsxwriter -luuid -lpthread && ./main

std::string get\_time\_now(bool is_exact = false)
{
    std::chrono::time_point<std::chrono::high_resolution_clock> now = std::chrono::high_resolution_clock::now();
    time_t raw_time = std::chrono::high_resolution_clock::to\_time\_t(now);
    struct tm tm_info = \*localtime(&raw_time);
    std::stringstream ss;
    ss << std::put\_time(&tm_info, "%Y%m%d%H%M%S");
    if (is_exact)
    {
        std::chrono::seconds seconds = std::chrono::duration\_cast<std::chrono::seconds>(now.time\_since\_epoch());
        std::chrono::milliseconds mills = std::chrono::duration\_cast<std::chrono::milliseconds>(now.time\_since\_epoch());
        std::chrono::microseconds micros = std::chrono::duration\_cast<std::chrono::microseconds>(now.time\_since\_epoch());
        std::chrono::nanoseconds nanos = std::chrono::duration\_cast<std::chrono::nanoseconds>(now.time\_since\_epoch());
        std::uint64\_t mills_count = mills.count() - seconds.count() \* 1000;
        std::uint64\_t micros_count = micros.count() - mills.count() \* 1000;
        std::uint64\_t nanos_count = nanos.count() - micros.count() \* 1000;
        ss << "\_" << std::setw(3) << std::setfill('0') << std::to\_string(mills_count)
           << std::setw(3) << std::setfill('0') << std::to\_string(micros_count)
           << std::setw(3) << std::setfill('0') << std::to\_string(nanos_count);
    }
    return ss.str();
}

// generate uuid
char \*uuid_value = (char \*)malloc(40);
char \*get\_uuid\_value()
{
    uuid_t new_uuid;
    uuid\_generate(new_uuid);
    uuid\_unparse(new_uuid, uuid_value);
    return uuid_value;
}

void xlsxwriter\_excel(const int &len)
{
    lxw_workbook \*workbook = workbook\_new("uuid.xlsx");

    lxw_worksheet \*worksheet = workbook\_add\_worksheet(workbook, NULL);
    worksheet\_set\_column(worksheet,0,0,50,NULL); // 设置列宽

    // write header
    worksheet\_write\_string(worksheet, 0, 0, "Header", NULL);
    worksheet\_write\_string(worksheet, 0, 1, "Number", NULL);

    for (int row = 1; row <= len; row++)
    {
        worksheet\_write\_string(worksheet, row, 0, get\_uuid\_value(), NULL);
        worksheet\_write\_number(worksheet, row, 1, row, NULL);
    }

    workbook\_close(workbook);
    std::cout << get\_time\_now(true) << ",finish in " << __FUNCTION__ << std::endl;
}

int main(int agrs,char \*\*argv)
{
    xlsxwriter\_excel(atoi(argv[1])); // 读取命令行，生成几个uuid
    return 0;
}

```

结合CGAL计算几何库对txt点集进行处理，并处理后的数据写入xlsx，示例：



```
#include <iostream>
#include <fstream>
#include <vector>
#include "xlsxwriter.h"
#include <CGAL/Exact\_predicates\_inexact\_constructions\_kernel.h>
#include <CGAL/Alpha\_shape\_2.h>

typedef CGAL::Exact_predicates_inexact_constructions_kernel K;
typedef K::Point_2 Point_2;

// 编译： g++ -o main main.cpp -lxlsxwriter -lCGAL -lgmp && ./main

int main() {
    // 1. 从文本文件中读取 XY 点集
    std::ifstream inputFile("input.txt");
    if (!inputFile) {
        std::cout << "Failed to open input file." << std::endl;
        return 1;
    }

    std::vector<Point_2> points;
    double x, y;
    while (inputFile >> x >> y) {
        points.push\_back(Point\_2(x, y));
    }
    inputFile.close();

    // ...

    // 2. 将处理后的点集写入 xlsx 文件中
    lxw_workbook\* workbook = workbook\_new("output.xlsx");
    lxw_worksheet\* worksheet = workbook\_add\_worksheet(workbook, NULL);

    int row = 0;
    for (const Point_2& point : points) {
        int col = 0;
        worksheet\_write\_number(worksheet, row, col++, CGAL::to\_double(point.x()), NULL);
        worksheet\_write\_number(worksheet, row, col, CGAL::to\_double(point.y()), NULL);
        row++;
    }

    workbook\_close(workbook);

    std::cout << "Output file has been generated." << std::endl;

    return 0;
}


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





