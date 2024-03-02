### 😏1. 异常处理介绍

在C++中，异常处理是一种用于捕获和处理程序运行期间产生的错误情况的机制。异常处理允许我们在程序中指定可能会引发异常的代码块，并定义相应的处理逻辑。

C++ 异常处理涉及到的类和关键字有：

`std::exception`：是所有标准异常类的基类。可以自定义继承自std::exception的异常类。

`std::runtime_error`：表示运行时错误的异常类，如逻辑错误、资源不足等。

`std::logic_error`：表示逻辑错误的异常类，如无效参数、空指针等。

`try、catch、throw`：是C++中用于处理异常的关键字。

* try：包含可能抛出异常的代码块，用于监视异常。
* catch：用于捕获并处理异常的代码块。
* throw：用于抛出异常

### 😊2. 常见错误

1.`语法错误`：这些错误通常是由于缺少分号、括号不匹配、拼写错误等导致的。

```cpp
int x = 5  // 缺少分号
if (x > 0)  // 缺少右括号
cout << "Hello, World!" << endl;  // 拼写错误（应为 std::cout）

```

2.`类型错误`：这些错误通常是由于变量类型不匹配或者类型转换错误导致的。

```cpp
int x = "Hello";  // 类型不匹配（应为 char\* 或 std::string）
double result = 10 / 3;  // 整数除法结果赋给浮点数类型（应为 10.0 / 3.0）

```

3.`数组越界`：这些错误通常是由于访问数组时超出了有效索引范围导致的。

```cpp
int arr[3] = {1, 2, 3};
int x = arr[3];  // 超出数组索引范围

```

4.`空指针错误`：这些错误通常是由于访问空指针导致的。

```cpp
int\* ptr = nullptr;
\*ptr = 10;  // 访问空指针

```

5.`逻辑错误`：这些错误通常是由于程序逻辑错误或算法错误导致的。

```cpp
for (int i = 0; i < 5; i--) {
    cout << i << " ";
}  // 循环条件错误（导致无限循环）

```

6.`内存泄漏`：这些错误通常是由于未正确释放动态分配的内存导致的。

```cpp
while (true) {
    int\* ptr = new int[100];
}  // 未释放动态分配的内存导致内存泄漏

```

### 😆3. 异常处理

简单的异常处理示例（除数为0）：

```cpp
#include <iostream>
#include <stdexcept>

int divide(int a, int b) {
    if (b == 0) {
        throw std::runtime\_error("Division by zero!");  // 抛出异常
    }
    return a / b;
}

int main() {
    try {
        int result = divide(10, 0);  // 在 try 块中调用可能引发异常的函数
        std::cout << "Result: " << result << std::endl;
    } catch (const std::exception& ex) {
        std::cout << "Exception caught: " << ex.what() << std::endl;  // 捕获并处理异常
    }
    
    return 0;
}

```

以上。
