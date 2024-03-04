### 😏1. 知识介绍

C++程序中的内存分为两个部分：`栈`（在函数内部声明的所有变量都将使用栈内存）和`堆`（程序中未使用的内存，在程序运行时可用于动态分配内存）。

C++ 中，可以使用`new`和`delete`运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。

malloc() 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不使用 malloc() 函数。new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还创建了对象。

### 😊2. 动态内存与示例

C++中的动态内存分配是一种在程序运行时按需分配和释放内存的机制。与静态内存（由编译器在编译时分配）和自动内存（由编译器在函数调用时自动分配和释放）不同，动态内存提供了更大的灵活性和控制能力。

动态内存管理需要特别注意以下几点：

* 必须手动释放通过 new 分配的内存，以防止内存泄漏。
* 不能重复释放已经释放的内存，这会导致未定义的行为。
* 动态分配的内存应该在不再使用时及时释放，以避免内存泄漏和资源浪费。

使用动态内存分配时，请确保谨慎操作，避免内存泄漏和悬空指针等问题，并根据需要及时释放动态分配的内存。

double变量示例：

```cpp
#include <iostream>
using namespace std;
 
int main ()
{
   double\* pvalue  = NULL; // 初始化为 null 的指针
   pvalue  = new double;   // 为变量请求内存
 
   \*pvalue = 29494.99;     // 在分配的地址存储值
   cout << "Value of pvalue : " << \*pvalue << endl;
 
   delete pvalue;         // 释放内存
 
   return 0;
}

```

二维数组示例：

```cpp
#include <iostream>
using namespace std;
 
int main()
{
    int \*\*p;   
    int i,j;   //p[4][8] 
    //开始分配4行8列的二维数据 
    p = new int \*[4];
    for(i=0;i<4;i++){
        p[i]=new int [8];
    }
 
    for(i=0; i<4; i++){
        for(j=0; j<8; j++){
            p[i][j] = j\*i;
        }
    }   
    //打印数据 
    for(i=0; i<4; i++){
        for(j=0; j<8; j++)     
        {   
            if(j==0) cout<<endl;   
            cout<<p[i][j]<<"\t";   
        }
    }   
    //开始释放申请的堆 
    for(i=0; i<4; i++){
        delete [] p[i];   
    }
    delete [] p;   
    return 0;
}

```

对象创建示例：

```cpp
#include <iostream>
using namespace std;
 
class Box
{
   public:
      Box() { 
         cout << "调用构造函数！" <<endl; 
      }
      ~Box() { 
         cout << "调用析构函数！" <<endl; 
      }
};
 
int main( )
{
   Box\* myBoxArray = new Box[4];
 
   delete [] myBoxArray; // 删除数组
   return 0;
}

```

### 😆3. 智能指针与示例

C++智能指针是一种用于自动管理动态分配的内存的指针类模板。它们提供了更安全和方便的方式来管理动态内存，减少内存泄漏和悬空指针等问题。

C++标准库中提供了三种智能指针：`std::unique_ptr`、`std::shared_ptr` 和 `std::weak_ptr`。

std::unique\_ptr 是 C++11 引入的智能指针，它具有独占性质。一个 std::unique\_ptr 拥有对其所指向对象的唯一所有权，并在其生命周期结束时自动释放内存。

```cpp
#include <memory>

void uniquePtrExample() {
    std::unique_ptr<int> ptr = std::make\_unique<int>(42);
    std::cout << \*ptr << std::endl;  // 输出: 42
    // 不需要手动释放内存，当 unique\_ptr 离开作用域时会自动释放内存
}

```

std::shared\_ptr 是 C++11 引入的智能指针，可以共享对象的所有权。多个 std::shared\_ptr 对象可以同时拥有对同一个对象的所有权，并且会跟踪引用计数。只有当所有 std::shared\_ptr 对象都释放了其对对象的所有权时，该对象才会被销毁。

```cpp
#include <memory>

void sharedPtrExample() {
    std::shared_ptr<int> ptr1 = std::make\_shared<int>(42);
    std::shared_ptr<int> ptr2 = ptr1;
    std::cout << \*ptr1 << " " << \*ptr2 << std::endl;  // 输出: 42 42
    // 不需要手动释放内存，当所有 shared\_ptr 离开作用域后才会自动释放内存
}

```

std::weak\_ptr 也是 C++11 引入的智能指针，它用于解决 std::shared\_ptr 的循环引用问题。std::weak\_ptr 允许你观测一个对象，但不拥有它，因此不会增加引用计数。可以使用 std::weak\_ptr 来检查所观测的对象是否已被销毁。

```cpp
#include <memory>

void weakPtrExample() {
    std::shared_ptr<int> sharedPtr = std::make\_shared<int>(42);
    std::weak_ptr<int> weakPtr = sharedPtr;

    if (auto ptr = weakPtr.lock()) {
        // weak\_ptr 可以被成功转换为 shared\_ptr，表示所观测的对象还存在
        std::cout << \*ptr << std::endl;  // 输出: 42
    } else {
        // 所观测的对象已经被销毁
        std::cout << "对象已被销毁" << std::endl;
    }
}

```

使用智能指针可以简化动态内存管理，减少手动释放内存的繁琐和错误。智能指针的选择取决于具体的需求，例如独占所有权或共享所有权。使用智能指针还可以避免循环引用导致的内存泄漏。

### 😏1. 字符型数组输出：

```cpp
char str[10] = {'1', '2'};
cout << str << endl;	//输出1 2

```

### 😊2. 非字符型数组输出：

```cpp
int arr[10] = {1, 2, 3};
cout << arr << endl;	//会按16进制输出a的值（地址）

```

### 😆3. 如果要输出非字符型数组中的内容，则需要采用循环打印：

```cpp
int arr[10] = { 1, 2, 3 };
for (int i = 0; i < 10; i++)
{
	cout << arr[i] << endl;	//输出1 2 3 0 0 0 0 0 0 0
}

```

### 😆4. 循环打印2个字符型数组（双for循环）：

```cpp
char str1[10] = { 'A', 'B', 'C' };
char str2[10] = { 'D', 'E', 'F' };
for (int i = 0; i < 3; i++)
{
	for (int j = 0; j < 3; j++)
	{
		cout << str1[i] << ' ' << str2[j] << ';';
		//输出A D;A E;A F;B D;B E;B F;C D;C E;C F;
	}
}

```

以上。





