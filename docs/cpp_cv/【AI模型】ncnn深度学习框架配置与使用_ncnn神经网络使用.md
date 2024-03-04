







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ncnn框架配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. ncnn介绍](#smirk1_ncnn_7)
	+ [:blush:2. 环境配置](#blush2__23)
	+ [:satisfied:3. 使用说明](#satisfied3__40)




### 😏1. ncnn介绍


项目Github地址：`https://github.com/Tencent/ncnn`


`ncnn`（Nebula Convolutional Neural Network）是一个高效、轻量级的深度学习框架，由Tencent开发。它专为移动设备和嵌入式系统设计，旨在提供快速、低功耗、小型的深度学习推理解决方案。


以下是 ncnn 的一些关键特点和优势：



> 
> 1.轻量级和高效性能：ncnn 是一个轻量级的深度学习框架，可以在资源受限的设备上高效运行。它采用了一系列优化策略，包括定点化计算、内存管理、自动化并行和多线程等，以提供快速且高效的推理性能。
> 
> 
> 



> 
> 2.跨平台支持：ncnn 提供了广泛的跨平台支持，包括 Android、iOS、Windows、Linux 等多个操作系统。它还支持多种硬件平台，如 ARM、X86、MIPS 等，以及多个计算加速器，如 GPU、DSP 和 NPU。
> 
> 
> 



> 
> 3.易于集成：ncnn 提供了简洁的 C++ 接口，易于集成到现有项目中。它可以与各种主流的开发工具和库进行配合，如 OpenCV、TensorFlow Lite、ONNX 等，使开发人员能够更加灵活地使用深度学习模型。
> 
> 
> 



> 
> 4.丰富的模型支持：ncnn 支持多种深度学习模型的加载和推理，包括常见的卷积神经网络（CNN）、循环神经网络（RNN）、生成对抗网络（GAN）等。同时，ncnn 还提供了一些自带的模型，如 SqueezeNet、MobileNet、YOLO、FaceNet 等，可用于快速原型设计和开发。
> 
> 
> 



> 
> 5.开源框架：ncnn 是一个开源框架，源代码托管在 GitHub 上。这使得开发人员能够查看和修改源代码，以满足自己的需求，并享受来自全球社区的支持和贡献。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# 安装依赖
sudo apt-get install -y build-essential git cmake libprotobuf-dev protobuf-compiler libopencv-dev
# 源码安装
git clone https://github.com/Tencent/ncnn.git
cd ncnn
mkdir build
cd build
cmake ..
make
sudo make install

```

使用时，引用头文件：`#include "net.h"`


### 😆3. 使用说明


基础测试：



```
cd ../benchmark
cp ../build/benchmark/benchncnn . && ./benchmark

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a140b49a10d7438f8aa3f1de5538fb65.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





