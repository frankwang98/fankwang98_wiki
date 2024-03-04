






**心口如一，犹不失为光明磊落丈夫之行也。——梁启超**  
 



#### 文章目录


* + [:smirk:1. AI模型部署方法](#smirk1_AI_2)
	+ [:blush:2. AI模型部署框架](#blush2_AI_14)
	+ - [ONNX](#ONNX_17)
		- [NCNN](#NCNN_25)
		- [OpenVINO](#OpenVINO_32)
		- [TensorRT](#TensorRT_40)
		- [Mediapipe](#Mediapipe_50)
		- [如何选择](#_58)
	+ [:satisfied:3. AI模型部署平台](#satisfied3_AI_64)




### 😏1. AI模型部署方法


在AI深度学习模型的训练中，一般会用Python语言实现，原因是其灵活、可读性强。但在AI模型实际部署中，主要会用到C++，原因在于其语言自身的高效性。


对于AI模型的部署，有这几种方法可供选择：


1. 使用 C++ 实现深度学习模型（★★★）  
 可以使用 C++ 编写自己的深度学习库或框架，但这需要您具有深入的数学和计算机科学知识。此外，也可以使用现有的开源 C++ 框架，如 TensorRT 和 OpenCV DNN 等。
2. 导出深度学习模型到应用平台（★★）  
 许多深度学习框架支持将训练好的模型导出为 C++ 可以读取的格式，如 ONNX、TensorFlow Lite、Caffe2 等。这样可以在不重新训练模型的情况下，在 C++ 代码中加载和运行模型。
3. 使用 C++ 库来加载和运行深度学习模型（★）  
 许多开发人员使用现有的 C++ 库来加载和运行深度学习模型，如 OpenCV、Dlib、Libtorch 等。这些库提供了一些方便的函数和接口，可以轻松地集成到您的 C++ 项目中。


### 😊2. AI模型部署框架


模型部署常见的推理框架有：**ONNX、NCNN、OpenVINO、 TensorRT、Mediapipe**。


#### ONNX


官网：`https://onnx.ai/`  
 github：`https://github.com/onnx/onnx`


开放神经网络交换ONNX（Open Neural Network Exchange）是一套表示深度神经网络模型的开放格式，由微软和Facebook于2017推出，然后迅速得到了各大厂商和框架的支持。通过短短几年的发展，已经成为表示深度学习模型的实际标准，并且通过ONNX-ML，可以支持传统非神经网络机器学习模型，大有一统整个AI模型交换标准的趋势。


无论使用什么样的训练框架来训练模型（比如TensorFlow/Pytorch/OneFlow/Paddle），你都可以在训练后将这些框架的模型统一转为ONNX存储。ONNX文件不仅存储了神经网络模型的权重，还存储了模型的结构信息、网络中各层的输入输出等一些信息。目前，ONNX主要关注在模型预测方面（inferring），将转换后的ONNX模型，转换成我们需要使用不同框架部署的类型，可以很容易的部署在兼容ONNX的运行环境中。


#### NCNN


github：`https://github.com/Tencent/ncnn`


ncnn 是一个为手机端极致优化的高性能神经网络前向计算框架，也是腾讯优图实验室成立以来的第一个开源项目。ncnn 从设计之初深刻考虑手机端的部署和使用，无第三方依赖，跨平台，手机端 CPU 的速度快于目前所有已知的开源框架。基于 ncnn，开发者能够将深度学习算法轻松移植到手机端高效执行，开发出人工智能 App。


从NCNN的发展矩阵可以看出，NCNN覆盖了几乎所有常用的系统平台，尤其是在移动平台上的适用性更好，在Linux、Windows和Android、以及iOS、macOS平台上都可以使用GPU来部署模型。


#### OpenVINO


官网：`https://docs.openvino.ai/latest/home.html`  
 github：`https://github.com/openvinotoolkit/openvino`


OpenVINO是一种可以加快高性能计算机视觉和深度学习视觉应用开发速度的工具套件，支持各种英特尔平台的硬件加速器上进行深度学习，并且允许直接异构执行。OpenVINO™工具包是用于快速开发应用程序和解决方案的综合工具包，可解决各种任务，包括模拟人类视觉，自动语音识别，自然语言处理，推荐系统等。可在英特尔®硬件上扩展计算机视觉和非视觉工作负载，从而最大限度地提高性能。


OpenVINO在模型部署前，首先会对模型进行优化，模型优化器会对模型的拓扑结构进行优化，去掉不需要的层，对相同的运算进行融合、合并以加快运算效率，减少内存拷贝；FP16、INT8量化也可以在保证精度损失很小的前提下减小模型体积，提高模型的性能。在部署方面，OpenVIVO的开发也是相对比较简单的，提供了C、C++和python3种语言编程接口。


#### TensorRT


官网：`https://developer.nvidia.com/zh-cn/tensorrt`  
 github：`https://github.com/NVIDIA/TensorRT`


NVIDIA TensorRT™ 是用于高性能深度学习推理的 SDK。此 SDK 包含深度学习推理优化器和运行时环境，可为深度学习推理应用提供低延迟和高吞吐量。


在推理过程中，基于 TensorRT 的应用程序的执行速度可比 CPU 平台的速度快 40 倍。借助 TensorRT，您可以优化在所有主要框架中训练的神经网络模型，精确校正低精度，并最终将模型部署到超大规模数据中心、嵌入式或汽车产品平台中。


TensorRT 以 NVIDIA 的并行编程模型 CUDA 为基础构建而成，可帮助您利用 CUDA-X 中的库、开发工具和技术，针对人工智能、自主机器、高性能计算和图形优化所有深度学习框架中的推理。


#### Mediapipe


官网：`https://google.github.io/mediapipe/`  
 github：`https://github.com/google/mediapipe`


MediaPipe是一款由 Google Research 开发并开源的多媒体机器学习模型应用框架。在谷歌，一系列重要产品，如 YouTube、Google Lens、ARCore、Google Home 以及 Nest，都已深度整合了 MediaPipe。作为一款跨平台框架，MediaPipe 不仅可以被部署在服务器端，更可以在多个移动端 （安卓和苹果 iOS）和嵌入式平台（Google Coral 和树莓派）中作为设备端机器学习推理 （On-device Machine Learning Inference）框架。


除了上述的特性，MediaPipe 还支持 TensorFlow 和 TF Lite 的推理引擎（Inference Engine），任何 TensorFlow 和 TF Lite 的模型都可以在 MediaPipe 上使用。同时，在移动端和嵌入式平台，MediaPipe 也支持设备本身的 GPU 加速。


#### 如何选择


1. ONNXRuntime 是可以运行在多平台 (Windows，Linux，Mac，Android，iOS) 上的一款推理框架，它接受 ONNX 格式的模型输入，支持 GPU 和 CPU 的推理。唯一不足就是 ONNX 节点粒度较细，推理速度有时候比其他推理框架如 TensorRT 较低。
2. NCNN是针对手机端的部署。优势是开源较早，有非常稳定的社区，开源影响力也较高。
3. OpenVINO 是 Intel 家出的针对 Intel 出品的 CPU 和 GPU 友好的一款推理框架，同时它也是对接不同训练框架如 TensorFlow，Pytorch，Caffe 等。不足之处可能是只支持 Intel 家的硬件产品。
4. TensorRT 针对 NVIDIA 系列显卡具有其他框架都不具备的优势，如果运行在 NVIDIA 显卡上， TensorRT 一般是所有框架中推理最快的。一般的主流的训练框架如TensorFlow 和 Pytorch 都能转换成 TensorRT 可运行的模型。当然了，TensorRT 的限制就是只能运行在 NVIDIA 显卡上，同时不开源 kernel。
5. MediaPipe 不支持除了tensorflow之外的其他深度学习框架。MediaPipe 的主要用例是使用推理模型和其他可重用组件对应用机器学习管道进行快速原型设计。MediaPipe 还有助于将机器学习技术部署到各种不同硬件平台上的演示和应用程序中，为移动、桌面/云、web和物联网设备构建世界级ML解决方案和应用程序。


### 😆3. AI模型部署平台


AI 模型部署是将训练好的 AI 模型应用到实际场景中的过程。以下是一些常见的 AI 模型部署平台：


1. 云端部署  
 云端部署是最流行的 AI 模型部署方式之一，通常使用云计算平台来托管模型和处理请求。例如，Amazon Web Services (AWS)、Microsoft Azure 和 Google Cloud Platform (GCP) 等云服务提供商都提供了 AI 模型部署解决方案。
2. 边缘设备部署  
 边缘设备部署是将模型部署到 IoT 设备或嵌入式系统等边缘设备上的过程。这种部署方式可以减少延迟和网络带宽消耗，并提高隐私性和安全性。
3. 移动设备部署  
 移动设备部署是将 AI 模型部署到移动设备上的过程，允许设备在本地执行推理而不需要依赖网络连接。这种部署方式对于需要快速响应和保护用户隐私的应用非常有用。
4. 容器化部署  
 容器化部署是将 AI 模型封装到一个轻量级的容器中，然后在不同的环境中进行部署和运行。容器化部署可以提高可移植性和灵活性，并简化部署过程。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ca9b0f5464ec4665a5508738e7e18dee.png)


以上。





