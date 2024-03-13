







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ThreadPoll线程池实现。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 线程池介绍](#smirk1__7)
	+ [:blush:2. 线程池实现1-单头文件](#blush2_1_29)
	+ [:satisfied:3. 线程池实现2-较复杂](#satisfied3_2_192)




### 😏1. 线程池介绍


线程池是一种线程管理的抽象概念，它主要用于**优化多线程应用程序的性能和资源利用**。在多线程编程中，创建和销毁线程是一个开销较大的操作。线程池通过预先创建一组线程，并将任务提交给这些线程来执行，从而避免了重复创建和销毁线程的开销。


线程池通常由以下几个组件组成：



> 
> 1.任务队列（Task Queue）：用于存储待执行的任务。当任务提交到线程池时，它们被放置在任务队列中等待执行。
> 
> 
> 2.线程池管理器（Thread Pool Manager）：负责创建、管理和调度线程池中的线程。它控制着线程的数量，可以动态地增加或减少线程的数量，以适应不同的工作负载。
> 
> 
> 3.工作线程（Worker Threads）：线程池中的实际执行单元。它们不断地从任务队列中获取任务并执行。
> 
> 
> 4.任务接口（Task Interface）：定义了要执行的任务的接口。通常，任务是以函数或可运行对象的形式表示。
> 
> 
> 


使用线程池的好处包括：



> 
> 提高性能：线程池可以减少线程的创建和销毁次数，避免了频繁的上下文切换，提高了多线程程序的性能和响应速度。
> 
> 
> 资源管理：线程池可以限制并发线程的数量，避免资源过度占用，从而更好地管理系统资源。
> 
> 
> 提高可扩展性：通过调整线程池的大小，可以适应不同的并发需求，提高系统的可扩展性。
> 
> 
> 


综上，线程池是一种重要的多线程编程技术，它能够有效地管理和利用线程，提高程序的性能和资源利用率。


### 😊2. 线程池实现1-单头文件


Github项目：`https://github.com/progschj/ThreadPool`


threadpoll.h



```
#ifndef THREAD\_POOL\_H
#define THREAD\_POOL\_H

#include <vector>
#include <queue>
#include <memory>
#include <thread>
#include <mutex>
#include <condition\_variable>
#include <future>
#include <functional>
#include <stdexcept>

class ThreadPool {
public:
    ThreadPool(size_t);
    // template enqueue: F & Args
    template<class F, class... Args>
    auto enqueue(F&& f, Args&&... args) // parameter list
        -> std::future<typename std::result_of<F(Args...)>::type>;  // return type
    ~ThreadPool();
private:
    // need to keep track of threads so we can join them
    std::vector< std::thread > workers;
    // the task queue
    std::queue< std::function<void()> > tasks;
    
    // synchronization: wait and wakeup
    std::mutex queue_mutex;
    std::condition_variable condition;
    bool stop;
};
 
// the constructor just launches some amount of workers
inline ThreadPool::ThreadPool(size_t threads)
    :   stop(false)
{
    for (size_t i = 0; i < threads; ++i)
        workers.emplace\_back(
            [this]
            {
                for(;;)
                {
                    // function template define
                    std::function<void()> task;
                    {
                        std::unique_lock<std::mutex> lock(this->queue_mutex);
                        // wait
                        this->condition.wait(lock,
                            [this]{ return this->stop || !this->tasks.empty(); });
                        if(this->stop && this->tasks.empty())
                            return;
                        // add and pop
                        task = std::move(this->tasks.front());
                        this->tasks.pop();
                    }

                    task();
                }
            }
    );
}

// add new work item to the pool
template<class F, class... Args>
auto ThreadPool::enqueue(F&& f, Args&&... args) 
    -> std::future<typename std::result_of<F(Args...)>::type>
{
    using return_type = typename std::result_of<F(Args...)>::type;

    auto task = std::make\_shared< std::packaged\_task<return\_type()> >(
            std::bind(std::forward<F>(f), std::forward<Args>(args)...)
        );
        
    std::future<return_type> res = task->get\_future();
    {
        std::unique_lock<std::mutex> lock(queue_mutex);

        // don't allow enqueueing after stopping the pool
        if(stop)
            throw std::runtime\_error("enqueue on stopped ThreadPool");

        tasks.emplace([task](){ (\*task)(); });
    }
    // wake up
    condition.notify\_one();
    return res;
}

// the destructor joins all threads
inline ThreadPool::~ThreadPool()
{
    {
        std::unique_lock<std::mutex> lock(queue_mutex);
        stop = true;
    }
    // wake up
    condition.notify\_all();
    for(std::thread &worker: workers)
        worker.join();
}

#endif

```

main.cpp



```
#include <iostream>
#include <vector>
#include <chrono>

#include "threadpoll.h"

int m\_sum(int x, int y) {
    return x + y;
}

int main()
{
    // 创建容量为5的线程池
    ThreadPool pool(5);

    // 简单使用
    auto result_simple = pool.enqueue(m_sum, 3, 5);
    std::cout << "result\_simple: " << result_simple.get() << std::endl;

    // 复杂使用
    std::vector< std::future<int> > results;

    for(int i = 0; i < 8; ++i) {
        results.emplace\_back(
            pool.enqueue([i] {
                std::cout << "hello " << i << std::endl;
                // std::this\_thread::sleep\_for(std::chrono::seconds(1));
                std::cout << "world " << i << std::endl;
                
                return i\*i;
            })
        );
    }

    std::this_thread::sleep\_for(std::chrono::seconds(2));
    std::cout << "result: ";
    for(auto && result: results)
        std::cout << result.get() << ' ';
    std::cout << std::endl;
    
    return 0;
}

```

编译运行：



```
g++ -o main main.cpp -lpthread && ./main

```

**推荐使用这一种**。使用上在原项目基础上进行了扩充，通过使用线程池，可以很方便地对线程进行操作，且不用考虑多任务的冲突等。


### 😆3. 线程池实现2-较复杂


Github项目：`https://github.com/volute24/ThreadPoll`



```
// main.cpp
#include "threadpool.h"
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>

#define THREADPOOL\_MAX\_NUM 5

void\* mytask(void \*arg)
{
    printf("thread %d is working on task %d\n", (int)pthread\_self(), \*(int\*)arg);
    sleep(1);
    free(arg);
    return NULL;
}

int main(int argc, char\* argv[])
{
    threadpool_t pool;
    // 初始化线程池，最多5个线程
    threadpool\_init(&pool, THREADPOOL_MAX_NUM);
	// 创建10个任务
    for(int i=0; i < 10; i++)
    {
        int \*arg =(int \*)malloc(sizeof(int));
        \*arg = i;
        threadpool\_add\_task(&pool, mytask, arg);
        //printf("arg address:%p,arg:[%d],i:[%d]\n",arg,\*arg,i);
    }
    threadpool\_destroy(&pool);
    return 0;
}

```


```
// threadpool.h
#ifndef \_THREAD\_POLL\_H\_
#define \_THREAD\_POLL\_H\_

#include "condition.h"

// 封装线程池中的对象需要执行的任务对象
typedef struct task
{
	void \*(\*run)(void \*args);//函数指针，需要执行的任务
	void \*arg;			//参数
	struct task \*next;	//任务队列中下一个任务
}task_t;

//定义线程池结构体
typedef struct threadpool
{
	condition_t ready;   //状态量
	task_t \*first;	     //任务队列中第一个任务
	task_t \*last;		//任务队列中最后一个任务
	int counter;		//线程池中已有线程数
	int idle;			//线程池中空闲线程数
	int max_threads;	//线程池最大线程数
	int quit;			//是否退出标志
}threadpool_t;

//线程池初始化
void threadpool\_init(threadpool_t \*pool,int threads);
//线程池中加入任务
void threadpool\_add\_task(threadpool_t \*pool,void \*(\*run)(void \*args),void \*arg);
//销毁线程池
void threadpool\_destroy(threadpool_t \*pool);

#endif

```


```
// threadpoll.cpp
#include "threadpool.h"
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <time.h>
#include <pthread.h>

using namespace std;

//线程执行
void \*thread\_routine(void \*arg)
{
	struct timespec abstime;
	int timeout;
	printf("thread %d is starting\n",(int)pthread\_self());
	threadpool_t \*pool = (threadpool_t \*)arg;

	while(1)
	{
		timeout = 0;
		//访问池前加锁
		condition\_lock(&pool->ready);
		//空闲
		pool->idle++;
		//等待队列任务|| 收到线程池销毁通知
		while(pool->first == NULL && !pool->quit)
		{
			//否则线程阻塞等待
			printf("thread %d is waiting\n",(int)pthread\_self());
			//获取从当前时间加上等待时间，设置超时睡眠时间
			//clock\_gettime 在编译链接时需加上 -lrt ,librt中实现了clock\_gettime函数
			clock\_gettime(CLOCK_REALTIME,&abstime);  //CLOCK\_REALTIME 系统实时时间
			abstime.tv_sec += 2;
			int status;
			status = condition\_timedwait(&pool->ready,&abstime);
			if(status == ETIMEDOUT)
			{
				printf("thread %d wait timed out\n",(int)pthread\_self());
				timeout = -1;
				break;
			}
		}

		pool->idle--;
		
		if(pool->first != NULL)
		{
		 	//取出等待队列最前任务，移除任务，并执行任务
		 	task_t \*t = pool->first;
			pool->first = t->next;
			//由于任务执行需要消耗时间，先解锁让其他线程访问线程池
			condition\_unlock(&pool->ready);
			//执行任务
			t->run(t->arg);
			//执行完任务释放内存
            free(t);
           	//重新加锁
            condition\_lock(&pool->ready);	
		}
		//退出线程池
	        if(pool->quit && pool->first == NULL)
	        {
	            pool->counter--;
	            //若线程池中没有线程，通知等待线程（主线程）全部任务已经完成
	            if(pool->counter == 0)
	            {
	                condition\_signal(&pool->ready);
	            }
	            condition\_unlock(&pool->ready);
	            break;
	        }
		 	//超时，跳出销毁线程
	        if(timeout == 1)
	        {
	            pool->counter--;
	            condition\_unlock(&pool->ready);
	            break;
	        }
			condition\_unlock(&pool->ready);
	}
    printf("thread %d is exiting\n", (int)pthread\_self());
    return NULL;
}

//线程池初始化
void threadpool\_init(threadpool_t \*pool, int threads)
{
    
    int nstatu = condition\_init(&pool-ready>);
    printf("Init return values:%d\n",nstatu);
    pool->first = NULL;
    pool->last =NULL;
    pool->counter =0;
    pool->idle =0;
    pool->max_threads = threads;
    pool->quit =0;
    
}

//增加一个任务到线程池
void threadpool\_add\_task(threadpool_t \*pool, void \*(\*run)(void \*arg), void \*arg)
{
    //产生一个新的任务
    task_t \*newtask = (task_t \*)malloc(sizeof(task_t));
    newtask->run = run;
    newtask->arg = arg;
    newtask->next=NULL;//新加的任务放在队列尾端
    
    //线程池的状态被多个线程共享，操作前需要加锁
    condition\_lock(&pool->ready);
    
    if(pool->first == NULL)
    {
        pool->first = newtask;
    }        
    else    
    {
        pool->last->next = newtask;
    }
    pool->last = newtask;  //队列尾指向新加入的线程
    
    //线程池中有线程空闲，唤醒
    if(pool->idle > 0)
    {
        condition\_signal(&pool->ready);
    }
    //当前线程池中线程个数没有达到设定的最大值，创建一个新的线程
    else if(pool->counter < pool->max_threads)
    {
        pthread_t tid;
        pthread\_create(&tid, NULL, thread_routine,pool);
        pool->counter++;
    }
    //结束，访问解锁
    condition\_unlock(&pool->ready);
}

//线程池销毁
void threadpool\_destroy(threadpool_t \*pool)
{
    if(pool->quit)
    {
    return;
    }
    condition\_lock(&pool->ready);
    pool->quit = 1;
    //线程池中线程个数大于0
    if(pool->counter > 0)
    {
        //对于等待的线程，发送信号唤醒
        if(pool->idle > 0)
        {
            condition\_broadcast(&pool->ready);
        }
        //正在执行任务的线程，等待他们结束任务
        while(pool->counter)
        {
            condition\_wait(&pool->ready);
        }
    }
    condition\_unlock(&pool->ready);
    condition\_destroy(&pool->ready);
}

```

。。。


编译：`g++ main.cpp condition.cpp threadpool.cpp -lpthread`


运行如下：



```
Init return values:0
thread 1696954112 is starting
thread 1705346816 is starting
thread 1688561408 is starting
thread 1688561408 is working on task 0
thread 1671776000 is starting
thread 1671776000 is working on task 2
thread 1705346816 is working on task 1
thread 1696954112 is working on task 3
thread 1680168704 is starting
thread 1680168704 is working on task 4
thread 1688561408 is working on task 5
thread 1671776000 is working on task 6
thread 1705346816 is working on task 7
thread 1680168704 is working on task 8
thread 1696954112 is working on task 9
thread 1680168704 is exiting
thread 1705346816 is exiting
thread 1696954112 is exiting

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





