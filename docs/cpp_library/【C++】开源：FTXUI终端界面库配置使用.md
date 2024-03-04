







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍FTXUI终端界面库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__76)




### 😏1. 项目介绍


官网：`https://arthursonzogni.github.io/FTXUI/index.html`


项目Github地址：`https://github.com/ArthurSonzogni/FTXUI`


`FTXUI`是一个开源的C++库，用于创建终端用户界面（TUI）。它旨在提供简单、现代和易用的界面设计，使开发者能够快速构建漂亮和交互性强的终端应用程序。


以下是`FTXUI`库的一些主要特点和功能：



> 
> 1.界面元素：FTXUI提供了一系列可用的界面元素，如文本标签、按钮、复选框、文本输入框、表格等。这些元素可以方便地组合和布局，使开发者能够创建复杂的用户界面。
> 
> 
> 



> 
> 2.布局和渲染：FTXUI具有灵活的布局和渲染功能，包括弹性布局（flexbox）和网格布局。开发者可以使用这些布局来定义界面元素的位置和大小，并根据需要自动调整布局。
> 
> 
> 



> 
> 3.交互性：FTXUI支持处理键盘和鼠标事件，以及捕获用户的输入。开发者可以轻松地为界面元素添加事件处理程序，以响应用户的操作和输入。
> 
> 
> 



> 
> 4.颜色和样式：FTXUI提供了丰富的颜色和样式选项，使开发者能够创建具有吸引力和个性化的界面。开发者可以自定义元素的背景色、前景色、边框样式等。
> 
> 
> 



> 
> 5.跨平台支持：FTXUI可以在多个平台上运行，包括Linux、macOS和Windows等。它使用了跨平台的终端库底层，以便在不同的操作系统上提供一致的体验。
> 
> 
> 



> 
> 6.简洁的API：FTXUI的API设计简洁、直观，易于使用和理解。它提供了一组简单的函数和类来创建和操作界面元素，使开发过程更加高效和愉悦。
> 
> 
> 


### 😊2. 环境配置



```
# 源码编译
git clone https://github.com/ArthurSonzogni/FTXUI
mkdir build && cd build
cmake ..
make
sudo make install # 是否安装到系统目录
# 如果要编译示例，将cmakelists里这一行改为ON
option(FTXUI_BUILD_EXAMPLES "Set to ON to build examples" ON)

```


```
# 程序编译参照官方的cmake示例
cmake_minimum_required (VERSION 3.11)
 
# --- Fetch FTXUI --------------------------------------------------------------
include(FetchContent)
 
set(FETCHCONTENT_UPDATES_DISCONNECTED TRUE)
FetchContent_Declare(ftxui
  GIT_REPOSITORY https://github.com/ArthurSonzogni/ftxui
  # Important: Specify a GIT\_TAG XXXXX here.
)
 
FetchContent_GetProperties(ftxui)
if(NOT ftxui_POPULATED)
  FetchContent_Populate(ftxui)
  add_subdirectory(${ftxui\_SOURCE\_DIR} ${ftxui\_BINARY\_DIR} EXCLUDE_FROM_ALL)
endif()
 
# ------------------------------------------------------------------------------
 
project(ftxui-starter
  LANGUAGES CXX
  VERSION 1.0.0
)
 
add_executable(ftxui-starter src/main.cpp)
target_include_directories(ftxui-starter PRIVATE src)
 
target_link_libraries(ftxui-starter
  PRIVATE ftxui::screen
  PRIVATE ftxui::dom
  PRIVATE ftxui::component # Not needed for this example.
)

```

### 😆3. 使用说明


这个库提供了许多示例，cmake编译示例后，在`build/examples/component`目录可以运行示例查看：



```
./ftxui_example_button
./ftxui_example_print_key_press
./ftxui_example_window
./ftxui_example_input
...

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/124949cc449341518513c060b6231f84.png)


最简示例：



```
#include <ftxui/dom/elements.hpp>
#include <ftxui/screen/screen.hpp>
#include <iostream>
 
int main(void) {
  using namespace ftxui;
 
  // Define the document
  Element document =
    hbox({
      text("left")   | border,
      text("middle") | border | flex,
      text("right")  | border,
    });
 
  auto screen = Screen::Create(
    Dimension::Full(),       // Width
    Dimension::Fit(document) // Height
  );
  Render(screen, document);
  screen.Print();
 
  return EXIT_SUCCESS;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





