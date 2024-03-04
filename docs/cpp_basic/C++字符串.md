### 😏1. 字符串库string介绍

在C++中，`std::string`是一个表示字符串的类，它是C++标准库中的一部分。`std::string`提供了许多功能和操作，使得字符串的处理更加方便和高效。

参考：`https://zh.cppreference.com/w/cpp/string/basic_string`

`std::string`的使用使得字符串的处理更加方便和安全，它提供了许多成员函数和操作符重载来简化字符串的操作。在C++中，`std::string`通常是首选的字符串表示方式，而不是C风格的字符数组。

### 😊2. 字符串构造

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // 1.无参构造
    string str;

    // 2.用=赋值
    string str2 = "hello";

    // 3.用char*传参
    string str3("ABC");

    // 4.重复字符构造
    string str4(10, 'A');

    // 5.拷贝构造
    string str5(str2);

    // 6.移动构造
    string str6(move(str5));

    // 7.构造指定范围内的字符
    string str7(str2, 3, 2);

    // 8.字符串拼接
    string str8 = str2 + " " + str3;
    cout << str8 << endl;
    
    return 0;
}

```

### 😆3. 元素访问

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // 元素访问有以下几种方式
    // 1.at()
    // 2.[]
    // 3.front()
    // 4.back()
    // 5.c_str()
    // 6.data()

    string str = "hello world";
    str.at(2) = 'a';
    cout << str.at(2) << endl;
    cout << str[1] << endl;
    cout << str.front() << endl;
    cout << str.back() << endl;
    cout << str.c_str() << endl;
    cout << str.data() << endl;
    
    return 0;
}

```

### 😆4. 容量操作

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // 容量操作
    // 1.empty()
    // 2.size() length() 字节
    // 3.max_size()
    // 4.resize(size_type cnt) resize(size_type cnt, CharT ch)
    // 5.reserve()
    // 6.capacity()
    // 7.shrink_to_fit()
    string str = "hello world";
    string str2 = "你好";
    cout << boolalpha << str.empty() << endl;
    cout << str2.size() << endl; // 一个汉字3字节
    cout << str.length() << endl;
    cout << str.max_size() << endl;
    // str.resize(10);
    str.resize(20, 'A');
    cout << str << endl;
    cout << str.capacity() << endl; // 分配的容量

    str.reserve(100); // 分配100个字符的容量
    cout << str.capacity() << endl;
    str.shrink_to_fit(); // 减少容量
    cout << str.capacity() << endl;
    
    return 0;
}

```

### 😆5. 迭代器

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // 迭代器 - 为容器类提供统一的遍历接口
    // 1.正向迭代器 iterator
    // 2.正向只读迭代器 const_iterator
    // 3.反向迭代器 reverse_iterator
    // 4.反向只读迭代器 const_reverse_iterator
    string str = "hello world";
    string::iterator it = str.begin(); // auto string::iterator(不可修改)
    cout << *it << endl; // h
    it++; // it += 2
    cout << *it << endl; // e
    *it = 'E';
    cout << *it << endl; // E
    cout << str << endl;

    for (; it != str.end(); it++)
    {
        cout << *it << endl; // print
    }

    string::reverse_iterator rit = str.rbegin();
    for (; rit != str.rend(); rit++)
    {
        cout << *rit << endl; // reverse print
    }
    
    return 0;
}

```

### 😆6. 字符串相关操作

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // 字符串相关操作
    // 1.==
    // 2.compare()
    // 3.starts_with() ends_with() // std20
    // 4.contains() // std23
    // 5.append()
    // 6.insert()
    // 7.push_back()
    // 8.pop_back()
    // 9.clear()
    // 10.erase()
    // 11.replace()
    // 12.substr()
    // 13.find()
    // 14.rfind()
    // 15.to_string()
    // 16.stoi() stof()
    // 17.hash
    string str = "hello world";
    string str2 = "Hello World";

    cout << boolalpha << (str == str2) << endl;
    cout << (str2.compare(str) == 0) << endl;

    cout << str.append("!!!").append(" C++") << endl;
    cout << str.insert(5, ",") << endl;
    str.push_back('!');
    str.pop_back();
    cout << str << endl;
    // cout << str.erase(5, 6) << endl;
    // str.clear();

    str.replace(str.begin(), str.end(), str2);
    cout << str << endl;
    cout << str.substr(2) << endl;

    cout << str.find("lo") << endl; // 用 npos 判断索引
    cout << str.rfind("l") << endl;

    cout << stoi("123") << endl;
    cout << stof("123.45") << endl;
    cout << to_string(123) << endl;

    hash<string> hs;
    cout << hs(str) << endl; // 计算哈希值
    
    return 0;
}

```

### 😆7. string_view

```cpp
#include <iostream>
#include <string>
#include <string_view>

using namespace std;

int main()
{
    cout << __cplusplus << endl;  // -std=c++17
    
    // string_view 字符串共享内存，避免重新分配
    const char *s = "hello, c++ stl";
    cout << uintptr_t(s) << endl;

    string_view sv = s;
    cout << uintptr_t(sv.data()) << endl;
    cout << sv << endl;

    // 移除字节
    sv.remove_prefix(7);
    cout << sv << endl;
    
    return 0;
}

```

以上。
