### 😏1. 并行编程OpenMP介绍

`OpenMP`是一种用于并行编程的开放标准，它旨在简化共享内存多线程编程的开发过程。OpenMP提供了一组指令和库例程，可以将顺序程序转换为可并行执行的代码。

`OpenMP`的核心思想是使用指令来标识出需要并行执行的代码块，并指定如何将工作划分到不同的线程中。开发人员可以在现有的顺序代码中插入特定的指令，以实现并行化。

以下是`OpenMP`的一些主要特性：

1.指令注释：通过在代码中插入特定的预处理指令，开发人员可以标识出应该并行执行的代码块。例如，可以使用`#pragma omp parallel`指令来创建一个并行区域。

2.线程创建与同步：OpenMP自动管理线程的创建和同步。在进入并行区域时，OpenMP会动态地创建一组线程，并在退出并行区域时进行同步。开发人员无需手动管理线程的创建和销毁。

3.工作分配：OpenMP提供了多种方式来将工作划分到不同的线程中。例如，可以使用`#pragma omp for`指令将循环迭代并行化，让不同线程处理不同的迭代。

4.共享内存模型：OpenMP使用共享内存模型，允许多个线程之间共享数据。开发人员可以使用shared关键字将变量声明为共享变量，以便多个线程可以访问和修改它们。

5.线程私有变量：除了共享变量外，OpenMP还支持线程私有变量。开发人员可以使用private关键字将变量声明为线程私有，确保每个线程都有自己的副本。

OpenMP广泛用于各种领域的并行编程，包括科学计算、图形处理、机器学习等。它提供了一种相对简单且易于使用的方法来利用多核处理器的计算能力，加速程序的执行。


### 😊2. openmp并行处理for循环

openmp常用来对代码中的for循环进行并行处理优化：

一个例子如下：

```cpp
// main.cpp
// 使用并行循环进行向量加法
#include <stdio.h>
#include <omp.h>

#define SIZE 10000

int main() {
    int i;
    int a[SIZE], b[SIZE], c[SIZE];

    // 初始化数组
    for (i = 0; i < SIZE; i++) {
        a[i] = i;
        b[i] = i;
    }

    #pragma omp parallel for
    for (i = 0; i < SIZE; i++) {
        c[i] = a[i] + b[i];
    }

    // 打印结果
    for (i = 0; i < SIZE; i++) {
        printf("%d ", c[i]);
    }
    printf("\n");

    return 0;
}

```

例程中使用`#pragma omp parallel for`指令来并行化for循环。这个指令告诉编译器将循环分割成多个任务，并由多个线程同时执行。每个线程负责处理循环的一个子集。


编译时启用OpenMP支持，`g++ main.cpp -fopenmp`


这样程序就可以并发执行，提高运算效率了。


### 😆3. openmp多线程执行效率对比


openmp可以对一段程序指定不同线程数来优化，下面是一个示例：

```cpp
#include <iostream>
#include <omp.h>
 
using namespace std;
 
void test()
{
	for (int i = 0; i < 50000; i++)
	{
	}
}
 
int main()
{
	float startTime = omp_get_wtime();
 
	//指定2个线程
#pragma omp parallel for num_threads(2)
	for (int i = 0; i < 50000; i++)
	{
		test();
	}
	float endTime = omp_get_wtime();
	printf("指定 2 个线程，执行时间: %f\n", endTime - startTime);
	startTime = endTime;
 
	//指定4个线程
#pragma omp parallel for num_threads(4)
	for (int i = 0; i < 50000; i++)
	{
		test();
	}
	endTime = omp_get_wtime();
	printf("指定 4 个线程，执行时间: %f\n", endTime - startTime);
	startTime = endTime;
 
	//指定8个线程
#pragma omp parallel for num_threads(8)
	for (int i = 0; i < 50000; i++)
	{
		test();
	}
	endTime = omp_get_wtime();
	printf("指定 8 个线程，执行时间: %f\n", endTime - startTime);
	startTime = endTime;
 
	//指定12个线程
#pragma omp parallel for num_threads(12)
	for (int i = 0; i < 50000; i++)
	{
		test();
	}
	endTime = omp_get_wtime();
	printf("指定 12 个线程，执行时间: %f\n", endTime - startTime);
	startTime = endTime;
 
	//不使用OpenMP
	for (int i = 0; i < 50000; i++)
	{
		test();
	}
	endTime = omp_get_wtime();
	printf("不使用OpenMP多线程，执行时间: %f\n", endTime - startTime);
	startTime = endTime;
 
	return 0;
}


```

结果如下：

```bash
指定 2 个线程，执行时间: 2.098389
指定 4 个线程，执行时间: 1.102295
指定 8 个线程，执行时间: 0.564941
指定 12 个线程，执行时间: 0.375244
不使用OpenMP多线程，执行时间: 3.956543

```

例程中使用`#pragma omp parallel for num_threads(12)`来对程序指定线程数，对这种运算次数多的情况下，提高openmp方法可压缩执行时间到1/4左右，但不能简单通过提高线程数来提高效率。

### 😏1. 多线程介绍

每一个进程（可执行程序）都有一个主线程，这个主线程是唯一的，自动创建的，即：一个进程中只有一个主线程，自己创建的线程一般称为子线程。

传统的C++没有引入线程概念，C++11标准提供了语言层面上的多线程，包含在头文件中。它解决了跨平台的问题，提供了管理线程、保护共享数据、线程间同步操作、原子操作等类。C++11 新标准中引入了5个头文件来支持多线程编程：

```cpp
- thread：线程相关
- mutex：与互斥量相关的类，如加锁与解锁
- atomic
- condition_variable
- future

```

### 😊2. 多线程操作

`join()`：等待或者阻塞，阻塞主线程的执行，直到子线程调用结束，然后子线程与主线程汇合，继续向下走。

`detach()`：分离，启动的线程自主在后台运行，当前的代码继续往下执行，不等待新线程结束。一旦调用了detach()，就不能再用join()，否则系统会报错。这是由于detach()之后，两条线程的执行速度不一致导致的。

`joinable()`：判断是否可以成功使用join()或detach()的。

```cpp
if (myThread.joinable()) foo.join();

```

`lock()`：使用互斥量进行共享内存保护的时候，一般情况是在所需要进行保护的代码段进行lock()操作，只有lock()成功时，代码才能继续执行，否则就会一直lock()不在向下执行，直到lock()成功。

`unlock()`：解锁资源

```cpp
// 一个mutex变量控制同一个资源，因此会先打印完*再打印$
// 两个mutex变量则可能出现交替打印，因为不是修改统一资源
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;  // mutex for critical section
void print_block (int n, char c) 
{
    mtx.lock();
    for (int i=0; i<10; i++)
    {
       std::cout << c; 
    }
    std::cout << '\n';
    mtx.unlock();
}
int main ()
{
    std::thread th1 (print_block,50,'*');//线程1：打印*
    std::thread th2 (print_block,50,'$');//线程2：打印$

    th1.join();
    th2.join();
    return 0;
}

```

`trylock()`：查看是否上锁


`std::lock_guard()`：即加锁，作用域结束后自动解锁，直接取代lock()与unlock()，用了lock_guard()之后，就不能在使用lock()与unlock()；创建lock_guard对象时，它将尝试获取提供给它的互斥锁的所有权。当控制流离开lock_guard对象的作用域时，lock_guard析构并释放互斥量。


`unique_lock`：是 lock_guard 的升级加强版，它具有 lock_guard 的所有功能，同时又具有其他很多方法，使用起来更加灵活方便，能够应对更复杂的锁定需要。


`死锁`：是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程称为死锁进程。


`condition_variable`


### 😆3. 多线程示例

Windows编写多线程C++程序需要包含头文件：`#include <thread>`

多线程实例：

```cpp
#include <iostream>
#include <thread>

using namespace std;
void print();

void print()
{
	cout << "-----------thread test-------------" << endl;
}

class test {
public:
	// operator()() - 重载运算符+参数传递
	void operator()() {
		cout << "class subthread starting!!!" << endl;
		cout << "class subthread over!!!" << endl;
	}
	void operator()(int x,int y) {
		cout << "x+y=" << x+y << endl;
	}
};

int main()
{
	cout << "主线程id:" << this_thread::get_id() << endl;

	// 线程休眠 - 不同的时间表示
	std::this_thread::sleep_for(std::chrono::seconds(1));   //1秒 = 1000毫秒=10^6微秒
	cout << "1s\n";
#if 0
	std::this_thread::sleep_for(std::chrono::microseconds(2 * 1000000));  //微秒
	cout << "2s\n";
	std::this_thread::sleep_for(std::chrono::milliseconds(3000));  //毫秒
	cout << "3s\n";
	std::this_thread::sleep_for(std::chrono::minutes(1));
	cout << "1min\n";
	std::this_thread::sleep_for(std::chrono::hours(1));
	cout << "1hour\n";
#endif

	// 创建子线程
	cout << "\n子线程1!!!" << endl;
	thread v1(print);	//入口函数print
	v1.join();	//加入主线程
	// 是否可joinable()
	if (v1.joinable()) {
		cout << "可以调用join()或者detach()" << endl;
	}
	else {
		cout << "不可以调用join()或者detach()" << endl;
	}

	cout << "\n子线程2!!!" << endl;
	test v2;
	thread t2(v2);
	t2.join();

	cout << "\n子线程3!!!" << endl;
	test v3;
	thread t3(v3,2,4);
	t3.join();

	return 0;
}

```

以上。





