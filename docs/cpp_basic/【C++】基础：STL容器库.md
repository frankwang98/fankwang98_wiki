>  😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏
>  这篇文章主要介绍STL容器库。
>  **<font color=red>学其所用，用其所学。——梁启超**                          
>  欢迎来到我的博客，一起学习，共同进步。
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

@[toc]
## :smirk:1. STL容器库介绍
`STL 容器库`是 STL 的一个重要组成部分，提供了多种数据结构，包括序列容器、关联容器和容器适配器等，用于存储和管理数据。容器管理着为其元素分配的存储空间，并提供成员函数来直接访问或通过迭代器（具有类似于指针的属性的对象）访问它们。
## :blush:2. 序列容器
### array静态连续数组

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

### vector动态连续数组

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
### deque双端队列
在队列两端都可以进行操作，也可以进行随机下标访问。其操作基本上与std::vector一样，比std::vector多了在头部进行插入和移除的操作。

一般来说，std::vector用在需要频繁进行随机下标访问的场景，如果需要频繁在头部和尾部进行插入和删除操作，则用std::deque。
### forward_list单向链表
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

### list双向链表
## :satisfied:3. 关联容器
### set集合

### map键值对集合

## :satisfied:4. 容器适配器
### stack栈
后进先出数据结构。

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

### queue队列
先进先出数据结构。

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
### priority_queue优先队列
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

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。
