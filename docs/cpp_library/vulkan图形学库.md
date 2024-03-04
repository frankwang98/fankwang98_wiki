> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍计算机图形学Vulkan基础与环境配置。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. 项目介绍

官网：`https://www.vulkan.org/`

`Vulkan` 是一种跨平台的图形和计算 API（应用程序接口），旨在提供高性能的图形渲染和计算功能。它由`Khronos Group`开发，作为`OpenGL`的继任者，设计用于取代OpenGL并提供更好的性能、更低的驱动开销和更大的可编程性。

以下是 `Vulkan` 的一些重要特点和优势：

> 1.低开销和高性能: Vulkan 通过最小化驱动开销和提供更多底层控制来实现高性能。它允许开发人员直接管理显卡资源，提供了更多的优化和调优选项，以最大限度地发挥硬件的性能潜力。

> 2.多线程和并行计算: Vulkan 提供了对多线程和并行计算的更好支持。它允许开发人员在多个线程中并行处理渲染和计算任务，以提高性能和利用现代多核处理器的能力。

> 3.交叉平台: Vulkan 被设计为跨平台的图形和计算 API。它可以在多种操作系统上运行，包括Windows、Linux、Android和iOS等。这使得开发人员可以使用相同的代码库在不同的平台上构建和部署游戏和图形应用程序。

> 4.低级别控制和可编程性: Vulkan 提供了更低级别的硬件访问和控制，使开发人员能够更深入地管理渲染和计算管线。它支持着色器编程，允许开发人员使用自定义的着色器程序来实现高度可编程的图形效果。

> 5.更好的内存管理: Vulkan 提供了更灵活的内存管理机制，允许开发人员更精细地控制图形和计算资源的分配和使用。这有助于减少内存碎片化并提高应用程序的性能和效率。

> 6.后向兼容性: Vulkan 设计时考虑了向后兼容性，使得旧版本的 Vulkan 应用程序能够在新版本的 Vulkan 实现上运行，而不需要进行大规模的代码修改。

### 😊2. 环境配置

下面进行环境配置：

#### Ubuntu

```bash
# 安装vulkan开发及工具包
sudo apt install vulkan-utils libvulkan-dev
vulkaninfo # 验证安装

# g++编译
g++ -o main main.cpp -lvulkan

```

#### Windows

这里我用的`clion+mingw`环境。

```bash
# 下载mingw编译好的vulkan库
https://packages.msys2.org/package/mingw-w64-x86_64-vulkan-loader?repo=mingw64
# 下载vulkan头文件
https://github.com/KhronosGroup/Vulkan-Headers/releases/tag/v1.3.276

```

`CMakeLists.txt`示例：

```bash
cmake_minimum_required(VERSION 3.19)
project(clion)

set(CMAKE_CXX_STANDARD 11)

include_directories("D:/develop/vulkan\_132/header/include")
link_directories("D:/develop/vulkan\_132/mingw64/lib")

add_executable(clion main.cpp)

target_link_libraries(clion vulkan-1)

```

### 😆3. 使用说明

vulkan基础示例：

```cpp
#include <iostream>
#include <vulkan/vulkan.h>

int main() {
    VkInstance instance;
    VkInstanceCreateInfo createInfo = {};
    createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;

    VkResult result = vkCreateInstance(&createInfo, nullptr, &instance);
    if (result != VK_SUCCESS) {
        std::cout << "Failed to create Vulkan instance." << std::endl;
        return 1;
    }

    uint32\_t extensionCount = 0;
    vkEnumerateInstanceExtensionProperties(nullptr, &extensionCount, nullptr);
    std::cout << "Number of supported extensions: " << extensionCount << std::endl;

    vkDestroyInstance(instance, nullptr);

    return 0;
}

```

以上。





