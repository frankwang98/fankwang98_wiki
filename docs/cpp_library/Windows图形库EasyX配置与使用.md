







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Windows图形库EasyX配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__21)
	+ [:satisfied:3. 使用说明](#satisfied3__39)
	+ - [窗口绘制圆的示例：](#_41)
		- [获取鼠标和键盘事件示例：](#_60)
		- [鼠标操作与绘制示例：](#_114)
		- [贪吃蛇示例：](#_160)




### 😏1. 项目介绍


官网：`https://easyx.cn/`


`EasyX`是一个基于`Windows`的简单图形库，它提供了一个易于使用的图形绘制接口，适用于初学者和爱好者进行图形编程。下面是`EasyX`库的一些特点和功能：



> 
> 1.易于学习和使用：EasyX采用了简单的图形绘制接口，使得初学者可以快速上手。它提供了一些基本的绘图函数，如画线、画圆、绘制文本等，使用户能够轻松创建图形界面和动画效果。
> 
> 
> 



> 
> 2.轻量级和快速：EasyX是一个轻量级的图形库，不需要复杂的安装和配置过程。它使用GDI（图形设备接口）来进行图形绘制，具有较快的绘图速度和相对较低的系统资源占用。
> 
> 
> 



> 
> 3.图形界面设计：EasyX提供了一些常用的图形界面控件，如按钮、文本框、滚动条等，使用户可以轻松创建交互式的图形界面。
> 
> 
> 



> 
> 4.动画和游戏开发：EasyX支持实时动画和游戏开发，提供了帧动画、双缓冲技术等功能，使用户能够创建流畅的动画效果和简单的游戏。
> 
> 
> 



> 
> 5.跨平台：EasyX主要针对Windows平台，支持Windows XP及以上版本。然而，EasyX也可以在部分Linux环境下使用，如Wine模拟器。
> 
> 
> 


### 😊2. 环境配置


我这里用的`Clion + mingw`，EasyX的下载地址：`https://easyx.cn/download/easyx4mingw_20220901.zip`


CMakeLists.txt示例：



```
cmake_minimum_required(VERSION 3.19)
project(easyx_demo)

set(CMAKE_CXX_STANDARD 14)

include_directories("D:/develop/easyx4mingw\_20220901/include")
link_directories("D:/develop/easyx4mingw\_20220901/lib64")

add_executable(easyx_demo main.cpp)
target_link_libraries(easyx_demo -leasyx)

```

### 😆3. 使用说明


官网也提供了函数使用的文档，并给出了一些示例：`https://docs.easyx.cn/zh-cn/char-matrix`


#### 窗口绘制圆的示例：



```
#include <graphics.h>
#include <conio.h>

int main()
{
    initgraph(640, 480);  // 创建一个640x480的绘图窗口

    circle(320, 240, 100);  // 在窗口中心画一个半径为100的圆

    getch();  // 等待用户按下任意键

    closegraph();  // 关闭绘图窗口
    return 0;
}

```

#### 获取鼠标和键盘事件示例：



```
#include <graphics.h>
#include <conio.h>
#include <stdio.h>

int main()
{
    initgraph(640, 480);  // 创建一个640x480的绘图窗口

    while (true)
    {
        // 监听键盘事件
        if (kbhit())
        {
            char ch = getch();  // 获取键盘按键
            if (ch == 'q' || ch == 'Q')
                break;  // 如果按下了Q键，退出循环
        }

        // 监听鼠标事件
        if (MouseHit())
        {
            MOUSEMSG mouseMsg = GetMouseMsg();

            if (mouseMsg.uMsg == WM_MOUSEMOVE)
            {
                int x = mouseMsg.x;
                int y = mouseMsg.y;
                // 在控制台输出鼠标移动的坐标
                printf("Mouse move: x = %d, y = %d\n", x, y);
            }
            else if (mouseMsg.uMsg == WM_LBUTTONDOWN)
            {
                int x = mouseMsg.x;
                int y = mouseMsg.y;
                // 在控制台输出鼠标左键按下的坐标
                printf("Left button down: x = %d, y = %d\n", x, y);
            }
            else if (mouseMsg.uMsg == WM_LBUTTONUP)
            {
                int x = mouseMsg.x;
                int y = mouseMsg.y;
                // 在控制台输出鼠标左键释放的坐标
                printf("Left button up: x = %d, y = %d\n", x, y);
            }
        }
    }

    closegraph();  // 关闭绘图窗口
    return 0;
}

```

#### 鼠标操作与绘制示例：



```
#include <graphics.h>

int main()
{
    // 初始化图形窗口
    initgraph(640, 480);

    ExMessage m;		// 定义消息变量

    while(true)
    {
        // 获取一条鼠标或按键消息
        m = getmessage(EX_MOUSE | EX_KEY);

        switch(m.message)
        {
            case WM_MOUSEMOVE:
                // 鼠标移动的时候画红色的小点
                putpixel(m.x, m.y, RED);
                break;

            case WM_LBUTTONDOWN:
                // 如果点左键的同时按下了 Ctrl 键
                if (m.ctrl)
                    // 画一个大方块
                    rectangle(m.x - 10, m.y - 10, m.x + 10, m.y + 10);
                else
                    // 画一个小方块
                    rectangle(m.x - 5, m.y - 5, m.x + 5, m.y + 5);
                break;

            case WM_KEYDOWN:
                if (m.vkcode == VK_ESCAPE)
                    return 0;	// 按 ESC 键退出程序
        }
    }

    // 关闭图形窗口
    closegraph();
    return 0;
}

```

#### 贪吃蛇示例：



```
#include <graphics.h>
#include <conio.h>
#include <time.h>

const int CELL_SIZE = 20;  // 每个单元格的尺寸
const int WIDTH = 800;  // 窗口宽度
const int HEIGHT = 600;  // 窗口高度
const int ROWS = HEIGHT / CELL_SIZE;  // 行数
const int COLS = WIDTH / CELL_SIZE;  // 列数

struct Point  // 坐标点结构体
{
    int x, y;
};

enum Direction  // 移动方向枚举
{
    UP,
    DOWN,
    LEFT,
    RIGHT
};

void DrawCell(int x, int y, COLORREF color)
{
    setfillcolor(color);
    setlinecolor(color);
    fillrectangle(x \* CELL_SIZE, y \* CELL_SIZE, (x + 1) \* CELL_SIZE, (y + 1) \* CELL_SIZE);
}

void DrawSnake(Point\* snake, int length)
{
    for (int i = 0; i < length; i++)
    {
        if (i == 0)
            DrawCell(snake[i].x, snake[i].y, RGB(0, 255, 0));  // 绘制蛇头
        else
            DrawCell(snake[i].x, snake[i].y, RGB(0, 200, 0));  // 绘制蛇身
    }
}

void GenerateFood(Point\* snake, int length, Point& food)
{
    while (true)
    {
        food.x = rand() % COLS;
        food.y = rand() % ROWS;

        bool overlap = false;
        for (int i = 0; i < length; i++)
        {
            if (snake[i].x == food.x && snake[i].y == food.y)
            {
                overlap = true;
                break;
            }
        }

        if (!overlap)
            break;
    }

    DrawCell(food.x, food.y, RGB(255, 0, 0));  // 绘制食物
}

void UpdateSnake(Point\* snake, int& length, Direction direction, bool& gameOver)
{
    Point head = snake[0];
    Point newHead = head;

    switch (direction)
    {
        case UP:
            newHead.y--;
            break;
        case DOWN:
            newHead.y++;
            break;
        case LEFT:
            newHead.x--;
            break;
        case RIGHT:
            newHead.x++;
            break;
    }

    if (newHead.x < 0 || newHead.x >= COLS || newHead.y < 0 || newHead.y >= ROWS)
    {
        gameOver = true;  // 越界，游戏结束
        return;
    }

    for (int i = length - 1; i > 0; i--)
    {
        snake[i] = snake[i - 1];
    }

    snake[0] = newHead;

    for (int i = 1; i < length; i++)
    {
        if (snake[i].x == newHead.x && snake[i].y == newHead.y)
        {
            gameOver = true;  // 撞到自己，游戏结束
            return;
        }
    }
}

int main()
{
    initgraph(WIDTH, HEIGHT);  // 创建一个指定宽高的绘图窗口

    srand(static\_cast<unsigned int>(time(nullptr)));  // 初始化随机数种子

    Point\* snake = new Point[ROWS \* COLS];  // 蛇的坐标数组
    int length = 1;  // 蛇的初始长度
    Direction direction = RIGHT;  // 蛇的初始移动方向
    bool gameOver = false;  // 游戏是否结束

    // 初始化蛇的初始位置
    snake[0].x = COLS / 2;
    snake[0].y = ROWS / 2;

    Point food;  // 食物的坐标

    GenerateFood(snake, length, food);  // 生成食物

    while (!gameOver)
    {
        // 监听键盘事件
        if (kbhit())
        {
            char ch = getch();
            switch (ch)
            {
                case 'W':
                case 'w':
                    if(direction != DOWN)
                        direction = UP;
                    break;
                case 'S':
                case 's':
                    if (direction != UP)
                        direction = DOWN;
                    break;
                case 'A':
                case 'a':
                    if (direction != RIGHT)
                        direction = LEFT;
                    break;
                case 'D':
                case 'd':
                    if (direction != LEFT)
                        direction = RIGHT;
                    break;
            }
        }

        cleardevice();  // 清空绘图窗口

        UpdateSnake(snake, length, direction, gameOver);  // 更新蛇的位置

        if (snake[0].x == food.x && snake[0].y == food.y)
        {
            length++;  // 蛇吃到食物，长度增加
            GenerateFood(snake, length, food);  // 生成新的食物
        }

        DrawSnake(snake, length);  // 绘制蛇

        DrawCell(food.x, food.y, RGB(255, 0, 0));  // 绘制食物

        Sleep(100);
    }

    delete[] snake;  // 释放内存

    closegraph();  // 关闭绘图窗口

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





