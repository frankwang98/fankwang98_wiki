> 大家平时玩ROS都是在Ubuntu系统上，那Windows系统可以安装吗，答案是：可以的！Windows为了发展自家的物联网生态，已经在Windows系统支持ROS了。

### 1.安装VS 2017

微软家的开发离不开VS，所以大家自行安装就好了。

VS 2017地址：`https://visualstudio.microsoft.com/zh-hans/thank-you-downloading-visual-studio/?sku=Community&rel=15`

进入选择安装的界面，注意勾选C++支持。安装完会有下面这个东西，VS下的命令行工具。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c5d09b0c57b04069987857806fc97eeb.png)

### 2.安装Chocolatey & Git

`Chocolatey`是Windows下的包管理工具，相当于Ubuntu中的apt-get，方便后续安装各种软件包。

在VS命令行中复制下面的命令安装：

```bash
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

```

安装完之后是这样：


![在这里插入图片描述](https://img-blog.csdnimg.cn/66084f6ad0c844329ffafcfb8f18d257.png)


比如，安装git可以这样：

```bash
choco install git -y

```

### 3.安装ROS


与Ubuntu安装ROS类似，都是要添加一个软件源，然后执行安装命令，如下：



```bash
choco source add -n=ros-win -s="https://roswin.azurewebsites.net/api/v2" --priority=1
choco upgrade ros-melodic-desktop -y

```

![请添加图片描述](https://img-blog.csdnimg.cn/2df9b9935062401c85cf49509b07e84a.png)


### 4.运行ROS例程


在Windows安装好ROS后，需要先打开VS命令行：


![在这里插入图片描述](https://img-blog.csdnimg.cn/bccfc6aa914a49759afaec0be99dad79.png)


然后进入ros的安装目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f0f98b76af514a80b229c6fd7f2d926f.png)


生成环境变量后，才能执行相关操作。


![在这里插入图片描述](https://img-blog.csdnimg.cn/2a609482d85745d18c672d7807ba2728.png)


是不是觉得会觉得很麻烦，每次都要这样重复操作，不怕，bat脚本帮你解决这个问题：


创建一个`ros.bat`的文件，输入：

```bash
@echo off

C:

cd C:\opt\ros\melodic\x64

C:\Windows\System32\cmd.exe /k "D:\Visual Studio\2017\Enterprise\Common7\Tools\VsDevCmd.bat" -arch=amd64 -host_arch=amd64

```

然后就可以一键启动脚本，进入之后，再执行一下setup.bat就好了。



```bash
## 终端1
setup.bat
roscore

## 终端2
setup.bat
rosrun turtlesim turtlesim_node

## 终端3
setup.bat
rosrun turtlesim turtle_teleop_key

```

![请添加图片描述](https://img-blog.csdnimg.cn/48d5c62b22d947c1b32ecb5a0366c639.png)


以上。


> Qt Creator是一个比较好的工程编辑器，通过安装ros插件使得可以在Qt中创建ros工程，免去了很多繁琐步骤。

### 1. qt-ros介绍

Qt-ROS 是将 Qt 框架与 ROS（机器人操作系统）结合使用的工具。Qt 是一个跨平台的应用程序开发框架，提供了丰富的图形界面和功能库，用于创建直观而强大的用户界面和应用程序。ROS 是一个开源的软件框架，用于构建机器人系统，并提供了一系列工具、库和规范，用于机器人软件开发。

Qt-ROS 的主要目的是为开发机器人应用程序提供一个强大的图形界面和用户交互能力，并与 ROS 的功能无缝集成。它允许开发者使用 Qt 的丰富工具和库来创建直观的用户界面，并使用 ROS 提供的底层功能来操作和控制机器人系统。

Qt-ROS 提供了一些重要的功能和特性：

> 1.可视化界面：使用 Qt-ROS，开发者可以轻松地创建具有丰富图形界面的机器人应用程序。Qt 提供了一系列强大的 UI 控件和布局管理器，使用户界面的设计和开发变得简单和灵活。

> 2.ROS 集成：Qt-ROS 提供了与 ROS 的无缝集成。它允许开发者使用 ROS 的功能，如话题（Topic）和服务（Service），通过 Qt 提供的接口进行通信和交互。这样，开发者可以在 Qt 应用程序中直接使用 ROS 的功能，如传感器数据获取、控制命令发送等。

> 3.跨平台支持：Qt-ROS 建立在 Qt 框架之上，因此享受到了 Qt 的跨平台特性。开发的应用程序可以在多个操作系统上运行，包括 Windows、Linux 和 macOS。

> 4.插件支持：Qt-ROS 提供了插件机制，允许开发者扩展和定制其功能。用户可以根据需要添加自定义插件，以满足特定的应用程序要求。

Qt-ROS 的组合能力使得机器人软件开发更加方便和高效。

请提前安装好Linux版本`Qt`和`ROS`。


### 2. 安装qt-ros插件


下载qt-ros在线安装程序或脱机安装程序。


地址：`https://ros-qtc-plugin.readthedocs.io/en/latest/_source/How-to-Install-Users.html`


安装步骤如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e2c1509a750b4be88e40d4b4807f94d0.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/7de1c7525a824864944a63682069f456.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/fc18a347b2d344ffb9b36fe7649ac239.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/170599683fff4b2d9d05f9d538298256.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/4355db5031044e46a5e067f25dd3b70c.png)


### 3. 创建ros工程


![在这里插入图片描述](https://img-blog.csdnimg.cn/9bde8f7dcabe433990144fd5993256f6.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/2eb0dddbab3b4a6f9a59543dfa74b3cd.png)


实际应用中，除了做ros机器人的图形界面外，在其他模块代码中也可以运用qt的特性来做开发。


以上。





