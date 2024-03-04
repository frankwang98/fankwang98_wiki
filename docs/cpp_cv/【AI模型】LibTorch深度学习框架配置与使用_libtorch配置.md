







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍LibTorch深度学习框架配置与使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. LibTorch介绍](#smirk1_LibTorch_7)
	+ [:blush:2. 环境安装与配置](#blush2__25)
	+ [:satisfied:3. 应用示例](#satisfied3__61)




### 😏1. LibTorch介绍


官网：`https://pytorch.org/`


`LibTorch`是PyTorch深度学习框架的C++版本，它提供了用于构建和训练神经网络模型的高级API和工具。LibTorch允许你在离线环境中使用PyTorch模型，而无需依赖Python解释器。


![在这里插入图片描述](https://img-blog.csdnimg.cn/5038bc2e716c48a6893b8c9653373e39.png)


以下是LibTorch的一些主要特点和功能：



> 
> 1.高性能：LibTorch被优化为高性能的C++库，可提供快速且高效的计算能力。它利用了底层的C++实现，可以在支持的硬件上获得最佳的计算性能。
> 
> 
> 



> 
> 2.深度学习支持：LibTorch支持各种深度学习任务，包括图像分类、目标检测、语义分割、机器翻译等。它提供了一系列的预训练模型和工具，方便你进行模型训练与推理。
> 
> 
> 



> 
> 3.跨平台支持：LibTorch可在多个操作系统上运行，包括Windows、Linux和macOS。这使得你可以在不同的设备上进行模型开发和部署，以满足特定的应用需求。
> 
> 
> 



> 
> 4.兼容性：由于LibTorch是基于PyTorch开发的，因此能够与PyTorch代码紧密集成。你可以轻松地在Python和C++之间切换，使用相同的模型、工具和API。
> 
> 
> 



> 
> 5.扩展性：LibTorch支持自定义C++扩展，你可以使用C++编写具有高效计算能力的自定义操作和模块。这使得你可以在深度学习框架中实现更多的自定义功能。
> 
> 
> 


### 😊2. 环境安装与配置


以Ubuntu为例配置LibTorch：



```
# 安装Pytorch cpu版本
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cpu
# 下载LibTorch
wget https://download.pytorch.org/libtorch/nightly/cpu/libtorch-shared-with-deps-latest.zip
unzip libtorch-shared-with-deps-latest.zip
# 配置环境（将PATH路径替换为自己的）
echo "export PATH=/path/to/libtorch:\$PATH" >> ~/.bashrc && source ~/.bashrc

```

CMakeList.txt构建示例：



```
cmake_minimum_required(VERSION 3.21)
project(HelloWorld)

# LibTorch需c++17支持
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE\_CXX\_FLAGS} -std=c++17")

find_package(Torch REQUIRED)
add_executable(HelloWorld HelloWorld.cpp)
target_link_libraries(HelloWorld $TORCH\_LIBRARIES)
#target\_link\_libraries(HelloWorld ${TORCH\_LIBRARIES}) # torchscript支持

```

构建编译示例：



```
mkdir build && cd build
cmake .. 
make
./HelloWorld（xxx）

```

### 😆3. 应用示例


有一个不错的LibTorch学习Github仓库推荐：`https://github.com/clearhanhui/LearnLibTorch`


Libtorch（c++）很多方面与Pytorch（python）用法基本一致，适合Pytorch的同学来转。


创建张量tensor示例：



```
#include <iostream>
#include <torch/torch.h>

int main() {
    // 创建一个(2,3)张量
    torch::Tensor tensor = torch::zeros({2, 3});
    std::cout << tensor << std::endl;
    std::cout << "Welcome to LibTorch" << std::endl;
    
    return 0;
}

```

TorchScript可对python定义的PyTorch模型进行序列化，并在c++中加载运行，示例：



```
#include <iostream>
#include <torch/script.h>
#include <torch/torch.h>
#include <vector>

int main() {
  // 模型路径
  std::string module_path = "../xxx.pt";

  // 加载模型
  torch::jit::script::Module module;
  try {
    module = torch::jit::load(module_path);
  } catch (const c10::Error &e) {
    std::cout << "error loading the model\n";
    return -1;
  }

  std::vector<torch::jit::IValue> x;
  x.push\_back(torch::ones({1, 1, 28, 28}));

  at::Tensor output = module.forward(x).toTensor();
  std::cout << output.sum() << std::endl;

  return 0;
}

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





