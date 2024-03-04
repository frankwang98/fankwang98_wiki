







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Boost配置文件解析库PropertyTree配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__38)




### 😏1. 项目介绍


项目Github地址：`https://github.com/boostorg/property_tree`


`Boost.PropertyTree`库是Boost C++库中的一个模块，用于处理配置文件和属性树的操作。它提供了一种方便的方式来读取、写入和操作各种配置文件格式，如`INI、XML、JSON`等。


`Boost.PropertyTree`库的主要特点包括：



> 
> 1.多格式支持：Boost.PropertyTree库支持多种常见的配置文件格式，包括INI、XML、JSON、INFO、CFG等。这使得开发人员可以使用统一的API来处理不同格式的配置文件。
> 
> 
> 



> 
> 2.简单易用：Boost.PropertyTree库提供了简洁的API，使得读取、写入和操作配置文件变得非常容易。开发人员可以使用类似于树结构的方式来访问和修改配置文件中的数据。
> 
> 
> 



> 
> 3.容器友好：Boost.PropertyTree库与STL容器无缝集成，可以方便地将配置文件数据存储到各种容器中，如std::map、std::vector等。这样，开发人员可以根据自己的需求选择适当的数据结构来存储配置文件数据。
> 
> 
> 



> 
> 4.可扩展性：Boost.PropertyTree库是一个可扩展的库，允许开发人员定义自定义数据类型和格式解析器，以支持其他非标准的配置文件格式或特殊需求。
> 
> 
> 



> 
> 5.跨平台支持：Boost库本身是跨平台的，因此Boost.PropertyTree库也具有跨平台的特性，可以在各种操作系统和编译器上使用。
> 
> 
> 


使用`Boost.PropertyTree`库，开发人员可以轻松地读取和写入各种配置文件格式，以及对配置数据进行操作和处理。


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装，包含Boost.PropertyTree属性树模块
sudo apt install libboost-dev

```

编译：



```
g++ -o main main.cpp && ./main

```

### 😆3. 使用说明


INI配置文件解析示例：



```
#include <iostream>
#include <boost/property\_tree/ptree.hpp>
#include <boost/property\_tree/ini\_parser.hpp>

using namespace std;

/\*
[Section1]
Username = john
Password = secret

[Section2]
Port = 8080
\*/

int main() {
    // 创建一个property\_tree对象
    boost::property_tree::ptree pt;
    // 使用ini\_parser库加载INI文件
    boost::property_tree::ini_parser::read\_ini("./data/data.ini", pt);

    // 读取配置项的值
    std::string username = pt.get<std::string>("Section1.Username");
    std::string password = pt.get<std::string>("Section1.Password");
    int port = pt.get<int>("Section2.Port");

    // 打印读取的值
    std::cout << "Username: " << username << std::endl;
    std::cout << "Password: " << password << std::endl;
    std::cout << "Port: " << port << std::endl;

    return 0;
}

```

XML配置文件解析示例：



```
#include <iostream>
#include <boost/property\_tree/ptree.hpp>
#include <boost/property\_tree/xml\_parser.hpp>

/\*
<root>
 <title>Sample Book</title>
 <year>2023</year>
 <author>John Doe</author>
</root>
\*/

int main() {
    // 创建一个property\_tree对象
    boost::property_tree::ptree pt;

    try {
        // 使用xml\_parser库加载XML文件
        boost::property_tree::read\_xml("./data/data.xml", pt);

        // 读取XML节点的值
        std::string title = pt.get<std::string>("root.title");
        int year = pt.get<int>("root.year");
        std::string author = pt.get<std::string>("root.author");

        // 打印读取的值
        std::cout << "Title: " << title << std::endl;
        std::cout << "Year: " << year << std::endl;
        std::cout << "Author: " << author << std::endl;
    } catch (const boost::property_tree::ptree_error& e) {
        // 处理解析错误
        std::cerr << "XML parsing error: " << e.what() << std::endl;
    }

    return 0;
}

```

JSON配置文件解析示例：



```
#include <iostream>
#include <boost/property\_tree/ptree.hpp>
#include <boost/property\_tree/json\_parser.hpp>

/\*
{
 "name": "John Doe",
 "age": 30,
 "address": {
 "city": "New York",
 "street": "123 Main St"
 }
}
\*/

int main() {
    // 创建一个property\_tree对象
    boost::property_tree::ptree pt;

    try {
        // 使用json\_parser库加载JSON文件
        boost::property_tree::read\_json("./data/data.json", pt);

        // 读取JSON节点的值
        std::string name = pt.get<std::string>("name");
        int age = pt.get<int>("age");
        std::string city = pt.get<std::string>("address.city");
        std::string street = pt.get<std::string>("address.street");

        // 打印读取的值
        std::cout << "Name: " << name << std::endl;
        std::cout << "Age: " << age << std::endl;
        std::cout << "City: " << city << std::endl;
        std::cout << "Street: " << street << std::endl;
    } catch (const boost::property_tree::ptree_error& e) {
        // 处理解析错误
        std::cerr << "JSON parsing error: " << e.what() << std::endl;
    }

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





