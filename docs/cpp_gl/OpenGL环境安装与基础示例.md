> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍计算机图形学OpenGL基础及环境配置。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. OpenGL介绍

`OpenGL`（Open Graphics Library）是一个跨平台的图形编程接口，用于渲染2D和3D图形。它提供了一组函数和命令，允许开发者通过编程方式控制图形硬件，从而创建高性能的图形应用程序。

以下是一些`OpenGL`的特点：

> 1.跨平台：OpenGL 是跨平台的，可以在各种操作系统和设备上运行，包括 Windows、Linux、Mac、iOS 和 Android 等。

> 2.低级别接口：OpenGL 是一个底层的图形编程接口，提供了对图形硬件的直接访问。它允许开发者直接操作图形渲染管线，控制顶点和像素的处理过程。

> 3.状态机：OpenGL 是基于状态机的编程模型。开发者通过设置不同的状态（例如颜色、材质、光照等），然后调用相应的绘制命令，来渲染图形对象。

> 4.二维和三维图形：OpenGL 支持绘制和处理2D和3D图形。它提供了基本的几何图元（如点、线、三角形），以及矩阵变换和投影等功能，使开发者能够创建复杂的图形场景。

> 5.着色器编程：OpenGL 使用着色器编程来控制图形渲染过程。着色器是运行在图形硬件上的小型程序，用于处理顶点和像素的计算和变换。开发者可以使用 GLSL（OpenGL Shading Language）编写自定义的着色器程序。

> 6.扩展性：OpenGL 具有可扩展性，支持各种扩展功能和特性。开发者可以利用扩展来实现更高级的图形效果和功能，满足特定的应用需求。

`OpenGL` 在游戏开发、计算机图形学、科学可视化、虚拟现实（VR）等领域得到广泛应用。它提供了强大的图形处理能力，允许开发者创建出具有高度交互性和视觉效果的应用程序。

官网：`https://opengl.org/`

学习网站：`https://learnopengl-cn.github.io/`

OpenGL最流行的几个库有GLUT、SDL、SFML、Vulkan和GLFW等，常见的搭配有glfw+glad+glm，下面主要用GLFW。

#### OpenGL基础


由于OpenGL是一个图形API，并不是一个独立的平台，它需要一个编程语言来工作，在这里我们使用的是C++。并不需要你是一个C++专家，但至少能写出比一个“Hello World”复杂的程序。


除此之外，我们也将用到一些数学知识（线性代数、几何、三角学），但大部分的功能甚至都不需要你理解这些数学知识，只要你会使用就行。（数学与程序设计的关系）


`OpenGL`一般被认为是一个API，包含了一系列可以操作图形、图像的函数。然而，OpenGL本身并不是一个API，它仅仅是一个由Khronos组织制定并维护的规范(Specification)。


OpenGL规范严格规定了每个函数该如何执行，以及它们的输出值。至于内部具体每个函数是如何实现(Implement)的，将由OpenGL库的开发者自行决定（实际的OpenGL库的开发者通常是显卡的生产商）。


**立即渲染模式与核心模式**  
 早期的OpenGL使用立即渲染模式（Immediate mode），这个模式下绘制图形很方便。OpenGL的大多数功能都被库隐藏起来，开发者很少有控制OpenGL如何进行计算的自由。在这种情况下，从OpenGL3.2开始，规范文档开始废弃立即渲染模式，并鼓励开发者在OpenGL的核心模式(Core-profile)下进行开发。


当使用OpenGL的核心模式时，OpenGL迫使我们使用现代的函数。现代函数要求使用者真正理解OpenGL和图形编程，它有一些难度，然而提供了更多的灵活性，更高的效率，更重要的是可以更深入的理解图形编程。


使用扩展的代码大多看上去如下：



```cpp
if(GL_ARB_extension_name)
{
    // 使用硬件支持的全新的现代特性
}
else
{
    // 不支持此扩展: 用旧的方式去做
}

```

**状态机**  
 OpenGL自身是一个巨大的状态机(State Machine)：一系列的变量描述OpenGL此刻应当如何运行。OpenGL的状态通常被称为OpenGL上下文(Context)。我们通常使用如下途径去更改OpenGL状态：设置选项，操作缓冲。最后，我们使用当前OpenGL上下文来渲染。


**对象**  
 OpenGL库是用C语言写的，同时也支持多种语言的派生，但其内核仍是一个C库。由于C的一些语言结构不易被翻译到其它的高级语言，因此OpenGL开发的时候引入了一些抽象层。“对象(Object)”就是其中一个。


在OpenGL中一个对象是指一些选项的集合，它代表OpenGL状态的一个子集。比如，我们可以用一个对象来代表绘图窗口的设置，之后我们就可以设置它的大小、支持的颜色位数等等。可以把对象看做一个C风格的结构体(Struct)：



```cpp
struct object\_name {
    float  option1;
    int    option2;
    char[] name;
};

```

**OpenGL函数库**  
 OpenGL相关的函数库有核心库（gl），实用库（glu），辅助库（aux）、实用工具库（glut），窗口库（glx、agl、wgl）和扩展函数库等。gl是核心，glu是对gl的部分封装。glx、agl、wgl 是针对不同窗口系统的函数。glut是为跨平台的OpenGL程序的工具包。扩展函数库是硬件厂商为实现硬件更新利用OpenGL的扩展机制开发的函数。


窗口管理

> GLUT：glut或者freegult主要是OpenGL 1.0的基本函数功能，前面几节主要用的这个库。  
> GLFW：glfw的开发目的是用于替代glut的。它是一个轻量级的，开源的，跨平台的library。中文学习网站用的这个库。

函数加载

> GLEW：glew是使用OpenGL 2.0之后的一个工具函数。中文学习网站用的这个库。  
> GLAD：glad是继gl3w，glew之后，当前最新的用来访问OpenGL规范接口的第三方库。简单说glad是glew的升级版。

部件工具

> Qt  
> wxWidgets  
> Imgui



了解了OpenGL的基础知识后，下面就开始创建一些很酷的图形吧。


### 😊2. 环境安装与配置


主要包括glfw、glad、imgui等库，包含vs、cmake配置。


#### windows+vs+msvc


Windows + Visual Studio 2017  
 可以通过安装`nupengl`程序包的方式。首先，新建一个VS空项目，我这里命名opengl\_demo，然后打开项目->管理NuGet程序包，搜索nupengl，安装`nupengl.core`程序包即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/505f916b88a44a62ad4a344859f3bdd5.png)


在我们画出出色的效果之前，首先要做的就是创建一个OpenGL上下文(Context)和一个用于显示的窗口。


`GLFW`是一个专门针对OpenGL的C语言库，它提供了一些渲染物体所需的最低限度的接口。中文学习网是用源码编译的，包括如何获取、编译、链接GLFW库，这里我用的二进制包，对于初学者来说可以更快的验证。


与之前配置nupengl程序包一样，先打开管理程序包，安装glfw：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/750226b0cbd04d7f873c8eec645bad2d.png)


`GLAD`是一个开源的库，它能解决一些繁琐的问题。GLAD的配置与大多数的开源库有些许的不同，是采用在线服务的。打开这个网站：`https://glad.dav1d.de/`


将语言(Language)设置为`C/C++`，在API选项中，选择`3.3以上的OpenGL(gl)`版本（我们的教程中将使用3.3版本，但更新的版本也能用）。之后将模式(Profile)设置为`Core`，并且保证选中了生成加载器(`Generate a loader)`选项。现在可以先（暂时）忽略扩展(Extensions)中的内容。都选择完之后，点击生成(`Generate`)按钮来生成库文件。


`GLAD`现在应该提供给你了一个zip压缩文件，包含两个头文件目录，和一个`glad.c`文件。将两个头文件目录（`glad`和`KHR`）复制到你的`Include`文件夹中（并在工程中将include添加到包含目录），并添加glad.c文件到你的工程中。


#### windows+clion+cmake


下载好`glfw`的二进制包，并生成`glad`文件后，开始cmake配置。然后可以新建一个`env`的环境目录，将库相关的头文件和dll放在环境目录里，如：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/755f2527f97d4d8db4bef6e024998beb.png)



```bash
# CMakeLists.txt示例
cmake_minimum_required(VERSION 3.19)
project(opengl_demo)

set(CMAKE_CXX_STANDARD 14)

include_directories("./env/include")
link_directories("./env/lib")

add_executable(opengl_demo main.cpp glad.c)
target_link_libraries(opengl_demo glfw3.dll)

```

然后就可以正常编译了。


#### ubuntu+cmake


待补充


### 😆3. 应用示例


下面就放一个学习网的创建窗口的简单示例，可以测试环境是否安装成功：



```cpp
#include <glad/glad.h>
#include <GLFW/glfw3.h>

#include <iostream>

void framebuffer\_size\_callback(GLFWwindow\* window, int width, int height);
void processInput(GLFWwindow \*window);

// settings 全局变量
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    // glfw: initialize and configure
    // ------------------------------
    glfwInit();		//初始化GLFW
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef \_\_APPLE\_\_
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    // glfw window creation
    // --------------------
    GLFWwindow\* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);	//窗口对象
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    // glad: load all OpenGL function pointers 初始化glad
    // ---------------------------------------
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }

    // render loop 渲染循环
    // -----------
    while (!glfwWindowShouldClose(window))
    {
        // input
        // -----
        processInput(window); //调用输入控制

        // render
        // ------
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // glfw: swap buffers and poll IO events (keys pressed/released, mouse moved etc.)
        // -------------------------------------------------------------------------------
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw: terminate, clearing all previously allocated GLFW resources. 释放资源
    // ------------------------------------------------------------------
    glfwTerminate();
    return 0;
}

// process all input: query GLFW whether relevant keys are pressed/released this frame and react accordingly 输入控制
// ---------------------------------------------------------------------------------------------------------
void processInput(GLFWwindow \*window)
{
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw: whenever the window size changed (by OS or user resize) this callback function executes 视窗大小
// ---------------------------------------------------------------------------------------------
void framebuffer\_size\_callback(GLFWwindow\* window, int width, int height)
{
    // make sure the viewport matches the new window dimensions; note that width and
    // height will be significantly larger than specified on retina displays.
    glViewport(0, 0, width, height);
}

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





