







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍FLTK图形界面库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__31)
	+ [:satisfied:3. 使用说明](#satisfied3__42)




### 😏1. 项目介绍


官网：`https://www.fltk.org/`


项目Github地址：`https://github.com/fltk/fltk`


`FLTK`（`Fast Light Toolkit`）是一个跨平台的C++图形用户界面（GUI）开发库。它是一个轻量级、高效且易于使用的库，旨在提供快速而灵活的GUI开发解决方案。


以下是一些`FLTK`库的特点和功能：



> 
> 1.跨平台支持：FLTK可以在多个操作系统上运行，包括Windows、macOS和Linux等。它使用了原生的API，使得应用程序在不同平台上的外观和行为保持一致。
> 
> 
> 



> 
> 2.轻量级和高效：FLTK库非常小巧，库文件大小较小，不依赖于其他大型库或运行时环境。它被设计为高效的库，具有快速的绘图和事件处理能力。
> 
> 
> 



> 
> 3.简单易用：FLTK提供了简单、直观的API和类，使得GUI开发变得容易上手。它具有清晰的文档和丰富的示例，帮助开发人员迅速入门并加速开发过程。
> 
> 
> 



> 
> 4.绘图和绘制：FLTK提供了强大的绘图功能，可以绘制各种形状、文本、图像等，以创建自定义界面元素和图形效果。
> 
> 
> 



> 
> 5.事件处理：FLTK库具有事件驱动的架构，可以响应鼠标、键盘和其他用户交互事件。开发人员可以轻松地编写事件处理代码来实现用户界面的交互性和响应性。
> 
> 
> 



> 
> 6.控件和窗口管理：FLTK库提供了多种常用的GUI控件，如按钮、文本框、滑块、列表框等，以及窗口和布局管理器，帮助开发人员构建复杂的用户界面。
> 
> 
> 



> 
> 7.支持OpenGL：FLTK与OpenGL集成良好，可以轻松创建使用OpenGL进行图形渲染和3D绘图的应用程序。
> 
> 
> 


`FLTK`是一个功能丰富、易于使用且跨平台的GUI开发库，适用于各种应用程序，从简单的工具到复杂的图形应用程序。


### 😊2. 环境配置



```
# apt安装
sudo apt install libfltk1.3-dev

```


```
# 编译
g++ -o main main.cpp  -lfltk

```

### 😆3. 使用说明


创建窗口示例：



```
#include <FL/Fl.H>
#include <FL/Fl\_Window.H>
#include <FL/Fl\_Button.H>

void buttonCallback(Fl_Widget\* widget, void\* data) {
    Fl_Button\* button = (Fl_Button\*)widget;
    button->label("Clicked!");
}

int main() {
    Fl_Window\* window = new Fl\_Window(300, 200, "FLTK Example");

    Fl_Button\* button = new Fl\_Button(100, 80, 100, 40, "Click Me");
    button->callback(buttonCallback);

    window->end();
    window->show();

    return Fl::run();
}

```

简单计算器示例：



```
#include <FL/Fl.H>
#include <FL/Fl\_Window.H>
#include <FL/Fl\_Button.H>
#include <FL/Fl\_Input.H>
#include <FL/Fl\_Output.H>
#include <iostream>
#include <sstream>

Fl_Input\* input;
Fl_Output\* output;

// 按钮回调函数
void buttonClicked(Fl_Widget\* widget, void\* data) {
    Fl_Button\* button = (Fl_Button\*)widget;
    const char\* label = button->label();

    std::string inputValue = input->value();
    std::stringstream ss(inputValue);
    double inputNumber;
    ss >> inputNumber;

    double result = 0.0;

    if (label == "+") {
        result = inputNumber + atof(output->value());
    } else if (label == "-") {
        result = atof(output->value()) - inputNumber;
    } else if (label == "\*") {
        result = inputNumber \* atof(output->value());
    } else if (label == "/") {
        if (inputNumber != 0) {
            result = atof(output->value()) / inputNumber;
        } else {
            output->value("Error: Division by zero");
            return;
        }
    }

    std::stringstream resultSS;
    resultSS << result;
    output->value(resultSS.str().c\_str());
}

int main() {
    Fl_Window\* window = new Fl\_Window(300, 200, "Simple Calculator");

    input = new Fl\_Input(10, 10, 280, 30);
    input->align(FL_ALIGN_TOP);

    output = new Fl\_Output(10, 50, 280, 30);
    output->align(FL_ALIGN_TOP);

    Fl_Button\* addButton = new Fl\_Button(10, 90, 60, 30, "+");
    addButton->callback(buttonClicked);

    Fl_Button\* subButton = new Fl\_Button(80, 90, 60, 30, "-");
    subButton->callback(buttonClicked);

    Fl_Button\* mulButton = new Fl\_Button(150, 90, 60, 30, "\*");
    mulButton->callback(buttonClicked);

    Fl_Button\* divButton = new Fl\_Button(220, 90, 60, 30, "/");
    divButton->callback(buttonClicked);

    window->end();
    window->show();

    return Fl::run();
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





