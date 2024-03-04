> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍jsoncpp的使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 

### 😏1. jsoncpp介绍

JsonCpp是一个开源的C++库，用于解析、生成和操作JSON格式数据。它支持标准的JSON语法，并具有良好的扩展性和可定制性。

该库提供了简单易用的API，可以轻松地实现JSON数据的读取、写入、修改和查询等操作。它还提供了丰富的错误处理机制和文档化的代码示例，使得初学者也能快速上手。

JsonCpp支持所有主流的C++编译器和操作系统平台，并且在多个开源项目中被广泛应用，如OpenCV、ROS等。同时，该库还提供了Python和Java等其他编程语言的绑定，方便跨语言使用。

JsonCpp是一个功能强大、易用性高、性能优秀的C++ JSON库，为JSON数据的处理提供了便利和效率。

### 😊2. jsoncpp安装

ubuntu apt安装比较简单：

```bash
sudo apt-get install libjsoncpp-dev

```

引用头文件：

```cpp
#include "jsoncpp/json/json.h

```

编译：

```bash
g++ main.cpp -o main -ljsoncpp

```

### 😆3. jsoncpp入门使用

#### 从字符串读取

```cpp
#include "jsoncpp/json/json.h"
#include <iostream>
#include <memory>
/*
 \* \brief Parse a raw string into Value object using the CharReaderBuilder
 \* class, or the legacy Reader class.
 \* Example Usage:
 \* $g++ readFromString.cpp -ljsoncpp -std=c++11 -o readFromString
 \* $./readFromString
 \* colin
 \* 20
*/
int main() {
  const std::string rawJson = R"({"Age": 20, "Name": "colin"})";
  const auto rawJsonLength = static\_cast<int>(rawJson.length());
  constexpr bool shouldUseOldWay = false;
  JSONCPP_STRING err;
  Json::Value root;

  if (shouldUseOldWay) {
    Json::Reader reader;
    reader.parse(rawJson, root);
  } else {
    Json::CharReaderBuilder builder;
    const std::unique_ptr<Json::CharReader> reader(builder.newCharReader());
    if (!reader->parse(rawJson.c\_str(), rawJson.c\_str() + rawJsonLength, &root,
                       &err)) {
      std::cout << "error" << std::endl;
      return EXIT_FAILURE;
    }
  }
  const std::string name = root["Name"].asString();
  const int age = root["Age"].asInt();

  std::cout << name << std::endl;
  std::cout << age << std::endl;
  return EXIT_SUCCESS;
}

```

#### 写入到字符串

```cpp
#include "jsoncpp/json/json.h"
#include <iostream>
/*
 \ brief Write a Value object to a string.
 \* Example Usage:
 \* $g++ stringWrite.cpp -ljsoncpp -std=c++11 -o stringWrite
 \* $./stringWrite
 \* {
 \* "action" : "run",
 \* "data" :
 \* {
 \* "number" : 1
 \* }
 \* }
 */
int main() {
  Json::Value root;
  Json::Value data;
  constexpr bool shouldUseOldWay = false;
  root["action"] = "run";
  data["number"] = 1;
  root["data"] = data;	// 嵌套

  if (shouldUseOldWay) {
    Json::FastWriter writer;
    const std::string json_file = writer.write(root);
    std::cout << json_file << std::endl;
  } else {
    Json::StreamWriterBuilder builder;
    const std::string json_file = Json::writeString(builder, root);
    std::cout << json_file << std::endl;
  }
  return EXIT_SUCCESS;
}

```

#### 从文件中读取

package.json

```json
{
  "name": "tmp",
  "version": "1.0.0",
  "dependencies": {
  }
}

```

readFile.cpp

```cpp
#include <iostream>
#include <fstream>
#include <jsoncpp/json/json.h>

using namespace std;

int main(){
   ifstream ifs("package.json");
   Json::Reader reader;
   Json::Value obj;
   reader.parse(ifs, obj);
   cout << " name " << obj["name"].asString() << endl;
   return 0;
}

```

#### 写入到文件

fileWrite.cpp

```cpp
#include <iostream>
#include <fstream>
#include <jsoncpp/json/json.h>

using namespace std;

int main(){
  fstream fs;
  fs.open("package_new.json",ios::out); // ios::out|ios::app为追加
  Json::Value root;
  Json::Value data;
  root["action"] = "run";
  data["number"] = 1;
  root["data"] = data;	// 嵌套

  Json::StreamWriterBuilder builder;
  const std::string json_file = Json::writeString(builder, root);
  // std::cout << json\_file << std::endl;
  
  fs << json_file << endl;  // 写入文件
  fs.close();
  return 0;
}

```

以上。





