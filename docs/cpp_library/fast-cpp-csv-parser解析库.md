> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍fast-cpp-csv-parser数据解析库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. 项目介绍

项目Github地址：`https://github.com/ben-strasser/fast-cpp-csv-parser`

`fast-cpp-csv-parser` 是一个快速、轻量级的C++ CSV解析库，用于解析和处理逗号分隔值（`CSV`）文件。它专注于提供高性能和低内存占用，并提供简单易用的API。

以下是 `fast-cpp-csv-parser` 的一些特点和功能：

> 1.快速解析：fast-cpp-csv-parser 的设计目标之一是提供快速的CSV解析性能。它使用高效的算法和数据结构，以最小的开销解析大型CSV文件。

> 2.低内存占用：该库在解析过程中使用较少的内存，这对于处理大型CSV文件或有限的内存环境非常有用。

> 3.简单易用的API：fast-cpp-csv-parser 提供了简洁的API，使CSV文件的解析和访问变得容易。它支持逐行解析、按列索引访问和按列名称访问等。

> 4.自定义选项：您可以根据需要配置解析器的选项，如分隔符、引号字符、是否跳过空行等。这使得它适应不同的CSV文件格式。

> 5.跨平台支持：fast-cpp-csv-parser 可在多个平台上运行，包括Windows、Linux和macOS。

`fast-cpp-csv-parser` 中有 `LineReader` 和 `CSVReader` 两个类，其中`LineReader` 类用于按行读取文本文件，而不关心是否是CSV格式，它提供了逐行读取文件的功能，可以用于处理任何文本文件；`CSVReader` 类是 fast-cpp-csv-parser 的主要类，专门用于解析和处理CSV文件，并可进行配置以满足需求。

### 😊2. 环境配置

该库是一个单头文件的解析库，因此只需将`csv.h`包含在项目中就可以。

```bash
# 编译
g++ -o main main.cpp -lpthread

```

### 😆3. 使用说明

CSVReader解析CSV文件示例：

```cpp
#include <iostream>
#include "csv.h"

/*
Name, Age, City
a, 10, city\_a
b, 11, city\_b
c, 12, city\_c
d, 13, city\_d
e, 14, city\_e
*/

int main() {
    io::CSVReader<3> csv("example.csv");  // 创建CSVReader对象，指定CSV文件名和列数

    // 设置CSV列名
    csv.read\_header(io::ignore_extra_column, "Name", "Age", "City");

    std::string name;
    int age;
    std::string city;

    // 逐行解析CSV文件并访问每一列的数据
    while (csv.read\_row(name, age, city)) {
        // 在此处对解析的数据进行处理
        std::cout << "Name: " << name << ", Age: " << age << ", City: " << city << std::endl;
    }

    return 0;
}

```

标准库解析CSV示例（对比）：

```cpp
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>

std::vector<std::vector<std::string>> parseCSV(const std::string& filename, char delimiter) {
    std::vector<std::vector<std::string>> data;

    std::ifstream file(filename);  // 打开CSV文件

    if (!file.is\_open()) {
        std::cout << "Failed to open file: " << filename << std::endl;
        return data;
    }

    std::string line;
    while (std::getline(file, line)) {
        std::vector<std::string> row;
        std::stringstream ss(line);
        std::string cell;

        while (std::getline(ss, cell, delimiter)) {
            row.push\_back(cell);  // 将每个单元格的数据添加到行向量中
        }

        data.push\_back(row);  // 将每行数据添加到数据向量中
    }

    file.close();  // 关闭文件

    return data;
}

int main() {
    std::vector<std::vector<std::string>> data = parseCSV("example.csv", ',');  // 解析CSV文件

    // 遍历解析后的数据并打印到控制台
    for (const auto& row : data) {
        for (const auto& cell : row) {
            std::cout << cell << "\t";
        }
        std::cout << std::endl;
    }

    return 0;
}

```

以上。