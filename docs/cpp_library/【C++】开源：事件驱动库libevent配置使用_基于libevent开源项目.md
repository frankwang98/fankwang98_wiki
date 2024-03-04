







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍事件驱动库libevent配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__45)




### 😏1. 项目介绍


项目Github地址：`https://github.com/libevent/libevent`


官网：`https://libevent.org/`


libevent是一个开源的事件驱动库，用于开发高性能、可扩展的网络应用程序。它提供了跨平台的抽象接口，使开发人员能够使用事件回调来管理网络连接、定时器和信号等事件。


以下是libevent库的一些主要特点和功能：



> 
> 1.事件驱动：libevent使用事件驱动的方式处理网络和I/O操作。它基于回调机制，可以处理各种事件，包括网络连接、读写操作、定时器等。
> 
> 
> 



> 
> 2.跨平台支持：libevent可以在多个平台上运行，包括Linux、Unix、Windows等。它封装了不同操作系统的底层API，使开发人员能够在不同平台上实现相同的功能。
> 
> 
> 



> 
> 3.高性能：libevent被设计成高效的事件通知引擎，它使用了高效的I/O多路复用技术（如epoll、kqueue等），能够同时处理大量的并发连接和事件。
> 
> 
> 



> 
> 4.可扩展性：libevent提供了可扩展的接口和机制，开发人员可以自定义事件的处理方式，并添加自定义的事件类型。它还支持多线程和多进程编程模型，方便实现并发处理。
> 
> 
> 



> 
> 5.支持多种协议：libevent支持多种网络协议，包括TCP、UDP、SSL等。它提供了相应的API和功能，以便开发人员轻松地构建各种网络应用程序。
> 
> 
> 


libevent 是一个功能强大的事件驱动网络库，广泛应用于构建高性能的服务器程序、代理、负载均衡器等网络应用。它提供了简洁的接口和丰富的功能，使开发人员能够轻松地编写高效、可扩展的网络应用程序。


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libevent-dev
# 查看版本（ubuntu默认2.1.8-stable）
pkg-config --modversion libevent

```


```
# 源码安装
tar -zxvf libevent-2.0.22-stable.tar.gz
cd libevent-2.0.22-stable/
./configure
make
sudo make install

```

### 😆3. 使用说明


定时器事件示例：



```
#include <iostream>
#include <event2/event.h>

void timerCallback(evutil_socket_t fd, short events, void\* arg) {
    std::cout << "Timer expired" << std::endl;

    // 停止事件循环
    event_base\* base = static\_cast<event\_base\*>(arg);
    event\_base\_loopbreak(base);
}

int main() {
    // 创建事件基础
    event_base\* base = event\_base\_new();
    if (!base) {
        std::cerr << "Failed to create event base" << std::endl;
        return 1;
    }
    std::cout << "start event..." << std::endl;

    // 创建定时器事件
    timeval tv = {2, 0};  // 2 秒
    event\* timerEvent = evtimer\_new(base, timerCallback, base);
    if (!timerEvent) {
        std::cerr << "Failed to create timer event" << std::endl;
        return 1;
    }

    // 添加定时器事件
    if (evtimer\_add(timerEvent, &tv) < 0) {
        std::cerr << "Failed to add timer event" << std::endl;
        return 1;
    }

    // 启动事件循环
    event\_base\_dispatch(base);

    // 清理资源
    event\_free(timerEvent);
    event\_base\_free(base);

    return 0;
}

```

编译运行：



```
g++ -o main main.cpp -levent
./main

```

基于libevent的线程池示例：



```
#include <event2/event.h>
#include <event2/thread.h>
#include <iostream>
#include <vector>
#include <queue>
#include <mutex>
#include <condition\_variable>
#include <functional>
#include <thread>

// 任务结构体
struct Task {
    std::function<void()> function;

    Task(const std::function<void()>& f) : function(f) {}
};

// 线程池类
class ThreadPool {
public:
    ThreadPool(int numThreads) : stop(false) {
        // 初始化libevent线程支持
        evthread\_use\_pthreads();

        for (int i = 0; i < numThreads; ++i) {
            threads.emplace\_back([this] {
                event_base\* base = event\_base\_new();
                if (!base) {
                    std::cerr << "Failed to create event base." << std::endl;
                    return;
                }

                // 创建事件来触发任务执行
                event\* ev = event\_new(base, -1, EV_PERSIST, [](evutil_socket_t fd, short events, void\* arg) {
                    ThreadPool\* threadPool = static\_cast<ThreadPool\*>(arg);
                    threadPool->executeTask();
                }, this);

                timeval delay = { 0, 1000 }; // 每隔1毫秒触发一次事件
                event\_add(ev, &delay);

                // 执行事件循环
                event\_base\_dispatch(base);

                event\_free(ev);
                event\_base\_free(base);
            });
        }
    }

    ~ThreadPool() {
        {
            std::unique_lock<std::mutex> lock(taskMutex);
            stop = true;
        }

        taskCondition.notify\_all();

        for (auto& thread : threads) {
            thread.join();
        }
    }

    template<class F>
    void enqueue(F&& f) {
        {
            std::unique_lock<std::mutex> lock(taskMutex);
            tasks.emplace(new Task(std::forward<F>(f)));
        }

        taskCondition.notify\_one();
    }

private:
    std::vector<std::thread> threads;
    std::queue<Task\*> tasks;

    std::mutex taskMutex;
    std::condition_variable taskCondition;

    bool stop;

    void executeTask() {
        Task\* task = nullptr;

        {
            std::unique_lock<std::mutex> lock(taskMutex);

            if (tasks.empty()) {
                return;
            }

            task = tasks.front();
            tasks.pop();
        }

        task->function();

        delete task;
    }
};

// 示例使用
void taskFunction(int id) {
    std::cout << "Task " << id << " is being executed." << std::endl;
}

int main() {
    const int numThreads = 4;
    ThreadPool threadPool(numThreads);
    std::cout << "Create " << numThreads << " pools..." << std::endl;

    // 提交任务到线程池
    for (int i = 0; i < 10; ++i) {
        threadPool.enqueue([i] {
            taskFunction(i);
        });
    }

    // 等待所有任务完成
    std::this_thread::sleep\_for(std::chrono::seconds(3));
    std::cout << "Task end!!!" << std::endl;

    return 0;
}


```

编译运行：



```
g++ -o main main.cpp -levent -lpthread -levent\_pthreads

```

TCP服务端和客户端示例：



```
// server.cpp
#include <iostream>
#include <cstring>
#include <event2/event.h>
#include <event2/listener.h>
#include <event2/bufferevent.h>

void readCallback(bufferevent\* bev, void\* arg) {
    char data[1024];
    int n;
    while ((n = bufferevent\_read(bev, data, sizeof(data))) > 0) {
        std::cout << "Received message: " << std::string(data, n);
        // 在这里可以处理接收到的消息
    }
}

void writeCallback(bufferevent\* bev, void\* arg) {
    // 在这里可以处理发送完成的逻辑
}

void eventCallback(bufferevent\* bev, short events, void\* arg) {
    if (events & BEV_EVENT_ERROR) {
        std::cerr << "Error occurred" << std::endl;
    }
    if (events & (BEV_EVENT_EOF | BEV_EVENT_ERROR)) {
        std::cout << "Connection closed" << std::endl;
        bufferevent\_free(bev);
    }
}

void acceptCallback(evconnlistener\* listener, evutil_socket_t fd, sockaddr\* address, int socklen, void\* arg) {
    std::cout << "Accepted connection" << std::endl;

    // 创建 bufferevent 对象
    event_base\* base = evconnlistener\_get\_base(listener);
    bufferevent\* bev = bufferevent\_socket\_new(base, fd, BEV_OPT_CLOSE_ON_FREE);
    if (!bev) {
        std::cerr << "Failed to create bufferevent" << std::endl;
        evutil\_closesocket(fd);
        return;
    }

    // 设置回调函数
    bufferevent\_setcb(bev, readCallback, writeCallback, eventCallback, nullptr);

    // 启用读写事件
    bufferevent\_enable(bev, EV_READ | EV_WRITE);
}

int main() {
    // 创建事件基础
    event_base\* base = event\_base\_new();
    if (!base) {
        std::cerr << "Failed to create event base" << std::endl;
        return 1;
    }

    // 创建监听器
    sockaddr_in sin;
    std::memset(&sin, 0, sizeof(sin));
    sin.sin_family = AF_INET;
    sin.sin_addr.s_addr = htonl(INADDR_ANY);
    sin.sin_port = htons(12345);

    evconnlistener\* listener = evconnlistener\_new\_bind(
        base,
        acceptCallback,
        nullptr,
        LEV_OPT_REUSEABLE | LEV_OPT_CLOSE_ON_FREE,
        -1,
        (struct sockaddr\*)&sin,
        sizeof(sin)
    );
    if (!listener) {
        std::cerr << "Failed to create listener" << std::endl;
        return 1;
    }

    std::cout << "Server is running..." << std::endl;

    // 启动事件循环
    event\_base\_dispatch(base);

    // 清理资源
    evconnlistener\_free(listener);
    event\_base\_free(base);

    return 0;
}

```


```
// client.cpp
#include <iostream>
#include <cstring>
#include <arpa/inet.h>
#include <event2/event.h>
#include <event2/bufferevent.h>

void readCallback(bufferevent\* bev, void\* arg) {
    char data[1024];
    int n;
    while ((n = bufferevent\_read(bev, data, sizeof(data))) > 0) {
        std::cout << "Received message: " << std::string(data, n);
        // 在这里可以处理接收到的消息
    }
}

void writeCallback(bufferevent\* bev, void\* arg) {
    // 在这里可以处理发送完成的逻辑
}

void eventCallback(bufferevent\* bev, short events, void\* arg) {
    if (events & BEV_EVENT_CONNECTED) {
        std::cout << "Connected to server" << std::endl;
        // 在这里可以处理连接成功的逻辑
    }
    else if (events & BEV_EVENT_ERROR) {
        std::cerr << "Error occurred" << std::endl;
    }
    else if (events & (BEV_EVENT_EOF | BEV_EVENT_ERROR)) {
        std::cout << "Connection closed" << std::endl;
        bufferevent\_free(bev);
    }
}

int main() {
    // 创建事件基础
    event_base\* base = event\_base\_new();
    if (!base) {
        std::cerr << "Failed to create event base" << std::endl;
        return 1;
    }

    // 创建 bufferevent 对象
    bufferevent\* bev = bufferevent\_socket\_new(base, -1, BEV_OPT_CLOSE_ON_FREE);
    if (!bev) {
        std::cerr << "Failed to create bufferevent" << std::endl;
        event\_base\_free(base);
        return 1;
    }

    // 设置回调函数
    bufferevent\_setcb(bev, readCallback, writeCallback, eventCallback, nullptr);

    // 连接到服务器
    sockaddr_in sin;
    std::memset(&sin, 0, sizeof(sin));
    sin.sin_family = AF_INET;
    sin.sin_addr.s_addr = inet\_addr("127.0.0.1");
    sin.sin_port = htons(12345);

    if (bufferevent\_socket\_connect(bev, (struct sockaddr\*)&sin, sizeof(sin)) < 0) {
        std::cerr << "Failed to connect to server" << std::endl;
        bufferevent\_free(bev);
        event\_base\_free(base);
        return 1;
    }

    // 启动事件循环
    event\_base\_dispatch(base);

    // 清理资源
    bufferevent\_free(bev);
    event\_base\_free(base);

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





