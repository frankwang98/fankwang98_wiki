







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍freeglut环境配置与创建点示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. FreeGLUT介绍](#smirk1_FreeGLUT_7)
	+ [:blush:2. 环境安装与配置](#blush2__21)
	+ [:satisfied:3. 应用示例](#satisfied3__43)




### 😏1. FreeGLUT介绍


`FreeGLUT`（Free OpenGL Utility Toolkit）是一个开源的替代性GLUT库，它提供了类似于GLUT的功能，并在其基础上进行了扩展和改进。FreeGLUT的目标是提供一个跨平台、功能丰富且易于使用的工具库，用于OpenGL程序开发。


下面是一些`FreeGLUT`库的特点和功能：



> 
> 1.跨平台支持：FreeGLUT可以在多个操作系统上运行，包括Windows、Linux和Mac OS X等。这使得开发者可以使用相同的代码在不同平台上进行OpenGL程序开发。
> 
> 
> 



> 
> 2.窗口管理：FreeGLUT提供了创建窗口、处理窗口事件（如键盘和鼠标输入）、窗口大小调整等功能，使得开发者可以轻松管理和交互窗口。它还支持多个窗口和全屏模式。
> 
> 
> 



> 
> 3.用户输入处理：FreeGLUT提供了处理用户输入（键盘和鼠标）的接口。开发者可以通过注册回调函数来处理键盘按键、鼠标点击等事件，实现与用户的交互。
> 
> 
> 



> 
> 4.定时器：类似于GLUT，FreeGLUT也支持定时器功能。你可以通过设置回调函数实现定时执行某些操作，如动画效果、游戏循环等。
> 
> 
> 



> 
> 5.扩展功能：FreeGLUT通过增加一些额外的功能来扩展原始的GLUT库。例如，它支持菜单和子菜单的创建和管理，支持鼠标滚轮事件、支持多种输入设备等。
> 
> 
> 


### 😊2. 环境安装与配置


下载链接：`https://packages.msys2.org/package/mingw-w64-x86_64-freeglut`


可以在这里下载基于`mingw64`编译的`freeglut`库，然后在clion里cmake配置项如下：



```
cmake_minimum_required(VERSION 3.19)
project(opengl_demo)

set(CMAKE_CXX_STANDARD 14)

include_directories("./env/include")
link_directories("./env/lib")

add_executable(opengl_demo main.cpp glad.c)
target_link_libraries(opengl_demo
        glfw3
        opengl32
        freeglut
        glu32
)

```

### 😆3. 应用示例


点创建示例：



```
#include <windows.h>
#include <GL/glut.h>

void init()
{
    glClearColor(0.0, 0.0, 0.0, 0.0);	//backgroud color(RGB(0,0,0-black;)+透明度)
    glMatrixMode(GL_PROJECTION);	//projection\_mode
    glLoadIdentity();
    gluOrtho2D(-100, 100, -100, 100);	//显示范围
}

void myPoints()
{
    glClear(GL_COLOR_BUFFER_BIT);
    glPointSize(3);
    glBegin(GL_POINTS);	//开始
    // points start
    glColor3f(1.0, 0.0, 0.0);	//red
    glVertex2i(-3, 3);
    glColor3f(0.0, 1.0, 0.0);	//green
    glVertex2i(10, 20);
    glColor3f(0.0, 0.0, 1.0);	//blue
    glVertex2i(0, -15);
    // points end
    glEnd();	//结束
    glFlush();	//渲染
}

int main(int argc, char\* argv[])
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
    glutInitWindowPosition(200, 300);
    glutInitWindowSize(300, 300);
    glutCreateWindow("Display Points on opengl1");

    init();	//初始化
    glutDisplayFunc(myPoints);	//回调函数（导入参数）
    glutMainLoop();

    return 0;
}

```

创建五角星示例：



```
#include <iostream>
#include <gl/glut.h>

using namespace std;

struct Point {
    int x;
    int y;
};

//

#define VERTEX\_COUNT 5

Point points[VERTEX_COUNT] = {
        103, 273,
        516, 273,
        184, 32,
        308, 452,
        439, 32
};

//

void draw\_dda(Point p1, Point p2) {
    GLfloat dx = p2.x - p1.x;
    GLfloat dy = p2.y - p1.y;

    GLfloat x1 = p1.x;
    GLfloat y1 = p1.y;

    GLfloat step = 0;

    if(abs(dx) > abs(dy)) {
        step = abs(dx);
    } else {
        step = abs(dy);
    }

    GLfloat xInc = dx/step;
    GLfloat yInc = dy/step;

    for(float i = 1; i <= step; i++) {
        glVertex2i(x1, y1);
        x1 += xInc;
        y1 += yInc;
    }
}

void init() {
    glClearColor(1.0, 1.0, 1.0, 0.0);
    glColor3f(0.0f, 0.0f, 0.0f);
    gluOrtho2D(0.0, 640.0, 0.0, 480.0);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    glPointSize(1.0f);
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glBegin(GL_POINTS);
    Point stPoint = points[0];

    for(int i = 1; i < VERTEX_COUNT; i++) {
        draw\_dda(stPoint, points[i]);
        stPoint = points[i];
    }
    draw\_dda(stPoint, points[0]);

    glEnd();
    glFlush();
}

int main(int argc, char\*\* argv) {
    glutInit(&argc, argv);
    glutInitWindowPosition(400, 200);
    glutInitWindowSize(640, 480);
    glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
    glutCreateWindow("OpenGL");
    init();
    glutDisplayFunc(display);
    glutMainLoop();
    return 0;
}

```

此外，可以参考该项目：`https://github.com/sprintr/opengl-examples`，提供了一些`glut`示例。


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





