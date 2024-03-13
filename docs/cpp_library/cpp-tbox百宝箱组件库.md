







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍cpp-tbox百宝箱组件库。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 安装运行](#blush2__28)
	+ [:satisfied:3. 进阶使用](#satisfied3__42)




### 😏1. 项目介绍


项目Github地址：`https://github.com/cpp-main/cpp-tbox`


cpp-tbox 是一个跨平台、轻量级的 C++ 工具库，旨在提供丰富的常用功能和便捷的编程接口。它的设计目标是简单易用、高效可靠，并且具有较低的资源消耗。


以下是 cpp-tbox 的一些特点和功能：



> 
> 1.跨平台支持：cpp-tbox 可以在多个主流操作系统上运行，包括 Windows、Linux、Mac等。它封装了操作系统相关的功能，提供了统一的接口，使得开发者可以在不同的平台下写出可移植的代码。
> 
> 
> 



> 
> 2.线程和并发支持：cpp-tbox 提供了线程管理、互斥锁、条件变量等基础的线程和并发功能。你可以使用 cpp-tbox 来创建和控制线程，进行线程同步和互斥操作，实现多线程并发编程。
> 
> 
> 



> 
> 3.内存管理：cpp-tbox 提供了内存管理的工具和接口，帮助你更方便地进行内存分配和释放。它封装了常用的内存操作，如动态内存分配、内存对齐、内存拷贝等，提供了安全和高效的内存管理功能。
> 
> 
> 



> 
> 4.常用数据结构和算法：cpp-tbox 实现了许多常用的数据结构和算法，如动态数组、链表、哈希表、堆等。它们被设计为高效和易用的，可以满足各种场景下的需求。
> 
> 
> 



> 
> 5.文件和IO操作：cpp-tbox 提供了便捷的文件和IO操作接口，使得文件处理和IO操作变得更简单。你可以使用 cpp-tbox 来读写文件、创建目录、遍历文件系统等。
> 
> 
> 



> 
> 6.字符串操作：cpp-tbox 封装了常见的字符串处理操作，如拆分、连接、格式化等。它还提供了正则表达式的支持，使得字符串匹配和替换更方便。
> 
> 
> 



> 
> 7.时间和日期处理：cpp-tbox 提供了时间和日期相关的功能，如获取当前时间、计算时间差、日期格式化等。它还支持时区转换，以及常见的时间和日期操作。
> 
> 
> 


### 😊2. 安装运行


编译运行：



```
cd cpp-tbox
# make编译
make 3rd-party modules RELEASE=1 STAGING\_DIR=$HOME/.tbox
# cmake编译
cmake -B build -DCMAKE\_INSTALL\_PREFIX=$HOME/.tbox
# 完成之后，头文件与库文件都在 $HOME/.tbox 路径下

```

另外，这个开源项目也配套了使用教程：`https://gitee.com/cpp-master/cpp-tbox-tutorials/blob/master/README.md`


我已经跟着走了一遍，感觉还是不错的，可以学到一些东西。


### 😆3. 进阶使用


一个基本的基于reactor事件的进程：



```
#include <tbox/main/main.h>

namespace app1 {

class App : public tbox::main::Module
{
  public:
    App(tbox::main::Context &ctx);
    ~App();

  protected:
    virtual bool onInit(const tbox::Json &cfg) override;
    virtual bool onStart() override;
    virtual void onStop() override;
    virtual void onCleanup() override;
};

}

```

Makefile示例：



```
TARGET:=demo

OBJECTS:=app_main.o

CXXFLAGS:=-I$(HOME)/.tbox/include -DLOG\_MODULE\_ID='"demo"'
LDFLAGS:=-L$(HOME)/.tbox/lib -rdynamic
LIBS:=\
	-ltbox\_main \
	-ltbox\_terminal \
	-ltbox\_network \
	-ltbox\_eventx \
	-ltbox\_event \
	-ltbox\_log \
	-ltbox\_util \
	-ltbox\_base \
	-lpthread -ldl

$(TARGET): $(OBJECTS)
	g++ -o $@ $^ $(LDFLAGS) $(LIBS)

clean:
	rm *.o

```

一个打印日志的示例：



```
#include <tbox/main/module.h>
#include <tbox/base/log.h>

#include <iostream>

class MyModule : public tbox::main::Module {
  public:
    explicit MyModule(tbox::main::Context &ctx) : tbox::main::Module("my", ctx) { }
    virtual ~MyModule() { }

  public:
    virtual bool onInit(const tbox::Json &js) override { 
      // 等级、时间点、线程号、模块名、函数名、内容、文件名：行号
      std::cout << "======= Start print log!" << std::endl;
      LogFatal("this is fatal log");
      LogErr("this is error log");
      LogWarn("this is warn log");
      LogInfo("this is info log");
      LogDbg("this is debug log");
      return true; 
    }
    virtual bool onStart() override { LogTag(); return true; }
    virtual void onStop() override { LogTag(); }
    virtual void onCleanup() override { LogTag(); }
};

namespace tbox {
namespace main {
void RegisterApps(Module &apps, Context &ctx) {
  apps.add(new MyModule(ctx));
  std::cout << "======= Start print log2!" << std::endl;
}
}
}

```

一个定时器事件：



```
#include <tbox/main/module.h>
#include <tbox/base/log.h>
#include <tbox/event/timer\_event.h> /// 导入TimerEvent类

class MyModule : public tbox::main::Module {
  public:
    explicit MyModule(tbox::main::Context &ctx)
        : tbox::main::Module("my", ctx)
        , tick\_timer\_(ctx.loop()->newTimerEvent())    /// 实例化定时器对象
    { }

    virtual ~MyModule() { delete tick_timer_; } /// 释放定时器对象

  public:
    virtual bool onInit(const tbox::Json &js) override {
        /// 初始化，设置定时间隔为1s，持续触发
        tick_timer_->initialize(std::chrono::seconds(1), tbox::event::Event::Mode::kPersist); // kPersist kOneshot
        /// 设置定时器触发时的回调
        tick_timer_->setCallback([this] { onTick(); });
        return true;
    }
    /// 启动
    virtual bool onStart() {
        tick_timer_->enable();
        return true;
    }
    /// 停止
    virtual void onStop() {
        tick_timer_->disable();
    }

  private:
    void onTick() {
        ++count_;
        LogDbg("count:%d", count_);
    }

  private:
    tbox::event::TimerEvent \*tick_timer_;
    int count_ = 0;
};

namespace tbox {
namespace main {
void RegisterApps(Module &apps, Context &ctx) {
  apps.add(new MyModule(ctx));
}
}
}

```

一个信号事件的处理：



```
#include <unistd.h>
#include <tbox/main/module.h>
#include <tbox/base/log.h>
#include <tbox/event/timer\_event.h>
#include <tbox/event/signal\_event.h> //! 导入SignalEvent

using namespace std::placeholders;

class MyModule : public tbox::main::Module {
  public:
    explicit MyModule(tbox::main::Context &ctx)
        : tbox::main::Module("my", ctx)
        , tick\_timer\_(ctx.loop()->newTimerEvent())
        , signal\_event\_(ctx.loop()->newSignalEvent())   //! 实例化SignalEvent对象
    { }

    virtual ~MyModule() {
        delete signal_event_;   //! 释放SignalEvent对象
        delete tick_timer_;
    }

  public:
    virtual bool onInit(const tbox::Json &js) override {
        tick_timer_->initialize(std::chrono::seconds(1), tbox::event::Event::Mode::kPersist);
        tick_timer_->setCallback([this] { onTick(); });

        //! 初始化SignalEvent，令其同时监听SIGUSR1,SIGUSR2信号，且是持续有效
        signal_event_->initialize({SIGUSR1, SIGUSR2}, tbox::event::Event::Mode::kPersist);
        //! 设置回调函数，均指向onSignalEvent()
        signal_event_->setCallback(std::bind(&MyModule::onSignalEvent, this, _1));
        return true;
    }
    virtual bool onStart() {
        tick_timer_->enable();
        signal_event_->enable(); //! 使能SignalEvent
        return true;
    }
    virtual void onStop() {
        signal_event_->disable();
        tick_timer_->disable();  //! 关闭SignalEvent
    }

  private:
    void onTick() {
        ++count_;
        LogDbg("count:%d", count_);
    }

    //! 信号事件时的处理
    void onSignalEvent(int signo) {
        LogInfo("got signal: %d", signo);
        if (signo == SIGUSR1)
            tick_timer_->disable(); //! 关闭定时器
        else if (signo == SIGUSR2)
            tick_timer_->enable();  //! 打开定时器
    }

  private:
    tbox::event::TimerEvent  \*tick_timer_;
    tbox::event::SignalEvent \*signal_event_;    //!< 定义SignalEvent对象
    int count_ = 0;
};

namespace tbox {
namespace main {
void RegisterApps(Module &apps, Context &ctx) {
  apps.add(new MyModule(ctx));
}
}
}

```

一个http服务示例：



```
#include <tbox/main/module.h>
#include <tbox/base/log.h>
#include <tbox/http/server/server.h> //! 导入Http服务

class MyModule : public tbox::main::Module {
  public:
    explicit MyModule(tbox::main::Context &ctx)
        : tbox::main::Module("my\_http", ctx)
        , http\_server\_(ctx.loop())  //! 实例化Http服务对象
    { }

  public:
    virtual bool onInit(const tbox::Json &js) override {
        //! 初始化Http服务，绑定 0.0.0.0:12345 地址，backlog为2
        if (!http_server_.initialize(tbox::network::SockAddr::FromString("0.0.0.0:12345"), 2))
            return false;

        //! 指定Http服务的请求的处理方法
        http_server_.use(
            [] (tbox::http::server::ContextSptr http_ctx,
                const tbox::http::server::NextFunc &next_func) {
                //! 不管请求的是什么，直接指定回复的body内容
                http_ctx->res().body = R"(<p>Welcome to CppTbox.</br>This is http example.</p>)";
            }
        );

        return true;
    }
    virtual bool onStart() override {
        http_server_.start();   //! 启动Http服务
        return true;
    }
    virtual void onStop() override {
        http_server_.stop();    //! 停止Http服务
    }

  private:
    tbox::http::server::Server http_server_;    //! 定义Http服务对象
};

namespace tbox {
namespace main {
void RegisterApps(Module &apps, Context &ctx) {
  apps.add(new MyModule(ctx));
}
}
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





