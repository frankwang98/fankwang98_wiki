






为了学好Unity，先来复习一下C#基础。




#### 文章目录


* + [C#认识](#C_3)
	+ [第一个C#程序](#C_21)
	+ [C#基本语法](#C_45)
	+ [C#数组](#C_93)
	+ [C#字符串](#C_125)




### C#认识


C#是微软公司在2000年6月发布的一种新的编程语言，继承于C/C++，因此也具有面向对象的特点；在此基础上，微软还进行了简化处理，使得开发者容易上手且不用担心内存问题。


C# 是 .Net 框架的一部分，且用于编写 .Net 应用程序。C# 文件的后缀为 `.cs`。与 Java 不同的是，文件名可以不同于类的名称。


以下是 C# 一些重要的功能：


* 布尔条件（Boolean Conditions）
* 自动垃圾回收（Automatic Garbage Collection）
* 标准库（Standard Library）
* 组件版本（Assembly Versioning）
* 属性（Properties）和事件（Events）
* 委托（Delegates）和事件管理（Events Management）
* 易于使用的泛型（Generics）
* 索引器（Indexers）
* 条件编译（Conditional Compilation）
* 简单的多线程（Multithreading）
* LINQ 和 Lambda 表达式
* 集成 Windows


### 第一个C#程序


参考学习：[菜鸟教程C#](https://www.runoob.com/csharp/csharp-tutorial.html)



```
using System;
// 声明namespace
namespace HelloWorldApplication
{
	// 声明class
    class HelloWorld
    {
    	// main入口
        static void Main(string[] args)
        {
            /\* 我的第一个 C# 程序\*/
            Console.WriteLine("Hello C#!");
            Console.ReadKey();	// 等待操作
        }
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/af6b924f3b8e4837bd13fcd882fe70b8.png)


### C#基本语法


看一个计算长方形面积的例子：



```
using System;
namespace RectangleApplication
{
    class Rectangle
    {
        // 成员变量
        public double length;
        public double width;
        // 成员函数Acceptdetails
        public void Acceptdetails()
        {
            length = 4.5;    
            width = 3.5;
        }
        // 成员函数：GetArea
        public double GetArea()
        {
            return length \* width;
        }
        // 成员函数：Display
        public void Display()
        {
            Console.WriteLine("Length: {0}", length);
            Console.WriteLine("Width: {0}", width);
            Console.WriteLine("Area: {0}", GetArea());
        }
    }
    
    class ExecuteRectangle
    {
        static void Main(string[] args)
        {
            Rectangle r = new Rectangle();
            r.Acceptdetails();
            r.Display();
            Console.ReadLine();
        }
    }
}


```

可以看出，与C++不同的是，类中的成员变量和成员函数前都要加上访问控制符（public、private、protected、internal、protected internal）。另外，如果没有指定访问修饰符，则使用类成员的默认访问修饰符，即为 private。


### C#数组


学习数组的创建。



```
using System;
namespace ArrayApplication
{
   class MyArray
   {
      static void Main(string[] args)
      {
         int []  n = new int[10]; /\* n 是一个带有 10 个整数的数组 \*/
         int i,j;


         /\* 初始化数组 n 中的元素 \*/        
         for ( i = 0; i < 10; i++ )
         {
            n[ i ] = i + 100;
         }

         /\* 输出每个数组元素的值 \*/
         for (j = 0; j < 10; j++ )
         {
            Console.WriteLine("Element[{0}] = {1}", j, n[j]);
         }
         Console.ReadKey();
      }
   }
}

```

### C#字符串


学习字符串的创建和连接。



```
using System;

namespace StringApplication
{
    class Program
    {
        static void Main(string[] args)
        {
           //字符串，字符串连接
            string fname, lname;
            fname = "Zhang";
            lname = "San";

            string fullname = fname + lname;
            Console.WriteLine("Full Name: {0}", fullname);

            //通过使用 string 构造函数
            char[] letters = { 'H', 'e', 'l', 'l','o' };
            string greetings = new string(letters);
            Console.WriteLine("Greetings: {0}", greetings);

            //方法返回字符串
            string[] sarray = { "Hello", "From", "Tutorials", "Point" };
            string message = String.Join(" ", sarray);
            Console.WriteLine("Message: {0}", message);

            //用于转化值的格式化方法
            DateTime waiting = new DateTime(2022, 10, 10, 17, 58, 1);
            string chat = String.Format("Message sent at {0:t} on {0:D}",
            waiting);
            Console.WriteLine("Message: {0}", chat);
            Console.ReadKey() ;
        }
    }
}

```

以上。





