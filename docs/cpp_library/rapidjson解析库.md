> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍rapidjson数据解析库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. 项目介绍

项目Github地址：`https://github.com/Tencent/rapidjson`

`RapidJSON` 是一个快速的 C++ JSON 解析器/生成器，具有高效的内存利用和低延迟。它是一个轻量级的、模块化的、功能齐全的 JSON 库，广泛应用于 C++ 程序中用于处理 JSON 数据。

RapidJSON 的特点包括：

> 1.快速高效：RapidJSON 通过最大程度地优化内存使用和计算效率来实现快速的 JSON 解析和生成，它在性能上表现出色。

> 2.标准兼容：RapidJSON 完全符合 JSON 标准（RFC 8259），可以处理各种合法的 JSON 数据。

> 3.模块化设计：RapidJSON 的设计非常模块化，允许用户根据自己的需求选择性地使用特定的功能模块，从而减少了库的大小和依赖关系。

> 4.可扩展性：RapidJSON 支持用户自定义分配器来管理内存分配，也支持自定义解析错误处理策略，使其在不同的应用场景下具有很好的灵活性。

> 5.跨平台：RapidJSON 可以在各种操作系统和编译器上运行，包括 Windows、Linux、macOS 等。

`RapidJSON` 提供了简单易用的 API，使得解析和生成 JSON 数据变得非常便捷。通过 RapidJSON，你可以轻松地在 C++ 程序中处理 JSON 数据，包括解析 JSON 字符串和构建 JSON 对象。

### 😊2. 环境配置

下面进行环境配置：

```bash
# apt安装
sudo apt install rapidjson-dev
# g++编译不用加 -l

```

### 😆3. 使用说明

下面进行使用分析：

#### 解析json数据示例：

```cpp
#include <iostream>
#include "rapidjson/document.h"
#include "rapidjson/writer.h"
#include "rapidjson/stringbuffer.h"

int main() {
    const char\* json = R"(
    {
    "name": "John",
    "age": 30,
    "isStudent": true,
    "scores": [85, 90, 88]
    }
    )";

    rapidjson::Document document;
    document.Parse(json);

    if (document.IsObject()) {
        // 提取字符串类型数据
        std::string name = document["name"].GetString();
        std::cout << "Name: " << name << std::endl;

        // 提取整数类型数据
        int age = document["age"].GetInt();
        std::cout << "Age: " << age << std::endl;

        // 提取布尔类型数据
        bool isStudent = document["isStudent"].GetBool();
        std::cout << "Is Student: " << (isStudent ? "true" : "false") << std::endl;

        // 提取数组类型数据
        const rapidjson::Value& scores = document["scores"];
        if (scores.IsArray()) {
            std::cout << "Scores: ";
            for (rapidjson::SizeType i = 0; i < scores.Size(); i++) {
                std::cout << scores[i].GetInt() << " ";
            }
            std::cout << std::endl;
        }
    }

    return 0;
}

```

#### 写入json数据示例：

```cpp
#include "rapidjson/document.h"
#include "rapidjson/writer.h"
#include "rapidjson/stringbuffer.h"
#include <iostream>

int main() {
    rapidjson::Document document;
    document.SetObject();

    // 添加字符串类型数据
    rapidjson::Value name;
    name.SetString("John", document.GetAllocator());
    document.AddMember("name", name, document.GetAllocator());

    // 添加整数类型数据
    rapidjson::Value age;
    age.SetInt(30);
    document.AddMember("age", age, document.GetAllocator());

    // 添加布尔类型数据
    rapidjson::Value isStudent;
    isStudent.SetBool(true);
    document.AddMember("isStudent", isStudent, document.GetAllocator());

    // 添加数组类型数据
    rapidjson::Value scores(rapidjson::kArrayType);
    scores.PushBack(85, document.GetAllocator());
    scores.PushBack(90, document.GetAllocator());
    scores.PushBack(88, document.GetAllocator());
    document.AddMember("scores", scores, document.GetAllocator());

    // 将 Document 对象序列化成 JSON 字符串
    rapidjson::StringBuffer buffer;
    rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
    document.Accept(writer);

    // 输出 JSON 字符串
    std::cout << buffer.GetString() << std::endl;

    return 0;
}

```

#### 从文件中解析json

```cpp
#include "rapidjson/document.h"
#include "rapidjson/filereadstream.h"
#include <cstdio>
#include <iostream>

int main() {
    // 打开 JSON 文件
    FILE\* fp = fopen("data.json", "r");
    if (fp == nullptr) {
        std::cerr << "Failed to open input.json" << std::endl;
        return 1;
    }

    // 读取文件内容
    char readBuffer[65536];
    rapidjson::FileReadStream is(fp, readBuffer, sizeof(readBuffer));

    // 创建 RapidJSON 解析器
    rapidjson::Document document;
    document.ParseStream(is);

    // 关闭文件
    fclose(fp);

    // 检查解析是否成功
    if (document.HasParseError()) {
        std::cerr << "JSON parse error: " << rapidjson::GetParseErrorFunc(document.GetParseError()) << std::endl;
        return 1;
    }

    // 访问和处理 JSON 数据
    if (document.IsObject()) {
        const rapidjson::Value& name = document["name"];
        const rapidjson::Value& age = document["age"];
        const rapidjson::Value& isStudent = document["isStudent"];

        std::cout << "Name: " << name.GetString() << std::endl;
        std::cout << "Age: " << age.GetInt() << std::endl;
        std::cout << "Is Student: " << (isStudent.GetBool() ? "Yes" : "No") << std::endl;
    } else {
        std::cerr << "JSON is not an object" << std::endl;
        return 1;
    }

    return 0;
}

```

#### 将json数据写入文件

```cpp
#include "rapidjson/document.h"
#include "rapidjson/writer.h"
#include "rapidjson/stringbuffer.h"
#include <iostream>
#include <fstream>

int main() {
    rapidjson::Document document;
    document.SetObject();

    // 添加字符串类型数据
    rapidjson::Value name;
    name.SetString("John", document.GetAllocator());
    document.AddMember("name", name, document.GetAllocator());

    // 添加整数类型数据
    rapidjson::Value age;
    age.SetInt(30);
    document.AddMember("age", age, document.GetAllocator());

    // 添加布尔类型数据
    rapidjson::Value isStudent;
    isStudent.SetBool(true);
    document.AddMember("isStudent", isStudent, document.GetAllocator());

    // 将 Document 对象序列化成 JSON 字符串
    rapidjson::StringBuffer buffer;
    rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
    document.Accept(writer);

    // 将 JSON 字符串写入文件
    std::ofstream outputFile("output.json");
    if (outputFile.is\_open()) {
        outputFile << buffer.GetString();
        outputFile.close();
        std::cout << "JSON data has been written to output.json" << std::endl;
    } else {
        std::cerr << "Failed to open output.json for writing" << std::endl;
    }

    return 0;
}

```

以上。





