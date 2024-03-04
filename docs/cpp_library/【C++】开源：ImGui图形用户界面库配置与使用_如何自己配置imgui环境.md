







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ImGui图形用户界面库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__74)




### 😏1. 项目介绍


项目Github地址：`https://github.com/ocornut/imgui`


`Dear ImGui` (`ImGui`) 是一个开源的、用 C++ 编写的图形用户界面（GUI）库。它由OCornut创建，旨在为应用程序和工具提供创建用户界面的简单高效的方式。


以下是 `Dear ImGui` 的一些主要特性和特点：



> 
> 1.即时模式 GUI：ImGui 遵循即时模式 GUI 的范例，用户界面不是通过保留模式或对象层次结构构建的。相反，每一帧都需要重新创建和绘制用户界面。这种设计使得创建和更新界面变得非常灵活和直观。
> 
> 
> 



> 
> 2.轻量级和可嵌入性：ImGui 是一个轻量级库，只有几个文件组成，可轻松嵌入到现有项目中。它没有任何外部依赖，使得集成和部署变得非常简单。
> 
> 
> 



> 
> 3.跨平台支持：ImGui 可以在多个平台上运行，包括 Windows、MacOS、Linux 和其他一些操作系统。它提供了与底层图形 API（如OpenGL、DirectX）的集成，以便在不同平台上绘制用户界面。
> 
> 
> 



> 
> 4.简单易用的 API：ImGui 提供了一个简单直观的 API，使得创建用户界面变得非常容易。您可以使用各种控件（如按钮、文本框、滑块等）来构建界面，并通过监听用户输入和响应事件来实现交互。
> 
> 
> 



> 
> 5.自定义性强：ImGui 具有强大的自定义能力，您可以自定义主题、样式和控件外观以满足您的需求。此外，您还可以编写自定义的渲染器，以实现与不同图形 API 的集成。
> 
> 
> 


`Dear ImGui` 是一个简单、灵活且强大的 GUI 库，适用于各种应用程序和工具的用户界面开发。无论是创建原型、调试工具还是构建实际应用程序，它都提供了一套方便的工具和框架来简化界面开发过程。


### 😊2. 环境配置


下面进行环境配置：



```
# windows vs
# windows端需要预装directx，VS的Kit中默认会有
# 源码中的example下有示例VS工程(.sln)，下载源码后直接用VS打开运行

```


```
# windows clion
# CMakeLists.txt示例，提前装好了glfw+glad+freeglut+opencv
cmake_minimum_required(VERSION 3.19)
project(opengl_demo)

set(CMAKE_CXX_STANDARD 14)

include_directories("./env/include")
link_directories("./env/lib")
include_directories("./env/imgui")
include_directories("./env/imgui/backends")

set(OpenCV_DIR "D:/develop/opencv341\_mingw/x64/mingw/lib")
find_package(OpenCV 3 REQUIRED)
MESSAGE("OpenCV version : ${OpenCV\_VERSION}")
include_directories(${OpenCV\_INCLUDE\_DIRS})
link_directories(${OpenCV\_LIBRARIES}) # ${OpenCV\_LIB\_DIR}

file(GLOB IMGUI "./env/imgui/\*.cpp") # 1.80

add_executable(opengl_demo main.cpp env/glad.c ${IMGUI})
target_link_libraries(opengl_demo
        glfw3 # 3.3.9
        opengl32
        freeglut # 3.4.0
        glu32
        ${OpenCV\_LIBRARIES}
        xinput
)

```


```
# ubuntu cmake
sudo apt install libglfw3 libglfw3-dev # 安装glfw
# 一个博主已经写了一个基于cmake的示例，这里引用一下 http://t.csdnimg.cn/LDY5H
https://github.com/tashaxing/imgui_cmake_starter
# 包含了imgui 1.83的源码和示例程序，直接编译即可

```

### 😆3. 使用说明


运行示例：


windows VS直接生成运行即可，ubuntu下cmake编译指令如下：



```
mkdir build && cd build
cmake ..
make
./imgui_cmake_starter

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/513f9bc1c95843f09cf1751af8eb5053.png)


Imgui的示例代码：



```
#include "imgui.h"
#include "imgui\_impl\_glfw.h"
#include "imgui\_impl\_opengl3.h"
#include <iostream>

#if defined(IMGUI\_IMPL\_OPENGL\_ES2)
#include <GLES2/gl2.h>
// About Desktop OpenGL function loaders:
// Modern desktop OpenGL doesn't have a standard portable header file to load OpenGL function pointers.
// Helper libraries are often used for this purpose! Here we are supporting a few common ones (gl3w, glew, glad).
// You may use another loader/header of your choice (glext, glLoadGen, etc.), or chose to manually implement your own.
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GL3W)
#include <GL/gl3w.h> // Initialize with gl3wInit()
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLEW)
#include <GL/glew.h> // Initialize with glewInit()
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLAD)
#include <glad/glad.h> // Initialize with gladLoadGL()
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLAD2)
#include <glad/gl.h> // Initialize with gladLoadGL(...) or gladLoaderLoadGL()
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLBINDING2)
#define GLFW\_INCLUDE\_NONE // GLFW including OpenGL headers causes ambiguity or multiple definition errors.
#include <glbinding/Binding.h> // Initialize with glbinding::Binding::initialize()
#include <glbinding/gl/gl.h>
using namespace gl;
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLBINDING3)
#define GLFW\_INCLUDE\_NONE // GLFW including OpenGL headers causes ambiguity or multiple definition errors.
#include <glbinding/glbinding.h>// Initialize with glbinding::initialize()
#include <glbinding/gl/gl.h>
using namespace gl;
#else
#include IMGUI\_IMPL\_OPENGL\_LOADER\_CUSTOM
#endif

// Include glfw3.h after our OpenGL definitions
#include <GLFW/glfw3.h>

// [Win32] Our example includes a copy of glfw3.lib pre-compiled with VS2010 to maximize ease of testing and compatibility with old VS compilers.
// To link with VS2010-era libraries, VS2015+ requires linking with legacy\_stdio\_definitions.lib, which we do using this pragma.
// Your own project should not be affected, as you are likely to link with a newer binary of GLFW that is adequate for your version of Visual Studio.
#if defined(\_MSC\_VER) && (\_MSC\_VER >= 1900) && !defined(IMGUI\_DISABLE\_WIN32\_FUNCTIONS)
#pragma comment(lib, "legacy\_stdio\_definitions")
#endif

static void glfw\_error\_callback(int error, const char\* description)
{
    fprintf(stderr, "Glfw Error %d: %s\n", error, description);
}

int main(int argc, char\*\* argv)
{
    // Setup window
    glfwSetErrorCallback(glfw_error_callback);
    if (!glfwInit())
        return 1;

    // Decide GL+GLSL versions
#if defined(IMGUI\_IMPL\_OPENGL\_ES2)
    // GL ES 2.0 + GLSL 100
    const char\* glsl_version = "#version 100";
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 2);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 0);
    glfwWindowHint(GLFW_CLIENT_API, GLFW_OPENGL_ES_API);
#elif defined(\_\_APPLE\_\_)
    // GL 3.2 + GLSL 150
    const char\* glsl_version = "#version 150";
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 2);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);  // 3.2+ only
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);            // Required on Mac
#else
    // GL 3.0 + GLSL 130
    const char\* glsl_version = "#version 130";
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 0);
    //glfwWindowHint(GLFW\_OPENGL\_PROFILE, GLFW\_OPENGL\_CORE\_PROFILE); // 3.2+ only
    //glfwWindowHint(GLFW\_OPENGL\_FORWARD\_COMPAT, GL\_TRUE); // 3.0+ only
#endif

    // Create window with graphics context
    GLFWwindow\* window = glfwCreateWindow(1280, 720, "Dear ImGui GLFW+OpenGL3 example", NULL, NULL);
    if (window == NULL)
        return 1;
    glfwMakeContextCurrent(window);
    glfwSwapInterval(1); // Enable vsync

    // Initialize OpenGL loader
#if defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GL3W)
    bool err = gl3wInit() != 0;
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLEW)
    bool err = glewInit() != GLEW_OK;
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLAD)
    bool err = gladLoadGL() == 0;
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLAD2)
    bool err = gladLoadGL(glfwGetProcAddress) == 0; // glad2 recommend using the windowing library loader instead of the (optionally) bundled one.
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLBINDING2)
    bool err = false;
    glbinding::Binding::initialize();
#elif defined(IMGUI\_IMPL\_OPENGL\_LOADER\_GLBINDING3)
    bool err = false;
    glbinding::initialize([](const char\* name) { return (glbinding::ProcAddress)glfwGetProcAddress(name); });
#else
    bool err = false; // If you use IMGUI\_IMPL\_OPENGL\_LOADER\_CUSTOM, your loader is likely to requires some form of initialization.
#endif
    if (err)
    {
        fprintf(stderr, "Failed to initialize OpenGL loader!\n");
        return 1;
    }

    // Setup Dear ImGui context
    IMGUI\_CHECKVERSION();
    ImGui::CreateContext();
    ImGuiIO& io = ImGui::GetIO(); (void)io;
    //io.ConfigFlags |= ImGuiConfigFlags\_NavEnableKeyboard; // Enable Keyboard Controls
    //io.ConfigFlags |= ImGuiConfigFlags\_NavEnableGamepad; // Enable Gamepad Controls

    // Setup Dear ImGui style
    ImGui::StyleColorsDark();
    //ImGui::StyleColorsClassic();

    // Setup Platform/Renderer backends
    ImGui\_ImplGlfw\_InitForOpenGL(window, true);
    ImGui\_ImplOpenGL3\_Init(glsl_version);

    // Load Fonts
    // - If no fonts are loaded, dear imgui will use the default font. You can also load multiple fonts and use ImGui::PushFont()/PopFont() to select them.
    // - AddFontFromFileTTF() will return the ImFont\* so you can store it if you need to select the font among multiple.
    // - If the file cannot be loaded, the function will return NULL. Please handle those errors in your application (e.g. use an assertion, or display an error and quit).
    // - The fonts will be rasterized at a given size (w/ oversampling) and stored into a texture when calling ImFontAtlas::Build()/GetTexDataAsXXXX(), which ImGui\_ImplXXXX\_NewFrame below will call.
    // - Read 'docs/FONTS.md' for more instructions and details.
    // - Remember that in C/C++ if you want to include a backslash \ in a string literal you need to write a double backslash \\ !
    //io.Fonts->AddFontDefault();
    //io.Fonts->AddFontFromFileTTF("../../misc/fonts/Roboto-Medium.ttf", 16.0f);
    //io.Fonts->AddFontFromFileTTF("../../misc/fonts/Cousine-Regular.ttf", 15.0f);
    //io.Fonts->AddFontFromFileTTF("../../misc/fonts/DroidSans.ttf", 16.0f);
    //io.Fonts->AddFontFromFileTTF("../../misc/fonts/ProggyTiny.ttf", 10.0f);
    //ImFont\* font = io.Fonts->AddFontFromFileTTF("c:\\Windows\\Fonts\\ArialUni.ttf", 18.0f, NULL, io.Fonts->GetGlyphRangesJapanese());
    //IM\_ASSERT(font != NULL);

    // Our state
    bool show_demo_window = true;
    bool show_another_window = false;
    ImVec4 clear_color = ImVec4(0.45f, 0.55f, 0.60f, 1.00f);

    // Main loop
    while (!glfwWindowShouldClose(window))
    {
        // Poll and handle events (inputs, window resize, etc.)
        // You can read the io.WantCaptureMouse, io.WantCaptureKeyboard flags to tell if dear imgui wants to use your inputs.
        // - When io.WantCaptureMouse is true, do not dispatch mouse input data to your main application.
        // - When io.WantCaptureKeyboard is true, do not dispatch keyboard input data to your main application.
        // Generally you may always pass all inputs to dear imgui, and hide them from your application based on those two flags.
        glfwPollEvents();

        // Start the Dear ImGui frame
        ImGui\_ImplOpenGL3\_NewFrame();
        ImGui\_ImplGlfw\_NewFrame();
        ImGui::NewFrame();

        // 1. Show the big demo window (Most of the sample code is in ImGui::ShowDemoWindow()! You can browse its code to learn more about Dear ImGui!).
        if (show_demo_window)
            ImGui::ShowDemoWindow(&show_demo_window);

        // 2. Show a simple window that we create ourselves. We use a Begin/End pair to created a named window.
        {
            static float f = 0.0f;
            static int counter = 0;

            ImGui::Begin("Hello, world!");                          // Create a window called "Hello, world!" and append into it.

            ImGui::Text("This is some useful text.");               // Display some text (you can use a format strings too)
            ImGui::Checkbox("Demo Window", &show_demo_window);      // Edit bools storing our window open/close state
            ImGui::Checkbox("Another Window", &show_another_window);

            ImGui::SliderFloat("float", &f, 0.0f, 1.0f);            // Edit 1 float using a slider from 0.0f to 1.0f
            ImGui::ColorEdit3("clear color", (float\*)&clear_color); // Edit 3 floats representing a color

            if (ImGui::Button("Button"))                            // Buttons return true when clicked (most widgets return true when edited/activated)
                counter++;
            ImGui::SameLine();
            ImGui::Text("counter = %d", counter);

            ImGui::Text("Application average %.3f ms/frame (%.1f FPS)", 1000.0f / ImGui::GetIO().Framerate, ImGui::GetIO().Framerate);
            ImGui::End();
        }

        // 3. Show another simple window.
        if (show_another_window)
        {
            ImGui::Begin("Another Window", &show_another_window);   // Pass a pointer to our bool variable (the window will have a closing button that will clear the bool when clicked)
            ImGui::Text("Hello from another window!");
            if (ImGui::Button("Close Me"))
                show_another_window = false;
            ImGui::End();
        }

        // Rendering
        ImGui::Render();
        int display_w, display_h;
        glfwGetFramebufferSize(window, &display_w, &display_h);
        glViewport(0, 0, display_w, display_h);
        glClearColor(clear_color.x \* clear_color.w, clear_color.y \* clear_color.w, clear_color.z \* clear_color.w, clear_color.w);
        glClear(GL_COLOR_BUFFER_BIT);
        ImGui\_ImplOpenGL3\_RenderDrawData(ImGui::GetDrawData());

        glfwSwapBuffers(window);
    }

    // Cleanup
    ImGui\_ImplOpenGL3\_Shutdown();
    ImGui\_ImplGlfw\_Shutdown();
    ImGui::DestroyContext();

    glfwDestroyWindow(window);
    glfwTerminate();

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





