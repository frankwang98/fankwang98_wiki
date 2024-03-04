> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍tinyxml2解析库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. 项目介绍

`tinyxml2`是一个轻量级的C++库，用于解析和生成XML文档。它是对原始`tinyxml`库的改进和扩展，提供了更快速、更强大的XML处理功能。

以下是一些`tinyxml2`的主要特点和功能：

> 1.简单易用：TinyXML-2提供了简单的API，使得解析和生成XML文档变得简单和直观。它使用类似于DOM（文档对象模型）的方法来操作XML元素，让开发者可以轻松地读取和写入XML数据。

> 2.轻巧高效：TinyXML-2具有非常小的内存占用和高性能。它专注于简单的XML操作，没有复杂的依赖关系，因此可以快速加载和处理大型XML文件。

> 3.支持解析和生成：TinyXML-2支持从字符串或文件中解析XML文档，并且可以生成格式良好的XML文本。它能够处理各种节点类型，如元素、属性、文本、注释等。

> 4.错误处理：TinyXML-2提供了灵活的错误处理机制。当解析XML时，它可以检测到语法错误、结构错误或其他问题，并提供相关的错误信息和异常处理机制。

> 5.跨平台：TinyXML-2可以在多个操作系统上使用，包括Windows、Linux和Mac OS等。

### 😊2. 环境配置

项目Github地址：`https://github.com/leethomason/tinyxml2`

```bash
# apt安装
sudo apt install libtinyxml2-dev

```

```bash
# 源码编译
git clone https://github.com/leethomason/tinyxml2.git
cd tinyxml2
make
sudo make install
# 查看版本
pkg-config --modversion tinyxml2

```

`g++`编译：`g++ -o main main.cpp -ltinyxml2`


### 😆3. 使用说明

#### 写入xml文件

```cpp
#include <iostream>
#include "tinyxml2.h"

int main() {
    // 创建XML文档
    tinyxml2::XMLDocument doc;

    // 创建根元素
    tinyxml2::XMLElement* root = doc.NewElement("Data");
    doc.InsertFirstChild(root);

    // 添加数字
    tinyxml2::XMLElement* numberElement = doc.NewElement("Number");
    numberElement->SetText(42);
    root->InsertEndChild(numberElement);

    // 添加字符串
    tinyxml2::XMLElement* stringElement = doc.NewElement("String");
    stringElement->SetText("Hello, world!");
    root->InsertEndChild(stringElement);

    // 添加数组
    tinyxml2::XMLElement* arrayElement = doc.NewElement("Array");
    int arrayData[] = {1, 2, 3, 4, 5};
    for (int i = 0; i < 5; i++) {
        tinyxml2::XMLElement* item = doc.NewElement("Item");
        item->SetText(arrayData[i]);
        arrayElement->InsertEndChild(item);
    }
    root->InsertEndChild(arrayElement);

    // 保存XML文档到文件
    doc.SaveFile("data/data.xml");

    std::cout << "XML file created successfully." << std::endl;

    return 0;
}

```

#### 读取xml文件

```cpp
#include <iostream>
#include "tinyxml2.h"

int main() {
    // 加载XML文件
    tinyxml2::XMLDocument doc;
    if (doc.LoadFile("data/data.xml") != tinyxml2::XML_SUCCESS) {
        std::cout << "Failed to load the file." << std::endl;
        return 1;
    }

    // 获取根元素
    tinyxml2::XMLElement* root = doc.FirstChildElement("Data");
    if (!root) {
        std::cout << "Failed to find the root element." << std::endl;
        return 1;
    }

    // 读取数字
    int number;
    if (auto numberElement = root->FirstChildElement("Number")) {
        numberElement->QueryIntText(&number);
        std::cout << "Number: " << number << std::endl;
    } else {
        std::cout << "Failed to find the Number element." << std::endl;
    }

    // 读取字符串
    if (auto stringElement = root->FirstChildElement("String")) {
        const char* stringValue = stringElement->GetText();
        if (stringValue) {
            std::cout << "String: " << stringValue << std::endl;
        } else {
            std::cout << "Failed to get the String value." << std::endl;
        }
    } else {
        std::cout << "Failed to find the String element." << std::endl;
    }

    // 读取数组
    if (auto arrayElement = root->FirstChildElement("Array")) {
        for (auto item = arrayElement->FirstChildElement("Item"); item; item = item->NextSiblingElement("Item")) {
            int value;
            item->QueryIntText(&value);
            std::cout << "Array Item: " << value << std::endl;
        }
    } else {
        std::cout << "Failed to find the Array element." << std::endl;
    }

    return 0;
}

```

#### xml地图解析

项目github地址（推荐学习）：`https://github.com/chenyongzhe/HdmapEngine`

这个地图解析引擎项目用`tinyxml2`库解析`apollo opendrive xml`格式的高精地图，包含道路、车道连接关系、信号灯等元素，以及车道搜索、wgs84转东北天等工具，最后可用`python matplotlib`将处理完的地图show出来。

下面是一些解析示例：

```cpp
// 读取xml文件，去判断Node
bool HdmapEngine::paserApolloxml(const char *file_name)
{
  tinyxml2::XMLDocument doc;
  if (doc.LoadFile(file_name) != XML_SUCCESS)
  return false;

  XMLElement *root = doc.RootElement();
  XMLElement *roadNode = root->FirstChildElement("road"); // find road node

  while (roadNode != NULL)
  {
    string name = roadNode->Attribute("name"); // road name
    // if name include string("Road") or string("junction")
    if (name.substr(0, 4) == "Road")
    {
      // cout << roadNode->Attribute("id") << endl;
      paserRoad(roadNode);
    }
    else if (name.substr(0, 8) == "junction")
    {
      paserJunction(roadNode);
    }

    roadNode = roadNode->NextSiblingElement();
  }

  // 创建搜索树
  vector<Point> centerLintePoints;
  for (int i = 0; i < laneList.size(); i++)
  {
    // if(laneList[i].centerLinePoints!=NULL)
    for (int j = 0; j < laneList[i]->centerLinePoints.size(); j++)
    {
      centerLintePoints.push_back(*(laneList[i]->centerLinePoints[j]));
    }
  }
  // cout<<"centerpoint size "<<centerLintePoints.size()<<endl;
  tree->read_in(centerLintePoints);
  // cout<<"centerpoint size "<<centerLintePoints.size()<<endl;
  // cout<<"创建搜索树成功"<<endl;
  // printRoad();

  return true;
}

```

```cpp
// 解析road元素
bool HdmapEngine::paserRoad(XMLElement *roadNode)
{
   string predecessor_elementType;
   int predecessor_id = INT_MAX;
   int successor_id = INT_MAX;
   string successor_elementType;
   double road_length;
   road_id = atoi(roadNode->Attribute("id"));

   XMLElement *linkNode = roadNode->FirstChildElement("link");
   XMLElement *lanes = roadNode->FirstChildElement("lanes");
   XMLElement *laneSection = lanes->FirstChildElement("laneSection");

   XMLElement *sucNode = linkNode->FirstChildElement("successor");

   if (sucNode != NULL)
   {
      // cout << sucNode->Attribute("elementType") << " " << sucNode->Attribute("elementId") << endl;
      successor_elementType = sucNode->Attribute("elementType");

      successor_id = atoi(sucNode->Attribute("elementId"));
   }
   XMLElement *preNode = linkNode->FirstChildElement("predecessor");
   if (preNode != NULL)
   {
      // cout << preNode->Attribute("elementType") << " " << preNode->Attribute("elementId") << endl;
      predecessor_elementType = preNode->Attribute("elementType");

      predecessor_id = atoi(preNode->Attribute("elementId"));
   }

   Road *road = new Road(road_id, predecessor_elementType, predecessor_id, successor_elementType, successor_id);

   int jun = atoi(roadNode->Attribute("junction"));
   if (jun == -1)
   {
      // 非路口道路
      road->isJunctionRoad = false;
   }
   else
   {
      // 路口道路
      road->isJunctionRoad = true;
   }

   laneSection_id = 0;
   // lanesection
   while (laneSection != NULL)
   {
      paserLaneSection(laneSection, road);
      laneSection_id++;
      laneSection = laneSection->NextSiblingElement("laneSection");
   }

   // 解析stopline
   paserStopLineCrosswalk(roadNode, road);

   road->length = road->getRoadLength();
   roadList.push_back(road);
   roadMap[road->road_id] = road;

   return true;
}

```

`gps转xyz`工具部分：

```cpp
bool TransformUtil::gps2xyz(const double &longitude, const double &latitude, const double &altitude,Eigen::Vector3d &xyz)
{
  // gps << gps_msg.longitude, gps_msg.latitude, gps_msg.altitude;
  Eigen::Vector3d gps(longitude, latitude, altitude);
  Eigen::Vector3d gps_ECEF = WGS84toECEF(gps);

  // 处理GPS数据
  double rad_lon = gps_origin_[1] / 180 * M_PI;
  double rad_lat = gps_origin_[0] / 180 * M_PI;
  double sin_lon = sin(rad_lon);
  double cos_lon = cos(rad_lon);
  double sin_lat = sin(rad_lat);
  double cos_lat = cos(rad_lat);

  Eigen::Matrix3d rot = Eigen::Matrix3d::Zero();
  // clang-format off
  rot << -sin_lon, cos_lon, 0,
        -sin_lat * cos_lon, -sin_lat * sin_lon, cos_lat,
        cos_lat * cos_lon, cos_lat * sin_lon, sin_lat;
  // clang-format on

  Eigen::Vector3d diff_ECEF = gps_ECEF - origin_ECEF_;
  Eigen::Vector3d xyz_ECEF = rot * diff_ECEF;
  xyz << xyz_ECEF[0], xyz_ECEF[1], xyz_ECEF[2];
  
  return true;
}

```

以上。





