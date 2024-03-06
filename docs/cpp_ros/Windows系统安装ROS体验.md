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
