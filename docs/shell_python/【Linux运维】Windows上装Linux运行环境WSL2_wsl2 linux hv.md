







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍WSL2的安装与使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. WSL2介绍](#smirk1_WSL2_7)
	+ [:blush:2. WSL2安装](#blush2_WSL2_25)
	+ [:satisfied:3. WSL2测试](#satisfied3_WSL2_71)
	+ - [WSL常用命令](#WSL_74)
		- [WSL与Windows共享文件夹](#WSLWindows_90)
		- [WSL使用VSCode](#WSLVSCode_103)
		- [WSL安装git](#WSLgit_112)
		- [WSL运行Linux GUI应用](#WSLLinux_GUI_122)
		- [WSL安装图形界面](#WSL_142)
		- [WSL关于ROS的图形界面](#WSLROS_165)
		- [WSL安装Qt](#WSLQt_178)
		- [WSL安装OpenGL](#WSLOpenGL_192)
		- [WSL安装数据库](#WSL_322)




### 😏1. WSL2介绍


WSL2是Windows Subsystem for Linux的第二个版本，它允许在Windows操作系统上运行本地Linux应用程序。相比于WSL1，WSL2采用了全新的虚拟化技术，使得Linux内核可以直接运行在一个轻量级的虚拟机中，从而提供更好的性能和更高的兼容性。


具体来说，WSL2使用了`Hyper-V虚拟机`来托管Linux内核。这样一来，WSL2可以实现真正的本地Linux内核，并支持Docker等应用程序的运行。


与WSL1相比，WSL2还提供了**更好的文件系统性能**，同时可以直接访问Windows文件系统中的文件。这意味着您可以在Windows和Linux之间共享文件，而不需要通过FTP或其他协议进行传输。


总的来说，WSL2为开发人员、运维人员以及需要在Windows环境下使用Linux工具的用户带来了很大的便利。


这里再说一下它和虚拟机/双系统的区别：



> 
> 1.它只是个终端，能让我们体验ubuntu下的一些指令操作，但却无法显示GUI程序、图像信息等，主打的点应该是可以和windows同时使用吧  
>  2.可以使用vim和nano，不能使用gedit；windows主系统和linux文件互通  
>  3.它最大的好处可能是更方便了服务器管理者的，因为它集成了如ssh这些命令（方便管理服务器和设备），还有就是可以bash脚本（执行自动化命令）
> 
> 
> 


最精简的安装命令：`wsl --install`


默认安装Ubuntu版本，其他版本需手动设置安装。


### 😊2. WSL2安装


请确认电脑Windows版本在2004以上。


在 Windows 10/11 上安装 WSL 2 的过程如下（下面操作请以管理员身份打开Powershell运行）：


1.启用/安装WSL



```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

```

2.启用虚拟机平台



```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

```

开启虚拟化设置：


控制面板->程序->启用或关闭 windows 功能，开启 `Windows 虚拟化`和 `Linux 子系统（WSL2)`以及`Hyper-V`，然后在powershell输入`bcdedit /set hypervisorlaunchtype auto`


3.设置WSL 2为默认值



```
wsl --set-default-version 2

```

4.安装 Linux 发行版


有了 WSL 和必要的虚拟化技术，接下来要做的就是从 `Microsoft Store` 中选择并安装 Linux 发行版。



```
# 最好选择LTS版本
Ubuntu20/18/16
wsl.exe --install -d Ubuntu-18.04

```

5.卸载旧版WSL  
 若要从计算机中删除旧WSL，请通过命令行或 PowerShell 实例运行以下命令：`wsl --unregister Legacy`。 卸载旧发行版可以运行：`wsl --unregister <DistributionName>`，如`wsl --unregister Ubuntu`，删除发行版后，运行 `wsl --list` 将会显示它不再列出。


还可以选择手动删除旧发行版，方法是使用 Windows 文件资源管理器或 PowerShell 删除 `%localappdata%\lxss\` 文件夹（及其所有子内容）：`rm -Recurse $env:localappdata/lxss/`。


6.其他问题  
 错误：WslRegisterDistribution failed with error: 0x800701bc…  
 解决：参考`http://t.csdn.cn/urRBb`进行内核升级


错误：WslRegisterDistribution failed with error: 0x80370114  
 解决：`https://zhuanlan.zhihu.com/p/361310073`


### 😆3. WSL2测试


安装完成后，可以在开始菜单打开，或通过`Windows Terminal`终端打开，然后设置用户名和密码。


#### WSL常用命令



```
wsl --version	# 版本
wsl --status	# 状态，看是1还是2
wsl --set-default <Distribution Name>	# 设置默认Linux发行版
wsl --list --online		# 可用的发行版
wsl --list --verbose	# 已安装的发行版
wsl.exe --install -d <Distribution Name>	# 终端安装指定发行版（也可在应用商店安装）
wsl --unregister Ubuntu	# 删除发行版（然后可以重新安装，相当于还原出厂设置了）
wsl -l -shutdown	# 重启内核
wsl --update	# 内核更新
wsl -l -v	# 查看wsl情况
sudo apt update && sudo apt upgrade	# 更新与升级

```

#### WSL与Windows共享文件夹


1.windows访问ubuntu wsl的文件夹：



```
\\wsl$\Ubuntu-18.04\home\dev

```

2.ubuntu wsl访问windows的文件夹：



```
cd /mnt/c	# 只需在硬盘符前加上/mnt就行
# 也可直接在windows目录下启动终端，然后执行wsl即可

```

#### WSL使用VSCode


WSL里可以直接使用`code .`打开VSCode，第一次打开会自动安装vscode，很方便（毕竟是微软自己的）。输出如下：



```
Installing VS Code Server for x64 (b3e4e68a0bc097f0ae7907b217c1119af9e03435)
Downloading: 100%
Unpacking: 100%
Unpacked 1759 files and folders to /home/dev/.vscode-server/bin/b3e4e68a0bc097f0ae7907b217c1119af9e03435.

```

#### WSL安装git


与ubuntu一样：`sudo apt-get install git`


然后配置git：



```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"

```

#### WSL运行Linux GUI应用


WSL 2 使 Linux GUI 应用程序在 Windows 上使用起来原生且自然。



```
# 安装gedit编辑器
sudo apt install gedit -y
gedit
# 安装gimp图形编辑器
sudo apt install gimp -y
gimp
# 安装nautilus文件管理器
sudo apt install nautilus -y
nautilus
# 安装vlc播放器
sudo apt install vlc -y
vlc
# 安装X11应用
sudo apt install x11-apps -y
xcalc, xclock, xeyes

```

#### WSL安装图形界面


可参考：`http://t.csdn.cn/MLGcG`


Windows端安装VcXsrv软件用于显示图形界面：`https://sourceforge.net/projects/vcxsrv/`（或者mobaxterm）


Ubuntu端安装xfce4图形界面工具：`sudo apt install -y xfce4`


配置DISPLAY：


首先查看nameserver：`cat /etc/resolv.conf`，如172.23.192.1；


然后添加到.bashrc：



```
vim ~/.bashrc
# 在文件最后追加下面内容,地址使用上面查看到的
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
source ~/.bashrc

```

最后启动：`startxfce4`


然后就可以查看到图形界面了。不过WSL也在慢慢支持原生的Linux GUI程序，我们真正需要的不是一个图形桌面，而是能在WSL端也能看到如相机图像等GUI程序，方便进行计算机视觉开发。


#### WSL关于ROS的图形界面



```
rqt	# 安装完xfce4后，是可以正常打开rqt的
# rviz和gazebo是有关opengl的，默认会报错（核心转储），需要配置一下
vim  ~/.bashrc
export LIBGL\_ALWAYS\_INDIRECT=0
source ~.bashrc
# 然后就可以正常打开了
rviz
gazebo

```

#### WSL安装Qt


安装Qt 5开发包和Qt Creator集成开发环境（IDE）：



```
sudo apt-get install build-essential qt5-default qtcreator
qmake --version		# 查看版本

```

启动qt开发环境：



```
qtcreator
# 然后可以做一个初始的界面并用xfce4显示

```

#### WSL安装OpenGL


安装OpenGL环境：



```
sudo apt-get install build-essential libgl1-mesa-dev freeglut3-dev

```

创建示例程序`test.cpp`：



```
#include <GL/glut.h>
 
#define ColoredVertex(c, v) do{ glColor3fv(c); glVertex3fv(v); }while(0)
static int angle = 0;
static int rotateMode = 0;
 
void myDisplay(void)
{
	static int list = 0;
	if (list == 0)
	{
		GLfloat
			PointA[] = { 0.5f, 0.5f, -0.5f },
			PointB[] = { 0.5f, -0.5f, -0.5f },
			PointC[] = { -0.5f, -0.5f, -0.5f },
			PointD[] = { -0.5f, 0.5f, -0.5f },
			PointE[] = { 0.5f, 0.5f, 0.5f },
			PointF[] = { 0.5f, -0.5f, 0.5f },
			PointG[] = { -0.5f, -0.5f, 0.5f },
			PointH[] = { -0.5f, 0.5f, 0.5f };
		GLfloat
			ColorA[] = { 1, 0, 0 },
			ColorB[] = { 0, 1, 0 },
			ColorC[] = { 0, 0, 1 },
			ColorD[] = { 1, 1, 0 },
			ColorE[] = { 1, 0, 1 },
			ColorF[] = { 0, 1, 1 },
			ColorG[] = { 1, 1, 1 },
			ColorH[] = { 0, 0, 0 };
 
		list = glGenLists(1);
		glNewList(list, GL_COMPILE);
		
		// 面1
		glBegin(GL_POLYGON);
		ColoredVertex(ColorA, PointA);
		ColoredVertex(ColorE, PointE);
		ColoredVertex(ColorH, PointH);
		ColoredVertex(ColorD, PointD);
		glEnd();
		
		// 面2
		glBegin(GL_POLYGON);
		ColoredVertex(ColorD, PointD);
		ColoredVertex(ColorC, PointC);
		ColoredVertex(ColorB, PointB);
		ColoredVertex(ColorA, PointA);
		glEnd();
		
		// 面3
		glBegin(GL_POLYGON);
		ColoredVertex(ColorA, PointA);
		ColoredVertex(ColorB, PointB);
		ColoredVertex(ColorF, PointF);
		ColoredVertex(ColorE, PointE);
		glEnd();
		
		// 面4
		glBegin(GL_POLYGON);
		ColoredVertex(ColorE, PointE);
		ColoredVertex(ColorH, PointH);
		ColoredVertex(ColorG, PointG);
		ColoredVertex(ColorF, PointF);
		glEnd();
		
		// 面5
		glBegin(GL_POLYGON);
		ColoredVertex(ColorF, PointF);
		ColoredVertex(ColorB, PointB);
		ColoredVertex(ColorC, PointC);
		ColoredVertex(ColorG, PointG);
		glEnd();
		
		// 面6
		glBegin(GL_POLYGON);
		ColoredVertex(ColorG, PointG);
		ColoredVertex(ColorH, PointH);
		ColoredVertex(ColorD, PointD);
		ColoredVertex(ColorC, PointC);
		glEnd();
		glEndList();
 
		glEnable(GL_DEPTH_TEST);
	}
	
	// 已经创建了显示列表，在每次绘制正四面体时将调用它
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	glPushMatrix();
	glRotatef(angle / 10, 1, 0.5, 0.0);
	glCallList(list);
	glPopMatrix();
	glutSwapBuffers();
}
 
void myIdle(void)
{
	++angle;
	if (angle >= 3600.0f)
	{
		angle = 0.0f;
	}
	myDisplay();
}
 
int main(int argc, char \*argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGBA | GLUT_DOUBLE);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(700, 700);
	glutCreateWindow("First OpenGL Program");
	
	glutDisplayFunc(&myDisplay);
	glutIdleFunc(&myIdle);     //空闲调用
 
	glutMainLoop();
 
	return 0;
}

```

编译程序：`g++ test.cpp -o test -l GL -l GLU -l glut`


#### WSL安装数据库


参考：`https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-database`


Linux和Bash入门：`https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/linux`


参考学习资料：`https://learn.microsoft.com/zh-cn/windows/wsl/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





