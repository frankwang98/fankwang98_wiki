







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍单元测试框架gtest配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__36)




### 😏1. 项目介绍


项目Github地址：`https://github.com/google/googletest.git`


`Google Test`（简称为 `gtest`）是一个流行的 C++ 测试框架，用于编写和执行单元测试、集成测试和功能测试。它是 Google 开发的开源项目，旨在提供简单、灵活和可扩展的测试解决方案。以下是对 Google Test 的一些重要特点和功能的介绍：



> 
> 1.易于入门和使用：Google Test 提供了简洁而直观的 API，使得编写和运行测试用例非常容易。它遵循 xUnit 风格的测试框架设计，并提供了丰富的断言宏来验证预期结果。
> 
> 
> 



> 
> 2.支持多种测试类型：Google Test 支持单元测试、集成测试和功能测试。你可以使用它来编写针对函数、类、模块或整个应用程序的测试。
> 
> 
> 



> 
> 3.参数化测试：Google Test 允许你使用参数化测试来覆盖不同的输入和参数组合。你可以使用 TEST\_P 和 INSTANTIATE\_TEST\_SUITE\_P 宏来定义和实例化参数化测试。
> 
> 
> 



> 
> 4.固件（Fixture）支持：Google Test 支持测试固件的概念，允许你在测试之前和之后设置和清理共享资源。通过使用 TEST\_F 宏定义测试固件，可以方便地在多个测试用例之间共享初始化和清理代码。
> 
> 
> 



> 
> 5.丰富的断言：Google Test 提供了丰富的断言宏来验证预期结果。例如，你可以使用 EXPECT\_EQ 来检查两个值是否相等，或使用 EXPECT\_TRUE 来验证条件是否为真。
> 
> 
> 



> 
> 6.输出详细信息：Google Test 在测试运行过程中会生成详细的输出信息，包括测试结果、失败原因和附加信息等。这些信息有助于诊断问题和快速修复错误。
> 
> 
> 



> 
> 7.可扩展性：Google Test 具有良好的可扩展性，允许你编写自定义的测试扩展和辅助函数。你可以根据需要创建自己的断言宏、打印函数和参数生成器等。
> 
> 
> 



> 
> 8.平台支持：Google Test 支持多种平台和编译器，包括 Windows、Linux、macOS 和各种 C++ 编译器。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libgtest-dev
# 编译运行
g++ -o main main.cpp test.cpp -lgtest -lgtest\_main -pthread  && ./main

```

### 😆3. 使用说明


gtest使用示例（通过判断等于EQ、对错TRUE来输出单元测试结果）：



```
// test.h
#ifndef MATH\_UTILS\_H
#define MATH\_UTILS\_H

int Add(int a, int b);
int Subtract(int a, int b);
bool IsPrime(int number);

#endif // MATH\_UTILS\_H

```


```
// test.cpp
#include "test.h"

int Add(int a, int b) {
  return a + b;
}

int Subtract(int a, int b) {
  return a - b;
}

bool IsPrime(int number) {
  if (number < 2) {
    return false;
  }
  for (int i = 2; i <= number / 2; ++i) {
    if (number % i == 0) {
      return false;
    }
  }
  return true;
}

```


```
// main.cpp
#include "gtest/gtest.h"
#include "test.h"

// 测试 Add() 函数
TEST(MathUtilsTest, AddTest) {
  EXPECT\_EQ(4, Add(2, 2));
  EXPECT\_EQ(-1, Add(2, -3));
  EXPECT\_EQ(10, Add(5, 5));
}

// 测试 Subtract() 函数
TEST(MathUtilsTest, SubtractTest) {
  EXPECT\_EQ(2, Subtract(5, 3));
  EXPECT\_EQ(-5, Subtract(0, 5));
  EXPECT\_EQ(0, Subtract(10, 10));
}

// 测试 IsPrime() 函数
TEST(MathUtilsTest, IsPrimeTest) {
  EXPECT\_TRUE(IsPrime(2));
  EXPECT\_TRUE(IsPrime(3));
  EXPECT\_FALSE(IsPrime(4));
  EXPECT\_TRUE(IsPrime(5));
  EXPECT\_FALSE(IsPrime(6));
  EXPECT\_TRUE(IsPrime(7));
}

int main(int argc, char\*\* argv) {
  testing::InitGoogleTest(&argc, argv);
  /\* 用TEST宏定义测试用例，验证函数的行为和结果是否符合预期 \*/
  return RUN\_ALL\_TESTS();
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





