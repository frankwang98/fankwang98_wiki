







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ncurses终端文本界面库。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__40)




### 😏1. 项目介绍


项目Github地址：`https://github.com/mirror/ncurses`


`ncurses`是一个文本模式用户界面（`TUI`）的库，它提供了一套函数和工具，用于处理终端的输入和输出，以创建交互式的、基于文本的应用程序。它是使用C语言编写的，并且被广泛用于Unix-like系统中。


下面是一些关于ncurses库的特点和功能：



> 
> 1.文本模式用户界面：ncurses专注于创建文本模式下的用户界面，而不是图形界面。它可以在终端中创建窗口、标签、按钮等元素。
> 
> 
> 



> 
> 2.终端独立性：ncurses可以在不同的终端类型上运行，因为它使用了终端数据库（terminfo）来处理不同终端的差异性。这意味着编写的代码可以在各种终端上保持一致运行。
> 
> 
> 



> 
> 3.屏幕刷新控制：ncurses提供了一系列函数来控制屏幕的刷新，包括清除屏幕、移动光标、刷新显示等，从而实现对界面的实时更新。
> 
> 
> 



> 
> 4.键盘和鼠标输入处理：ncurses可以捕获键盘和鼠标输入，并提供函数来处理用户输入，例如响应按键、鼠标点击等。
> 
> 
> 



> 
> 5.颜色和图形处理：ncurses支持在文本模式下使用颜色，可以设置文本的前景色和背景色，以及终端的颜色属性。
> 
> 
> 



> 
> 6.多窗口管理：ncurses允许创建多个窗口，并提供了函数来管理这些窗口，包括创建、删除、移动、重绘等操作。
> 
> 
> 



> 
> 7.动态界面更新：ncurses可以实现动态更新界面，通过重绘窗口或部分内容，可以实现实时显示信息。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
sudo apt-get install libncurses5-dev

```

编译运行：



```
g++ main.cpp -o main -lncurses

```

### 😆3. 使用说明


下面进行使用分析：


创建窗口示例：



```
#include <iostream>
#include <ncurses.h>

int main() {
    // 初始化ncurses
    initscr();
    
    // 创建一个新窗口
    WINDOW\* window = newwin(10, 30, 0, 0);

    while (true) {
        // 在窗口中显示文本
        mvwprintw(window, 1, 1, "Hello, ncurses!");

        // 刷新窗口显示
        wrefresh(window);
    }
    
    // 获取用户输入
    int ch = getch();
    
    // 清理ncurses环境并退出
    endwin();
    return 0;
}


```

简单的系统监控界面：



```
#include <ncurses.h>
#include <unistd.h>

int main() {
    // 初始化ncurses库
    initscr();
    cbreak(); // 禁用行缓冲
    noecho(); // 禁用回显
    nodelay(stdscr, true); // 非阻塞输入

    while (true) {
        // 清除屏幕
        clear();

        // 获取系统信息并显示
        // 这里使用假数据作为示例
        mvprintw(0, 0, "CPU Usage: 50%%");
        mvprintw(1, 0, "Memory Usage: 60%%");

        refresh(); // 刷新屏幕

        // 等待一段时间后继续循环
        usleep(500000); // 延迟500毫秒（0.5秒）
    }

    // 结束ncurses库
    endwin();

    return 0;
}

```

打印带颜色的文本效果示例：



```
#include <ncurses.h>

int main() {
    // 初始化ncurses库
    initscr();
    
    // 启用颜色支持
    start\_color();
    
    // 设置文本颜色对应的颜色对
    init\_pair(1, COLOR_RED, COLOR_BLACK);
    init\_pair(2, COLOR_GREEN, COLOR_BLACK);
    
    // 在屏幕上打印红色和绿色文本
    attron(COLOR\_PAIR(1));
    printw("This is a red text.\n");
    
    attron(COLOR\_PAIR(2));
    printw("This is a green text.\n");
    
    // 刷新窗口
    refresh();
    
    // 等待用户按下任意键后退出
    getch();
    
    // 结束并关闭ncurses库
    endwin();
    
    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





