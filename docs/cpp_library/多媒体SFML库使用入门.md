







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍SFML库使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. SFML库介绍](#smirk1_SFML_7)
	+ [:blush:2. SFML库安装](#blush2_SFML_18)
	+ [:satisfied:3. SFML库使用](#satisfied3_SFML_24)




### 😏1. SFML库介绍


SFML (Simple and Fast Multimedia Library) 是一个开源的、跨平台的C++多媒体库，它提供了一系列简单易用的接口和工具，可以方便地创建各种图形、音频、视频等应用程序。SFML 支持 Windows, Linux, macOS 和 Android 四种操作系统。


SFML 提供了以下功能：



> 
> 窗口管理：创建窗口，处理输入事件（键盘，鼠标），显示图像  
>  图形绘制：支持 2D图形绘制，包括基本图形（点，线，矩形，圆等）、渲染纹理、精灵动画等  
>  音频处理：支持 PCM 音频流播放、录制，以及音量控制、特效等  
>  网络通信：支持 TCP 和 UDP 协议的网络通信  
>  多线程处理：支持多线程并发处理，可以在主线程上更新窗口和处理输入事件
> 
> 
> 


### 😊2. SFML库安装


SFML官网：`https://www.sfml-dev.org/index.php`


可通过apt或source code的方式安装，这里用的apt安装。


在Linux开发环境中，通过这条命令安装：`sudo apt-get install libsfml-dev`


### 😆3. SFML库使用


下面创建一个示例程序，来验证SFML安装成功：


一个窗口绘制示例：



```
#include <SFML/Graphics.hpp>

int main()
{
    sf::RenderWindow window(sf::VideoMode(200, 200), "SFML works!");
    sf::CircleShape shape(100.f);
    shape.setFillColor(sf::Color::Green);

    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        window.clear();
        window.draw(shape);
        window.display();
    }

    return 0;
}

```

编译程序：



```
g++ -c main.cpp
g++ main.o -o sfml-app -lsfml-graphics -lsfml-window -lsfml-system
./sfml-app

```

运行如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b29b8bbad6114dcb91d634033c030921.png)


一个音频处理示例：



```
#include <SFML/Audio.hpp>
#include <iostream>

// 自定义音频处理函数
void processAudio(sf::Int16\* samples, std::size_t sampleCount) {
    // 遍历每个样本并进行处理（示例：将音量降低一半）
    for (std::size_t i = 0; i < sampleCount; ++i) {
        samples[i] /= 2;
    }
}

int main() {
    // 加载音频文件
    sf::SoundBuffer buffer;
    if (!buffer.loadFromFile("audio.wav")) {
        std::cout << "无法加载音频文件" << std::endl;
        return EXIT_FAILURE;
    }

    // 获取音频数据
    const sf::Int16\* samples = buffer.getSamples();
    std::size_t sampleCount = buffer.getSampleCount();

    // 处理音频数据
    processAudio(const\_cast<sf::Int16\*>(samples), sampleCount);

    // 播放处理后的音频
    sf::Sound sound;
    sound.setBuffer(buffer);
    sound.play();

    // 等待音频播放完成
    while (sound.getStatus() == sf::Sound::Playing) {}

    return EXIT_SUCCESS;
}

```

程序编译：



```
g++ main.cpp -o main -lsfml-audio -lsfml-system
# 运行如下，我现在没有音频文件
Failed to open sound file "audio.wav" (couldn't open stream)
无法加载音频文件

```

一个网络处理-tcpclient示例：



```
#include <SFML/Network.hpp>
#include <iostream>

int main() {
    // 创建TCP套接字
    sf::TcpSocket socket;

    // 连接到服务器
    sf::IpAddress serverIp = "127.0.0.1"; // 服务器IP地址
    unsigned short serverPort = 12345; // 服务器端口号
    sf::Socket::Status status = socket.connect(serverIp, serverPort);
    if (status != sf::Socket::Done) {
        std::cout << "无法连接到服务器" << std::endl;
        return EXIT_FAILURE;
    }

    // 发送数据到服务器
    std::string message = "Hello Server!";
    status = socket.send(message.c\_str(), message.size() + 1); // 发送包括空字符在内的全部消息内容
    if (status != sf::Socket::Done) {
        std::cout << "发送消息失败" << std::endl;
        return EXIT_FAILURE;
    }

    // 接收服务器的响应
    char buffer[1024];
    std::size_t received;
    status = socket.receive(buffer, sizeof(buffer), received);
    if (status != sf::Socket::Done) {
        std::cout << "接收响应失败" << std::endl;
        return EXIT_FAILURE;
    }

    // 输出服务器的响应
    std::cout << "服务器的响应: " << buffer << std::endl;

    // 关闭套接字连接
    socket.disconnect();

    return EXIT_SUCCESS;
}

```

程序编译：



```
g++ main.cpp -lsfml-network -lsfml-system

```

一个多线程处理示例：



```
#include <SFML/Graphics.hpp>
#include <iostream>
#include <thread>

// 线程函数
void threadFunction() {
    // 在后台线程中执行任务
    for (int i = 0; i < 5; ++i) {
        std::cout << "后台线程执行任务 " << i << std::endl;
        std::this_thread::sleep\_for(std::chrono::seconds(1));
    }
}

int main() {
    // 创建窗口
    sf::RenderWindow window(sf::VideoMode(800, 600), "SFML 多线程示例");

    // 创建后台线程
    std::thread thread(&threadFunction);

    // 渲染循环
    while (window.isOpen()) {
        // 处理事件
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                // 关闭窗口时停止后台线程并退出程序
                thread.join();
                window.close();
            }
            else if (event.type == sf::Event::MouseButtonPressed) {
                // 单击窗口时输出消息
                std::cout << "前台线程接收到鼠标点击事件" << std::endl;
            }
        }

        // 清空窗口
        window.clear(sf::Color::White);

        // 刷新窗口
        window.display();
    }

    return EXIT_SUCCESS;
}

```

程序编译：



```
g++ main.cpp -lsfml-graphics -lsfml-window -lsfml-system -lpthread
运行如下：
后台线程执行任务 0
后台线程执行任务 1
后台线程执行任务 2
后台线程执行任务 3
后台线程执行任务 4
前台线程接收到鼠标点击事件
前台线程接收到鼠标点击事件
前台线程接收到鼠标点击事件
前台线程接收到鼠标点击事件
前台线程接收到鼠标点击事件

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





