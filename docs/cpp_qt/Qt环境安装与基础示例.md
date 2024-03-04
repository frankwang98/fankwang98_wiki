> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Qt环境配置与入门。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

<iframe src="//player.bilibili.com/player.html?aid=920485946&bvid=BV1Wu4y1776o&cid=1320557821&p=1" width="600" height="400" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

### 😏1. Qt介绍

`Qt`是一个跨平台的C++应用程序开发框架，被广泛用于开发图形界面和嵌入式系统应用程序。它最初由奥斯陆的一家挪威公司Trolltech（现在被Digia收购）开发，并于1995年首次发布。

`Qt Creator`是Qt官方的IDE，此外也兼容第三方扩展，如`Visual Studio、Python`。Qt可以使用纯C++开发界面和逻辑；也可以用`QML`做界面，C++做逻辑，QML效果会好一些。`Qt Quick`技术是指用QML快速开发图形界面。

Qt具有许多特性，使得其成为一个流行的开发框架：

> 1.跨平台支持：Qt可以在各种操作系统上运行，包括Windows、MacOS、Linux、Android和iOS等。并且Qt应用程序在不同平台运行时的外观和行为都相同，这大大提高了开发效率和用户体验。

> 2.应用程序开发：Qt提供了丰富的库和工具，用于开发各种应用程序，从简单的命令行工具到复杂的图形用户界面应用程序。

> 3.图形用户界面设计：Qt拥有强大的界面设计工具Qt Designer，可用于创建漂亮的用户界面。它还支持自定义样式表和主题，以及无缝集成SVG图形等。

> 4.数据库访问：Qt提供了名为Qt SQL的模块，用于访问各种关系型数据库。它可轻松地连接到多个数据库，如MySQL、Oracle和SQLite等。

> 5.网络编程：Qt网络模块提供了一组高级API，用于开发基于TCP、UDP和HTTP协议的网络应用程序。

> 6.多语言支持：Qt提供了强大的多语言支持，包括Unicode和本地化字符串等。这使得开发者可以轻松地编写跨国界面并支持多种语言。

Qt是一个功能强大，易于使用且具有跨平台特性的应用程序开发框架。它被视为开发图形用户界面和嵌入式系统应用程序的首选框架之一。主要用于嵌入式和桌面的图形界面开发，具体有`基本界面、二维绘图、三维绘图、音视频、网络通信、文件管理`等。

### 😊2. Qt环境配置

国内下载Qt可以从镜像网站下载，常用的几个网站是：

```bash
中国科学技术大学：http://mirrors.ustc.edu.cn/qtproject/
清华大学：https://mirrors.tuna.tsinghua.edu.cn/qt/
北京理工大学：http://mirror.bit.edu.cn/qtproject/
中国互联网络信息中心：http://mirror.bit.edu.cn/qtproject/

```

可以下载多平台的版本，例如Windows的`exe`、Ubuntu的`run`，运行安装即可，选择需要的模块。目前更新到Qt6，Qt5.15之后就只能在线安装了。

另外，在Ubuntu也可以直接命令行安装：`sudo apt install qt5-default`

对应版本是5.9.5稳定版，可通过`qmake -v`查看。

### 😆3. Qt入门示例

Qt安装完成后，自带丰富的`example`，可以学习。

可以自己新建一个项目，了解Qt的基本开发流程。

新建Qt工程时，Qt的Application有多个应用程序的创建模板，我们先了解以下两种：

1. Qt Widgets Application，支持桌面平台的有图形用户界面的应用程序。GUI 的设计完全基于 C++ 语言，采用 Qt 提供的一套 C++ 类库。
2. Qt Console Application，控制台应用程序，无 GUI 界面，一般用于学习 C/C++ 语言，只需要简单的输入输出操作时可创建此类项目。

Qt有3种基类：

1. QMainWindow 是主窗口类，主窗口具有主菜单栏、工具栏和状态栏，类似于一般的应用程序的主窗口；
2. QWidget 是所有具有可视界面类的基类，选择 QWidget 创建的界面对各种界面组件都可以支持；
3. QDialog 是对话框类，可建立一个基于对话框的界面。

可以直接打开mainwindow.ui来到`Designer`设计模式，同VB这类图形化编程语言类似。然后添加一个Label控件，可以添加文字，更改大小，基本上和VB的操作模式一样。也可以在代码中直接生成图形控件。

程序生成控件的代码示例如下：

```cpp
#include "mainwindow.h"
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QLabel *label = new QLabel("Hello Qt!");
    label->show();
    return app.exec();
}

```

Qt的编译工具默认是`qmake`，Qt6之后编译工具改为了`cmake`，两者各有优势，需要学会去使用。

Ubuntu可以在命令行直接生成pro工程文件，较为方便：

```
qmake -project
qmake xxx.pro
make	# 编译

```

Windows Qt程序的构建快捷键`Ctrl+B`，运行快捷键`Ctrl+R`，可以在构建和运行设置中`自定义构建的目录`。

### 😆4. Qt信号槽机制

信号槽机制是一种用于在对象之间进行通信的机制。它是Qt框架的核心特性之一，使得在事件发生时能够自动触发相应的操作，从而实现对象之间的解耦和灵活的交互。底层基于libuv库，以实现高性能的事件驱动和非阻塞I/O操作。

信号槽的连接有多种方式：

1. SIGNAL/SLOT
2. 函数地址
3. UI界面-转到槽
4. UI界面-信号槽编辑器
5. lambda表达式

此外，还有多种定义和使用信号槽的方式，如连接重载的信号和槽，可以用函数指针的方式，让编译器自动推断有参和无参函数。

```cpp
    // 定义函数指针，编译器自动推断，有参和无参
    void (Commander::*pGo)() = &Commander::go;
    void (Soldier::*pFight)() = &Soldier::fight;
    connect(&commander, pGo, &soldier, pFight);
    commander.go();
    void (Commander::*pGoForFreedom)(QString) = &Commander::go;
    void (Soldier::*pFightForFreedom)(QString) = &Soldier::fight;
    connect(&commander, pGoForFreedom, &soldier, pFightForFreedom);
    commander.go("for freedom!");

```

还有一个信号连接多个槽，多个信号连接一个槽，信号连接信号，断开连接等其他应用，可以在项目中合理使用。

以上。





