







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Linux端V4L2视频设备库。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__23)
	+ [:satisfied:3. 使用说明](#satisfied3__32)




### 😏1. 项目介绍


Video4Linux2（`V4L2`）是一个用于Linux操作系统的视频设备驱动框架。它提供了一个统一的接口，用于在应用程序和视频设备之间进行通信和交互。


V4L2支持各种类型的视频设备，包括USB摄像头、摄像机、TV调谐器、网络摄像头等。通过使用V4L2，开发者可以轻松地访问和控制视频设备，以捕获视频流、调整图像参数、设置视频格式和分辨率等。


以下是V4L2的一些重要特点和概念：



> 
> 1.设备节点：每个视频设备在Linux系统中都表示为一个设备节点，通常位于/dev/video\*路径下。应用程序通过打开这些设备节点来访问相应的视频设备。
> 
> 
> 



> 
> 2.视频捕捉：V4L2允许应用程序从视频设备中捕获视频帧或图像。它提供了一系列的API函数，使应用程序能够请求存储视频帧的缓冲区，并在设备准备好时将其读取到内存中。
> 
> 
> 



> 
> 3.视频输出：除了捕获视频，V4L2还支持将视频数据发送到视频设备，以便在外部显示设备上进行输出。应用程序可以将视频帧写入输出缓冲区，并通过相应的IOCTL调用将其发送到视频设备。
> 
> 
> 



> 
> 4.控制和参数设置：V4L2允许应用程序对视频设备进行控制和配置。例如，应用程序可以设置摄像头的亮度、对比度、饱和度等参数，选择摄像头的输入源，设置视频格式和分辨率等。
> 
> 
> 



> 
> 5.帧缓冲管理：V4L2通过Frame Buffer子系统来管理视频帧的缓冲区。它提供了API函数来请求和管理用于存储视频帧的缓冲区，并进行帧缓冲的交换和处理。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# v4l2是linux内核的一部分，只需安装开发库
sudo apt-get install libv4l-dev
# 使用v4l2开发
# 在应用程序中使用 #include <linux/videodev2.h> 来引入V4L2的头文件，并使用相关的API函数

```

### 😆3. 使用说明


下面进行使用分析：


基于v4l2调用usb摄像头并用opencv显示示例：



```
#include <iostream>
#include <cstdlib>
#include <cstring>
#include <fcntl.h>
#include <unistd.h>
#include <sys/ioctl.h>
#include <sys/mman.h> //共享内存
#include <linux/videodev2.h>
#include <opencv2/opencv.hpp>

#define WIDTH 640
#define HEIGHT 480

int main() {
    int fd;
    struct v4l2\_capability cap;
    struct v4l2\_format fmt;
    struct v4l2\_requestbuffers req;
    struct v4l2\_buffer buf;
    enum v4l2\_buf\_type type;

    // 打开摄像头设备
    fd = open("/dev/video0", O_RDWR);
    if (fd == -1) {
        std::cerr << "无法打开摄像头设备" << std::endl;
        return 1;
    }

    // 查询摄像头能力
    if (ioctl(fd, VIDIOC_QUERYCAP, &cap) == -1) {
        std::cerr << "无法查询摄像头能力" << std::endl;
        close(fd);
        return 1;
    }

    // 设置视频格式
    memset(&fmt, 0, sizeof(fmt));
    fmt.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
    fmt.fmt.pix.width = WIDTH;
    fmt.fmt.pix.height = HEIGHT;
    fmt.fmt.pix.pixelformat = V4L2_PIX_FMT_YUYV; // YUV格式
    if (ioctl(fd, VIDIOC_S_FMT, &fmt) == -1) {
        std::cerr << "无法设置视频格式" << std::endl;
        close(fd);
        return 1;
    }

    // 请求视频缓冲区
    memset(&req, 0, sizeof(req));
    req.count = 1;
    req.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
    req.memory = V4L2_MEMORY_MMAP;
    if (ioctl(fd, VIDIOC_REQBUFS, &req) == -1) {
        std::cerr << "无法请求视频缓冲区" << std::endl;
        close(fd);
        return 1;
    }

    // 映射视频缓冲区到用户空间
    struct v4l2\_buffer\* buffers = new v4l2_buffer[req.count];
    void\*\* frame_buffers = new void\*[req.count];
    for (int i = 0; i < req.count; i++) {
        memset(&buf, 0, sizeof(buf));
        buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
        buf.memory = V4L2_MEMORY_MMAP;
        buf.index = i;
        if (ioctl(fd, VIDIOC_QUERYBUF, &buf) == -1) {
            std::cerr << "无法查询视频缓冲区" << std::endl;
            close(fd);
            return 1;
        }
        frame_buffers[i] = mmap(NULL, buf.length, PROT_READ | PROT_WRITE, MAP_SHARED, fd, buf.m.offset);
        if (frame_buffers[i] == MAP_FAILED) {
            std::cerr << "无法映射视频缓冲区到用户空间" << std::endl;
            close(fd);
            return 1;
        }
    }

    // 入队视频缓冲区
    for (int i = 0; i < req.count; i++) {
        memset(&buf, 0, sizeof(buf));
        buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
        buf.memory = V4L2_MEMORY_MMAP;
        buf.index = i;
        if (ioctl(fd, VIDIOC_QBUF, &buf) == -1) {
            std::cerr << "无法入队视频缓冲区" << std::endl;
            close(fd);
            return 1;
        }
    }

    // 开始视频流采集
    type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
    if (ioctl(fd, VIDIOC_STREAMON, &type) == -1) {
        std::cerr << "无法开始视频流采集" << std::endl;
        close(fd);
        return 1;
    }

    // 循环获取并显示相机数据
    cv::Mat frame(HEIGHT, WIDTH, CV_8UC2);
    cv::namedWindow("Camera", cv::WINDOW_AUTOSIZE);
    while (true) {
        // 出队视频缓冲区
        memset(&buf, 0, sizeof(buf));
        buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
        buf.memory = V4L2_MEMORY_MMAP;
        if (ioctl(fd, VIDIOC_DQBUF, &buf) == -1) {
            std::cerr << "无法出队视频缓冲区" << std::endl;
            close(fd);
            return 1;
        }

        // 处理相机数据（这里只是简单地将YUYV格式的数据转换为RGB格式）
        cv::cvtColor(cv::Mat(HEIGHT, WIDTH, CV_8UC2, frame_buffers[buf.index]), frame, cv::COLOR_YUV2BGR_YUYV);

        // 显示相机数据
        cv::imshow("Camera", frame);
        if (cv::waitKey(1) == 27) {
            break; // 按下Esc键退出循环
        }

        // 再次入队视频缓冲区
        if (ioctl(fd, VIDIOC_QBUF, &buf) == -1) {
            std::cerr << "无法再次入队视频缓冲区" << std::endl;
            close(fd);
            return 1;
        }
    }

    // 停止视频流采集
    type = V4L2_BUF_TYPE_VIDEO_CAPTURE;
    if (ioctl(fd, VIDIOC_STREAMOFF, &type) == -1) {
        std::cerr << "无法停止视频流采集" << std::endl;
        close(fd);
        return 1;
    }

    // 解除映射视频缓冲区
    for (int i = 0; i < req.count; i++) {
        munmap(frame_buffers[i], buf.length);
    }

    // 关闭摄像头设备
    close(fd);

    delete[] buffers;
    delete[] frame_buffers;

    return 0;
}

```

编译运行：



```
g++ -o main main.cpp `pkg-config --libs opencv`
./main

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





