### 😏1. 标准库介绍

C++ 标准库是 C++ 语言提供的标准库，它包含了多个模块（头文件），每个模块提供了不同的功能。

C++ 标准模板库（STL，Standard Template Library）是 C++ 的一部分，提供了一组通用的模板类和函数，用于实现常见的数据结构和算法。

STL主要包含以下三个组件：

- **容器（Containers）**：  
容器是STL中用于存储和管理数据的类模板。STL提供了各种不同类型的容器，包括动态数组（vector）、双向链表（list）、队列（queue）、栈（stack）、集合（set）、映射（map）等。每种容器都具有不同的特点和适用场景，开发人员可以根据需要选择合适的容器来存储和操作数据。

- **算法（Algorithms）**：  
算法是STL中用于处理容器中数据的函数模板。STL提供了大量的算法，包括查找、排序、合并、替换、计数等。这些算法实现了常见的数据处理操作，并且对于多数情况下都有高效的实现。开发人员可以通过简单地调用这些算法，而无需自己实现复杂的数据处理逻辑。

- **迭代器（Iterators）**：  
迭代器是STL中用于遍历容器中元素的抽象概念。通过使用迭代器，开发人员可以在不关心具体容器实现的情况下，对容器中的元素进行迭代和访问。STL提供了多种类型的迭代器，包括输入迭代器、输出迭代器、正向迭代器、双向迭代器和随机访问迭代器。不同类型的迭代器支持不同的操作和功能，开发人员可以根据需要选择适合的迭代器。

STL的优点有：

1. 可重用性：STL提供了通用的数据结构和算法，可以在不同的项目和场景中重复使用，避免了重复编写相似的代码。  
2. 高效性：STL中的容器和算法都经过了优化，具有高效的实现。STL使用了模板和内联函数等技术，在编译时生成高效的代码。  
3. 可扩展性：STL支持用户自定义类型的容器和算法，可以根据实际需求进行扩展和定制。  
4. 代码可读性和可维护性：STL提供了一致的接口和命名规范，使得代码更易于理解和维护。同时，STL中的容器和算法都经过了广泛测试和验证，具有较高的可靠性。

两个在线学习地址：

```bash
https://cui-jiacai.gitbook.io/c++-stl-tutorial/
https://zoupers.gitbook.io/cpp-17-stl-cookbook/

```

### 😊2. 常用容器模块

容器主要有序列容器（array、vector、dequeue、forward_list、list）和关联容器（map、set、multimap、multiset），分别用于不同的数据存储和访问方式。

#### string：字符串，抽象char*

```cpp
std::string str("hello");
// 后面追加
str.append(" world");
// 前面插入
str.insert(0, "ok,");
// 截取子字符串
str = str.substr(3);
printf("sub str=%s\n", str.c_str());
// 空格替换为逗号
for (int i = 0; i < str.size(); ++i) {
    char ch = str.at(i);
    if (ch == ' ') {
        str.replace(i, 1, 1, ',');
    }
}
// 后面添加字符
str.push_back('!');
// 删除指定位置的字符
str.erase(str.size() - 1);
size_t pos = str.find("world");
printf("find pos=%ld\n", pos);
// 判断字符串是否相等，== 属于操作符重载
if (str == "hello,world") {
    // ...
}

```

#### array静态连续数组

```cpp
#include <iostream>
#include <array>

using namespace std;

int main() {
    // 静态连续数组
    array<int, 5> a = {1, 2, 3, 4, 5};
    cout << a[0] << endl;
    cout << a.at(1) << endl; // error:at(20)

    // 初始化数组
    array<int, 10> a1;
    a1.fill(10);
    for (auto &n : a1)
    {
        cout << n << endl;
    }

    // 使用迭代器遍历
    array<int, 10>::iterator it = a1.begin(); // auto it
    for (; it != a1.end(); it++)
    {
        cout << *it << endl;
    }

    // array也可创建自定义数据类型

    return 0;
}
```

#### vector：动态数组，支持快速随机访问。

```cpp
#include <iostream>
#include <vector>
#include <array>
using namespace std;
int main()
{
    std::vector<int> demo{ 1,2 };

    demo.push_back(1); //push_back操作

    std::vector<int> test{ 3,4,5 };
    demo.insert(demo.end(), test.begin(), test.end()); //inset操作

    // 循环打印值
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }

    // 使用迭代器 iterator 访问值
    vector<int>::iterator v = demo.begin();
    while (v != demo.end()) {
        cout << "value of v = " << *v << endl;
        v++;
    }
    return 0;
}

```

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    vector<int> vec(3, 100);
    vec.reserve(6); // 预留5个位置
    vec.push_back(10);
    cout << (uintptr_t)vec.data() << endl;
    vec.push_back(110); // insert
    cout << (uintptr_t)vec.data() << endl;

    vec.emplace_back(120); // emplace原位构造，复杂数据
    cout << (uintptr_t)vec.data() << endl;

    cout << vec.size() << endl;
    cout << vec.capacity() << endl;

    for (auto &n : vec)
    {
        cout << n << "\t";
    }

    return 0;
}
```

#### list：双向链表，支持高效的插入和删除操作。

```cpp
#include <iostream>
#include <list>

int main() {
    // 创建一个空的list容器
    std::list<int> myList;

    // 在容器尾部插入元素
    myList.push_back(10);
    myList.push_back(20);
    myList.push_back(30);

    // 在容器头部插入元素
    myList.push_front(5);

    // 使用迭代器遍历输出容器中的元素
    std::cout << "List elements: ";
    for (auto it = myList.begin(); it != myList.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 在指定位置插入元素
    auto insertPos = std::find(myList.begin(), myList.end(), 20);
    if (insertPos != myList.end()) {
        myList.insert(insertPos, 15);
    }

    // 使用迭代器反向遍历输出容器中的元素
    std::cout << "Reversed List elements: ";
    for (auto rit = myList.rbegin(); rit != myList.rend(); ++rit) {
        std::cout << *rit << " ";
    }
    std::cout << std::endl;

    // 移除指定值的元素
    myList.remove(10);

    // 清空容器
    myList.clear();

    return 0;
}

```

#### forward_list单向链表
单向链表迭代器只能做自增，不能与数字相加减，也不能两个迭代器相减。

```cpp
#include <iostream>
#include <forward_list>

using namespace std;

// 一个元素返回true时移除对应元素
bool pre(const int &val)
{
    return val > 3; // 移除大于3的元素
}

int main() {
    forward_list<int> fls = {5, 6, 2, 3, 1};
    forward_list<int> fls2 = {0, 4, 17, 12, 15,18};

    // 升序排序/降序排序
    fls.sort(); // fls.reverse();
    fls2.sort();

    // 移除元素
    fls.remove(3);
    fls.remove_if(pre);
    // 也可以用lambda表达式
    fls.remove_if([](const int &val) { return val > 3; });

    // 合并
    fls2.merge(fls);

    for (auto &v : fls2)
    {
        cout << v << "\t\t";
    }

    return 0;
}
```

#### queue：队列

```cpp
std::queue<int> queue;
// 入队
queue.push(111);
queue.push(222);
queue.push(333);
printf("queue front=%d, back=%d", queue.front(), queue.back());
// 出队
while (!queue.empty()) {
    int front = queue.front();
    queue.pop();
}

```

```cpp
#include <iostream>
#include <queue>

using namespace std;

int main()
{
    queue<const char *> q;

    // 入队，如果是复合数据类型，用emplace就地构造代替push入队
    q.push("he");
    q.push("ll");
    q.push("o");

    // 出队
    while (!q.empty())
    {
        const char *name = q.front(); // 先获取队首元素
        q.pop(); // 将队首元素出队
        cout << name << "已出队，感觉良好。队里还有" << q.size() << "个元素" << endl;
    }


    return 0;
}
```

#### deque：双端队列，支持首尾插入和删除操作

在队列两端都可以进行操作，也可以进行随机下标访问。其操作基本上与std::vector一样，比std::vector多了在头部进行插入和移除的操作。

一般来说，std::vector用在需要频繁进行随机下标访问的场景，如果需要频繁在头部和尾部进行插入和删除操作，则用std::deque。

```cpp
#include <iostream>
#include <deque>

int main() {
    // 创建一个空的deque容器
    std::deque<int> myDeque;

    // 在容器尾部插入元素
    myDeque.push_back(10);
    myDeque.push_back(20);
    myDeque.push_back(30);

    // 在容器头部插入元素
    myDeque.push_front(5);

    // 使用索引访问和输出容器中的元素
    std::cout << "Deque elements: ";
    for (size_t i = 0; i < myDeque.size(); ++i) {
        std::cout << myDeque[i] << " ";
    }
    std::cout << std::endl;

    // 在指定位置插入元素
    auto insertPos = myDeque.begin() + 2;
    myDeque.insert(insertPos, 15);

    // 使用迭代器遍历输出容器中的元素
    std::cout << "Deque elements after insertion: ";
    for (auto it = myDeque.begin(); it != myDeque.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 移除指定位置的元素
    auto erasePos = myDeque.begin() + 1;
    myDeque.erase(erasePos);

    // 使用迭代器反向遍历输出容器中的元素
    std::cout << "Reversed Deque elements: ";
    for (auto rit = myDeque.rbegin(); rit != myDeque.rend(); ++rit) {
        std::cout << *rit << " ";
    }
    std::cout << std::endl;

    // 清空容器
    myDeque.clear();

    return 0;
}

```

#### set：集合，存储唯一值，并按照一定的排序规则进行自动排序。

```cpp
#include <iostream>
#include <set>

int main() {
    // 创建一个空的set容器
    std::set<int> mySet;

    // 向容器中插入元素
    mySet.insert(10);
    mySet.insert(20);
    mySet.insert(20);  // 重复的元素将被忽略
    mySet.insert(30);

    // 判断元素是否存在
    if (mySet.find(20) != mySet.end()) {
        std::cout << "Element 20 is found in the set." << std::endl;
    } else {
        std::cout << "Element 20 is not found in the set." << std::endl;
    }

    // 输出容器中的元素个数
    std::cout << "Size of set: " << mySet.size() << std::endl;

    // 使用范围循环遍历输出容器中的元素
    std::cout << "Set elements: ";
    for (const auto& element : mySet) {
        std::cout << element << " ";
    }
    std::cout << std::endl;

    // 移除元素
    int key = 10;
    mySet.erase(key);

    // 检查容器是否为空
    if (mySet.empty()) {
        std::cout << "Set is empty." << std::endl;
    } else {
        std::cout << "Set is not empty." << std::endl;
    }

    return 0;
}

```

#### map：映射，存储键值对，按照键的大小进行自动排序。

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main()
{
    map<string,int> phone_book; // 创建一个map类容器，用于存储电话号码簿

    // 创建电话簿
    phone_book["zhangsan"] = 12345678; // 通过[]操作和关键字往容器中加入元素
    phone_book["lisi"] = 87654321;
    phone_book["wanger"] = 56781234;
    phone_book.insert(std::pair<string, int("zhangtian", 65458997));
    // ......
    // 输出电话号码簿
    cout << "电话号码簿的信息如下：\n";
    for (pair<string, int item: phone_book) {
        // for (auto it = phone_book,begin(); it != phone_book.end(); it++)
        // C++11中引入的enhanced-for loop
        cout << item.first << ": " << item.second << endl; 
        // 输出元素的姓名和电话号码
    }

    // 查找某个人的电话号码
    string name;
    cout << "请输入要查询号码的姓名：";
    cin >> name;
    map<string,int::const_iterator it; // 创建一个不能修改所指向的元素的迭代器
    it = phone_book.find(name); // 查找关键字为name的容器元素
    if (it == phone_book.end()) // 判断是否找到
        cout << name << ": not found\n"; // 未找到
    else
        cout << it-first << ": " << it-second << endl; // 找到
    return 0;
}

```

#### unordered_set：无序不重复元素集合，存储唯一值，并提供常数时间的查找操作。

```cpp
#include <iostream>
#include <unordered_set>

int main() {
    // 创建一个 unordered_set
    std::unordered_set<int> mySet;

    // 插入元素
    mySet.insert(10);
    mySet.insert(20);
    mySet.insert(30);

    // 查找元素
    if (mySet.find(20) != mySet.end()) {
        std::cout << "Element 20 found in set" << std::endl;
    } else {
        std::cout << "Element 20 not found in set" << std::endl;
    }

    // 删除元素
    mySet.erase(10);

    // 遍历集合
    for (const auto& elem : mySet) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;

    return 0;
}
```


#### unordered_map：无序映射，存储键值对，并提供常数时间的查找操作。

```cpp
#include <iostream>
#include <unordered_map>

int main() {
    // 创建一个 unordered_map，将字符串映射到整数
    std::unordered_map<std::string, int> myMap;

    // 插入键值对
    myMap["apple"] = 5;
    myMap["banana"] = 10;
    myMap["orange"] = 7;

    // 查找元素
    if (myMap.find("banana") != myMap.end()) {
        std::cout << "Price of banana: " << myMap["banana"] << std::endl;
    } else {
        std::cout << "Banana not found in map" << std::endl;
    }

    // 修改元素
    myMap["orange"] = 8;

    // 遍历 map
    for (const auto& pair : myMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    return 0;
}
```


### 😆3. 常用算法模块

#### sort：对容器进行排序。

```cpp
std::vector<int array{3, 6, 1, 5, 9, 2, 8};
std::sort(array.begin(), array.end());
for (int it: array) {
    printf("%d ", it);
}

```

#### find：在容器中查找指定元素。

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;
int main() 
{
    int a[10] = { 10,20,30,40 };
    vector<int> v;
    v.push_back(1);    v.push_back(2);
    v.push_back(3);    v.push_back(4); //此后v里放着4个元素：1,2,3,4

    // 定义迭代器，用find查找
    vector<int>::iterator p;
    p = find(v.begin(), v.end(), 3); //在v中查找3
    if (p != v.end()) //若找不到,find返回 v.end()
        cout << "1. " << *p << endl; //找到了
    
    p = find(v.begin(), v.end(), 9);
    if (p == v.end())
        cout << "not found " << endl; //没找到

    p = find(v.begin() + 1, v.end() - 1, 4); //在,3 这两个元素中查找4
    cout << "2. " << *p << endl;

    int * pp = find(a, a + 4, 20);
    if (pp == a + 4)
        cout << "not found" << endl;
    else
        cout << "3. " << *pp << endl;

    return 0;
}

```

#### binary_search：二分查找

```cpp
bool result = std::binary_search(array.begin(), array.end(), 8);

```

#### remove：从容器中移除指定值。


#### transform：对容器中的元素应用某个操作并存储结果。


#### accumulate：计算容器中元素的累加值。


#### count：计算容器中满足条件的元素个数。


#### reverse：反转容器中的元素顺序。

```cpp
std::reverse(array.begin(), array.end());

```

#### replace：替换

```cpp
std::replace(array.begin(), array.end(), 6, 666);

```

### 😆4. 其他模块

#### 函数对象（Function Objects）

STL提供了函数对象类模板，允许用户自定义函数对象，即重载函数自定义运算符 operator() 的对象（也称为**仿函数**），以便在算法中使用。  
函数对象是一个行为类似于函数的对象，可以重载函数调用运算符 operator()()。  
使用函数对象可以实现更加灵活的算法操作，包括自定义的排序规则、条件判断等。

```cpp
#include <iostream>

// 定义一个函数对象类
class MyFunctor {
public:
    void operator()(int x) const {
        std::cout << "Square of " << x << " is: " << x * x << std::endl;
    }
};

int main() {
    // 创建函数对象实例
    MyFunctor myFunctor;
    // 使用函数对象调用它的 operator() 方法
    myFunctor(5); // 输出：Square of 5 is: 25

    // 可以直接创建临时函数对象并调用
    MyFunctor()(8); // 输出：Square of 8 is: 64

    return 0;
}
```

#### 适配器（Adapters）

STL提供了适配器类模板，用于将容器或迭代器的接口进行适配或扩展，以满足特定的需求。 
常见的适配器有 `stack、queue、priority_queue`，它们在底层使用了不同的容器实现，并且提供了特定的接口和功能。

```cpp
#include <iostream>
#include <stack>

int main() {
    std::stack<int> myStack;

    // 将元素压入栈中
    myStack.push(10);
    myStack.push(20);
    myStack.push(30);

    // 访问栈顶元素
    std::cout << "Top element of stack: " << myStack.top() << std::endl;

    // 弹出栈顶元素
    myStack.pop();

    // 检查栈是否为空
    if (myStack.empty()) {
        std::cout << "Stack is empty" << std::endl;
    } else {
        std::cout << "Stack is not empty" << std::endl;
    }

    return 0;
}
```

```cpp
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int main()
{
    stack<string> str_stack;

    // 入栈， 如果是复合数据结构，用emplace就地构造代替push入栈
    str_stack.push("H");
    str_stack.push("e");
    str_stack.push("l");
    str_stack.push("lo");

    // 出栈
    while(!str_stack.empty())
    {
        string str = str_stack.top(); // 先用top获取到栈顶元素
        str_stack.pop(); // 弹出栈顶元素
        cout << str << "--已出栈，感觉良好。栈里还有" << str_stack.size() << "个元素" << endl; 
    }

    return 0;
}
```

#### priority_queue优先队列
可以根据优先级的高低确定出队顺序的数据结构。如果是复合数据类型，需要提供比较函数或者重载<运算符。

```cpp
#include <iostream>
#include <queue>

int main() {
    // 创建一个最大堆优先队列，存储 int 类型的元素
    std::priority_queue<int> maxHeap;

    // 添加元素到优先队列中
    maxHeap.push(3);
    maxHeap.push(5);
    maxHeap.push(1);
    maxHeap.push(7);
    maxHeap.push(2);

    // 打印优先队列中的元素（不会按顺序打印）
    std::cout << "Elements in priority queue:" << std::endl;
    while (!maxHeap.empty()) {
        std::cout << maxHeap.top() << " ";
        maxHeap.pop(); // 弹出队首元素
    }
    std::cout << std::endl;

    return 0;
}
```

#### 分配器（Allocators）

STL允许用户自定义内存分配器，用于控制容器内部的内存管理和分配策略。  
用户可以通过实现自己的分配器类来满足特定的内存管理需求，例如内存池、定制的内存分配策略等。

```cpp
#include <iostream>
#include <vector>
#include <memory>

int main() {
    // 创建使用 std::allocator 的 vector
    std::vector<int, std::allocator<int>> myVector;

    // 向 vector 添加元素
    for (int i = 1; i <= 5; i++) {
        myVector.push_back(i);
    }

    // 遍历并打印 vector 中的元素
    for (const auto& element : myVector) {
        std::cout << element << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

#### 迭代器模块（Iterator Tags）

STL中引入了迭代器标签的概念，用于表示迭代器的类型和特性。  
迭代器标签包括输入迭代器、输出迭代器、前向迭代器、双向迭代器和随机访问迭代器，用于指定迭代器支持的操作和功能。

- 输入迭代器（Input Iterators）：只读访问容器中的元素。

- 输出迭代器（Output Iterators）：只写访问容器中的元素。

- 正向迭代器（Forward Iterators）：支持读写容器中的元素，并能够向前遍历。

- 双向迭代器（Bidirectional Iterators）：支持正向和逆向遍历。

- 随机访问迭代器（Random Access Iterators）：支持任意位置的高效随机访问。

#### 元编程技术（Metaprogramming Techniques）

STL广泛使用元编程技术，包括模板特化、模板偏特化、模板元编程、SFINAE（Substitution Failure Is Not An Error）等。 
元编程技术可以在编译期间进行计算和代码生成，以提高代码的效率和灵活性。

```cpp
#include <iostream>

template <int N>
struct Fibonacci {
    static const int value = Fibonacci<N - 1>::value + Fibonacci<N - 2>::value;
};

template <>
struct Fibonacci<0> {
    static const int value = 0;
};

template <>
struct Fibonacci<1> {
    static const int value = 1;
};

int main() {
    const int result = Fibonacci<10>::value;
    std::cout << "Fibonacci number at position 10 is: " << result << std::endl;

    return 0;
}
```

#### 异步操作

`异步操作`是一种编程模型，用于处理任务的非阻塞执行和事件驱动。在传统的同步操作中，程序会等待一个任务完成后才继续执行下一个任务，而在异步操作中，任务可以在后台执行，程序可以继续执行其他任务而无需等待当前任务完成。

使用C++11提供的`std::async`和`std::future`来实现异步任务示例：

```cpp
#include <iostream>
#include <future>

// 异步任务
int asyncTask(int input) {
    // 模拟耗时的操作
    std::this_thread::sleep_for(std::chrono::seconds(2));

    return input * 2;
}

int main() {
    // 启动异步任务，并获取 std::future 对象
    std::future<int> futureResult = std::async(std::launch::async, asyncTask, 5);

    // 在主线程中进行其他操作...

    // 获取异步任务的结果
    int result = futureResult.get();

    std::cout << "异步任务的结果为：" << result << std::endl;

    return 0;
}

```

此外，还包含memory内存管理库、thread多线程库、chrono时间库等。

## 数组

## 栈和队列

### 基本计算器（表达式求值）

```cpp
class Solution {
public:
    int calculate(string s) {
        // 用栈stack这种类型 只有加减的表达式
        stack<int> ops;
        ops.push(1);
        int sign = 1;

        int ret = 0;
        int n = s.length();
        int i = 0;
        while (i < n) {
            if (s[i] == ' ') {
                i++;
            } else if (s[i] == '+') {
                sign = ops.top();
                i++;
            } else if (s[i] == '-') {
                sign = -ops.top();
                i++;
            } else if (s[i] == '(') {
                ops.push(sign);
                i++;
            } else if (s[i] == ')') {
                ops.pop();
                i++;
            } else {
                long num = 0;
                while (i < n && s[i] >= '0' && s[i] <= '9') {
                    num = num * 10 + s[i] - '0';
                    i++;
                }
                ret += sign * num;
            }
        }
        return ret;
    }
};
```

## 单调栈

## 优先队列

## 双端队列

### 滑动窗口最大值
```cpp
#include <iostream>
#include <vector>
#include <deque>

using namespace std;

vector<int> slidingWindowMax(const vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;

    for (int i = 0; i < nums.size(); ++i) {
        // 在队尾保持递减顺序
        while (!dq.empty() && nums[i] >= nums[dq.back()]) {
            dq.pop_back();
        }
        dq.push_back(i);

        // 如果队头元素已不在窗口范围内，弹出
        if (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }

        // 将窗口内的最大值加入结果
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }

    return result;
}

int main() {
    vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
    int k = 3;
    
    vector<int> result = slidingWindowMax(nums, k);

    cout << "滑动窗口最大值：";
    for (int num : result) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```

## 哈希表

## 多重集合和映射

以上。
