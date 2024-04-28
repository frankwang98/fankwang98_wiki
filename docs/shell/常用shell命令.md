# Linux系统及Shell脚本精通

## Linux那些事儿

查看大文件：`find . -type f -size +200M -exec du -h {} \;`

Q：为什么要学习Linux？  
A：IT互联网企业无论是开发还是运维都要求精通Linux，因为服务器都是跑在Linux/类Linux系统上的。  
   物联网行业（包括自动驾驶）所用的开发系统也渐渐向Linux转移，因为开源、高效，最重要的是兼容性好。  

### 一、什么是Linux
Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。  
Linux 是一套免费使用和自由传播的类 Unix 操作系统，是一个基于 POSIX 和 UNIX 的多用户、多任务、支持多线程和多 CPU 的操作系统。  
Linux 能运行主要的 UNIX 工具软件、应用程序和网络协议。它支持 32 位和 64 位硬件，也能在 嵌入式设备（如树莓派、Jetson nano） 上安装。Linux 继承了 Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。  

详细介绍参[菜鸟教程-Linux介绍](https://www.runoob.com/linux/linux-intro.html)

### 二、Linux-Ubuntu系统安装
Linux 的发行版说简单点就是将 Linux 内核与应用软件做一个打包。  

声明：后文用的是Ubuntu发行版。   

#### 安装说明
- 关于虚拟机与双系统的选择问题作简要说明：
	1. 需要多系统协同工作环境，请选择虚拟机，如做网络安全与渗透测试需要搭建的eNSP和Kali Linux+靶机
	2. 需要纯净和性能较好的车辆机器人开发环境，请选择双系统，如运行Apollo/Autoware自动驾驶工具或进行GPU神经网络和深度学习等

- Linux（Ubuntu）安装问题
	1. 系统的安装过程可能会有各种bug出现，提供以下推荐安装标准，其他bug请自行/讨论解决
	2. 对于个人/团队开发者而言，一种最简单的安装方法如下：
		- 下载Linux镜像文件（如Ubuntu 18.04.06LTS.iso）,使用软碟通制作镜像盘，然后电脑开机按F8进入U盘启动，进行安装流程
		- 推荐的分区方法：两个分区u即可【1./efi（系统启动分区，1-10G） 2./（其余空间都给它）】
	3. 安装完成后电脑重启会自动进入引导程序，选择ubuntu启动，已经预装了包括浏览器等软件，这时我们先检查一下系统情况
		- 首先更新软件源，在软件和更新中选择清华源/中科大源
		- 安装systemback系统备份恢复工具（/README），将当前系统备份（备份的好处不多说了）
		- 可选择安装图形化软件管理工具Synaptic Package Manager
  
####  国内镜像网站
  - [tsinghua](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/)
  - [ustc](http://mirrors.ustc.edu.cn/ubuntu-releases/)
  - [zju](http://mirrors.zju.edu.cn/ubuntu-releases/)
  - [aliyun](http://mirrors.aliyun.com/ubuntu-releases/)


### 三、Linux系统启动过程
linux启动时我们会看到许多启动信息。

Linux系统的启动过程并不是大家想象中的那么复杂，其过程可以分为**5个阶段**：

1.内核的引导
首先电脑主板中的BIOS开机会自检硬盘中的启动设备。  

操作系统接管硬件后，首先读取`/boot`目录下的内核文件。

2.运行init
init进程时系统所有进程的起点，读取的是配置文件/etc/inittab。

许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

3.系统初始化
在init的配置文件中有这么一行： `si::sysinit:/etc/rc.d/rc.sysinit`　

它调用执行了`/etc/rc.d/rc.sysinit`，而`rc.sysinit`是一个bash shell的脚本，它主要是完成一些系统初始化的工作，rc.sysinit是每一个运行级别都要首先运行的重要脚本。

它主要完成的工作有：激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行的任务。

4.建立终端
rc执行完毕后，返回init。这时基本系统环境已经设置好了，各种守护进程也已经启动了。

init接下来会打开6个终端，以便用户登录系统。在inittab中的以下6行就是定义了6个终端：
```
1:2345:respawn:/sbin/mingetty tty1
2:2345:respawn:/sbin/mingetty tty2
3:2345:respawn:/sbin/mingetty tty3
4:2345:respawn:/sbin/mingetty tty4
5:2345:respawn:/sbin/mingetty tty5
6:2345:respawn:/sbin/mingetty tty6
```
从上面可以看出在2、3、4、5的运行级别中都将以respawn方式运行mingetty程序，mingetty程序能打开终端、设置模式。

同时它会显示一个文本登录界面，这个界面就是我们经常看到的登录界面，在这个登录界面中会提示用户输入用户名，而用户输入的用户将作为参数传给login程序来验证用户的身份。

5.用户登录系统
一般来说，用户登陆方式有 3 种：
* 命令行登录
* ssh登录
* 图形界面登录
一般我们装的Ubuntu系统都是可以图形界面登录，输入安装时设置的账号密码。

最后，个人PC上的Linux每天要关机，除了图形界面的关机按钮外，命令行关机有以下：
正确的关机流程为：`sync > shutdown / reboot / halt`

	sync：将数据由内存同步到硬盘中，防止数据丢失（有教训）。
	
	shutdown 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：
	
	shutdown –h 10 ‘This server will shutdown after 10 mins’ 这个命令告诉大家，计算机将在10分钟后关机，并且会显示在登陆用户的当前屏幕中。
	
	shutdown –h now 立马关机
	
	shutdown –h 20:25 系统会在今天20:25关机
	
	shutdown –h +10 十分钟后关机
	
	shutdown –r now 系统立马重启
	
	shutdown –r +10 系统十分钟后重启
	
	reboot 就是重启，等同于 shutdown –r now
	
	halt 关闭系统，等同于shutdown –h now 和 poweroff

### 四、Linux文件系统结构
```
/bin：
bin 是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。

/boot：
这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。

/dev ：
dev 是 Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。

/etc：
etc 是 Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录。

/home：
用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的，如上图中的 alice、bob 和 eve。

/lib：
lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。

/lost+found：
这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

/media：
linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下。

/mnt：
系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。

/opt：
opt 是 optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

/proc：
proc 是 Processes(进程) 的缩写，/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。

/root：
该目录为系统管理员，也称作超级权限者的用户主目录。

/sbin：
s 就是 Super User 的意思，是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里存放的是系统管理员使用的系统管理程序。

/selinux：
 这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

/srv：
 该目录存放一些服务启动之后需要提取的数据。

/sys：

这是 Linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。

sysfs 文件系统集成了下面3种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。

该文件系统是内核设备树的一个直观反映。

当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

/tmp：
tmp 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。

/usr：
 usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

/usr/bin：
系统用户使用的应用程序。

/usr/sbin：
超级用户使用的比较高级的管理程序和系统守护程序。

/usr/src：
内核源代码默认的放置目录。

/var：
var 是 variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

/run：
是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。
```

### 五、Linux常用命令

```
sudo apt-get update	// 软件更新
sudo apt-get upgrade	// 软件升级
sudo apt-get autoremove	// 自动移除不必要的包
sudo dpkg -i package_file.deb	// 安装deb套件包
sudo dpkg -r package_name	// 反安装deb套件包
cd、pwd、ls、mkdir、sudo、chmod、rm、mv、cp		// 文件目录操作
ifconfig、ping		// 网络配置	
```

对于一些生僻的命令，查看这里：[Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)

### 六、Linux常用软件
#### 编辑器
- nano/vim	// 命令行编辑器，可配置nanorc文件
- gedit		// 图形编辑器
- VSCode(code)	// 轻量化代码编辑器，可安装丰富的插件
- Qt		// 跨平台C++ GUI开发，也可用作C++、ROS编辑器

#### 常用配置文件
- /etc/apt/source.list	// apt安装源设置
- .bashrc		// 用户环境变量
- 启动应用程序 	 // 设置开机自启动

#### 常用软件
- shutter	//截图
- cutecom	//串口调试工具sudo apt-get install cutecom -y
- iptux	//即时通讯工具
- OBS studio	//录屏
- Remmina      //远程连接工具（支持RDP、SSH、VNC）

#### 机器人及自动驾驶开发环境
- C/C++、Python编译环境（ROS安装中会自动安装）
- ROS Melodic（自带Opencv和PCL 1.8）
- Autoware（感规控和标定工具箱）


### Linux常见问题
#### 远程登陆
1. ssh
mobaxterm、xshell、putty
2. vnc
vino、remmina、win10

#### nano编辑器快速配置
nano是ubuntu系统自带的编辑器，也是最容易学习上手的编辑器，但初始配置在使用起来有点不尽如人意，好在官方已经给我们留好了一些配置选项，下面就记录一下我常用的编辑器环境配置：

首先，新建文件nano ~/.nanorc，输入以下配置指令:		
```
set linenumbers
set autoindent
set mouse
set smooth
set tabsize 4
```
然后，进入系统变量配置文件，sudo nano /etc/nanorc，将该文件中以上几条指令前的#去掉即可；

再次进入nano，行号、缩进、鼠标等就生效了。

以上。

#### 无法获得dpkg前端锁的解决方法
问题：E：无法获得锁……是否有其他进程正占用它？

原因：进程占用

解决：在终端依次输入下面命令：	
```
sudo rm /var/lib/dpkg/lock

sudo rm /var/lib/dpkg/lock-frontend

sudo rm /var/cache/apt/archives/lock
```
然后，继续运行apt命令就正常了。

[参考链接](https://blog.csdn.net/qq_40344790/article/details/118143502)

#### 快速关机
##### 通过设置默认停止超时时间

关机的默认等待时间默认为 90 秒。在这个时间之后，你的系统会尝试强制停止服务。但一般情况下，我们会想让ubuntu的关机和开机一样快，这时我们就可以修改这个时间。

在位于 /etc/systemd/system.conf 的配置文件中找到所有的系统设置。该文件中包含很多以 # 开头的行，代表了文件中各条目的默认值。

在开始之前，最好先复制一份原始文件。

`sudo cp /etc/systemd/system.conf /etc/systemd/system.conf.orig`

在这里找到 DefaultTimeoutStopSec，它被设置为 90 秒。

`DefaultTimeoutStopSec=90s`
可以改为5s或10s。

##### top查看和关闭进程

- top kill

#### 一种类似迅雷的下载器安装
[原文链接](https://blog.csdn.net/qq_40344790/article/details/121786376?spm=1001.2014.3001.5501)

#### 环境及显卡配置
[原文链接](https://blog.csdn.net/qq_40344790/article/details/121765107?spm=1001.2014.3001.5501)

#### 【Linux】Vim的安装与环境配置
[原文链接](https://blog.csdn.net/qq_40344790/article/details/119513112)

#### Linux安装微信
最近找到一种在Ubuntu20.04用snap工具安装微信的方法，当然不是官方的产品。

步骤如下：
```
sudo apt install snapd snapd-xdg-open
sudo snap install electronic-wechat
```
启动：`electronic-chat`		
虽然安装是成功了，但提示“安全原因，不能登陆”，但网上有些帐号是可以的，这就“懂的都懂”了。

如果你的帐号不能使用，卸载命令如下：`sudo snap remove electronic-wechat`

#### Linux安装钉钉
相对于微信，钉钉还算国产化的好一点，支持国产CPU架构的电脑以及debian Linux。

考虑到大家一般用Ubuntu比较多，这里搬运官方基于Ubuntu18.04及以上版本的安装。具体如下：

1. 下载链接：(当前最新版本1.3.0.20214)		
`https://dtapp-pub.dingtalk.com/dingtalk-desktop/xc_dingtalk_update/linux_deb/Release/com.alibabainc.dingtalk_1.3.0.20214_amd64.deb`
2. 下载到本地，使用dpkg命令安装		
`sudo dpkg -i com.alibabainc.dingtalk_1.2.0.132_amd64.deb`
3. 钉钉卸载方法	
`sudo dpkg -r com.alibabainc.dingtalk`	
`sudo dpkg -P com.alibabainc.dingtalk`



## shell脚本编程

学习Linux除了学Linux安装和基础操作外，另一个必不可少的就是shell脚本编程。

大家都知道Linux系统的优点之一就是“**终端命令行操作的高效和快捷！**”，而Linux shell的用途就是编写自动化测试脚本，如一键安装XX软件、一键启动XX环境……

### 一、什么是shell脚本

大家都知道linux有很多内置的终端命令，是存放在/bin目录下的，这些可以说是内置shell命令。
Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计（脚本）语言。
Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

声明：以下都用bash shell，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

### 二、第一个shell脚本

和许多编程语言一样，通过执行 Hello World 程序给大家建立学习的信心，也用来确认编译环境正常！

```
#!/bin/bash
echo "Hello World !"
echo "请输入你的名字："
read name
echo -e "你好，$name！在这一天：\n"`date` "\n你开始学习shell了！\n我与你同在！"
```

(#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。)

echo 命令用于向窗口输出文本。	
运行shell脚本：  
`chmod +x ./test.sh`  	#使脚本具有执行权限  
`./test.sh`  #执行脚本

### 三、shell常用语法实践

#### shell变量

- 变量

```
#!/bin/bash
my_name="gege"
echo $my_name
```

#### shell字符串

- str

```
#!/bin/bash
your_name="awei"
str="Hello, I know you are \"$your_name\"! \n"	# \"是特殊转义，表示双引号
echo -e $str	# -e 开启转义
```

- str拼接

```
#!/bin/bash
your_name="awei阿伟"
my_name="dalao大佬"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${my_name} !"
echo $greeting  $greeting_1
```

- str提取长度

```
#!/bin/bash
string="abcd"
echo ${#string}   # 输出 4
#echo ${#string[0]}   # 输出 4
```

- str提取子字符串

```
#!/bin/bash
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

#### shell数组

bash当前只支持一维数组，不限制数组大小。

```
#!/bin/bash
array_name=(value0 value1 value2 value3)
echo ${array_name[@]}	# 打印所有数组变量
echo ${array_name[1]}	# 打印特定变量
# 取得数组元素的个数
length=${#array_name[@]}
echo $length
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
echo $lengthn
```

#### shell注释

```
#  单行注释
:<<!
多行注释
!
```

#### shell传递参数

```
#!/bin/bash
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
#-----------------------
echo "Shell 传递参数实例！";
echo "第一个参数为：$1";
echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

#### shell基本运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。	
expr 是一款表达式计算工具，使用它能完成表达式的求值操作。 
需要注意：	
表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。	
完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。		

##### shell算术运算

```
#!/bin/bash
# shell算术运算符
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

##### shell关系运算

```
#!/bin/bash
# shell关系运算符
a=10
b=20
# 等于
if [ $a -eq $b ]
then
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
# 不等于
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
# 左大于右
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
# 左小于右
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
# 左大于等于右
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
# 左小于等于右
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
```

##### shell布尔运算

```
#!/bin/bash
# shell布尔运算符
a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a == $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
```

##### shell逻辑运算

```
#!/bin/bash
# shell逻辑运算符
a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
```

##### shell字符串运算

```
#!/bin/bash
# shell字符串运算符
a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
```

##### shell文件测试运算

```
#!/bin/bash
# shell文件测试运算符
file="/home/wangzf/dev/fishros/1"	# 写要测试的文件
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

#### shell printf输出

```
#!/bin/bash
# shell printf输出
# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样
printf '%d %s\n' 1 "abc"

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

#### shell test测试

```
#!/bin/bash
# shell test命令（测试更规范）
#shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。
# 数值
num1=100
num2=100
if test $[num1] -eq $[num2]	# 代码中的 [] 执行基本的算数运算
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
# 字符
str1="ru1noob"
str2="runoob"
if test $str1 = $str2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
# 文件
cd ~
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
# 另外，Shell 还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其优先级为： ! 最高， -a 次之， -o 最低。例如：
cd ~
if test -e ./notFile -o -e ./bash
then
    echo '至少有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```

#### shell流程控制

##### if_else

```
#!/bin/bash
# shell if-else命令
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

##### for

```
#!/bin/bash
# shell for循环
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

##### while

```
#!/bin/bash
# shell while循环
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
# 以上实例使用了 Bash let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量
```

##### until

```
#!/bin/bash
# shell until循环
a=0

until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done
```

##### case-easc

```
#!/bin/bash
# shell case ... esac多选择语句,;;表示break
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

##### break

```
#!/bin/bash
# shell break（外面一个无限循环，里面多选择+break）
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

#### shell函数

- 无返回值

```
#!/bin/bash
# shell 函数
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

- 有返回值

```
#!/bin/bash
# shell 函数
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
# 函数返回值在调用该函数后通过 $? 来获得。 
```

#### shell输入/输出重定向

```
command > /dev/null	# 如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null，/dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。
command > file 	# 将输出重定向到 file
command < file 	#将输入重定向到 file。
command >> file 	#将输出以追加的方式重定向到 file。
n > file 	#将文件描述符为 n 的文件重定向到 file。
n >> file 	#将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m 	#将输出文件 m 和 n 合并。
n <& m 	#将输入文件 m 和 n 合并。
<< tag 	#将开始标记 tag 和结束标记 tag 之间的内容作为输入。
# 文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
```

#### shell文件包含/生效环境变量

```
source filename 或. filename   # 注意点号(.)和文件名中间有一空格
# 生效/包含上一个文件后，第二个文件可以引用第一个文件的内容/变量
```

### shell应用案例

#### 判断是否为root用户

```
#!/bin/bash
if [ $UID -eq 0 ];then
        echo "the user is root"
else
        echo "the user is not root"
fi
```

#### 读取键盘消息

```
#!/bin/bash
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

#### 多窗口启动

```
#!/bin/bash

{
gnome-terminal --title="roscore" -x bash -c "roscore"; 
}

echo 'roscore success'
sleep 1;

{
gnome-terminal --title="startAutoDriver" -x bash -c "bash startAutoDriver.sh"; 
}

echo 'startAutoDriver success'
sleep 1;

{
gnome-terminal --title="startLidarFusion" -x bash -c "bash startLidarFusion.sh"; 
}

echo 'startLidarFusion success'
sleep 1;

{
gnome-terminal --title="startLidars" -x bash -c "bash startLidars.sh"; 
}

echo 'startLidars success'
sleep 1;

{
gnome-terminal --title="startNtrip" -x bash -c "bash startNtrip.sh"; 
}

echo 'startNtrip success'
```

### 查看大文件
```
find . -type f -size +100M -exec du -h {} +
```


#### 文章目录


* + [nano编辑器](#nano_2)
	+ [vim编辑器](#vim_12)
	+ [shell常用命令](#shell_50)



  
 Linux的文本编辑器有nano、gedit、vi、vim等。 

### nano编辑器


nano是ubuntu系统自带的编辑器，也是最容易学习上手的编辑器，但初始配置在使用起来有点不尽如人意，好在官方已经给我们留好了一些配置选项，下面就记录一下我常用的编辑器环境配置：


首先，新建文件`nano ~/.nanorc`，输入以下配置指令；


![在这里插入图片描述](https://img-blog.csdnimg.cn/df943d374f6b41ea9b2bfde0b4cac182.png)  
 然后，进入系统变量配置文件，`sudo nano /etc/nanorc`，将该文件中以上几条指令前的`#`去掉即可；


再次进入nano，行号、缩进、鼠标等就生效了。


### vim编辑器


编辑文件：`sudo vim /etc/vim/vimrc`


在配置文件中可以看到有下面这个if判断,意思是语法高亮,如果是被注释掉状态（#/"）,可以将其放开（删掉#/"号）：



```
  if has("syntax")
    syntax on
  endif


```

然后在配置文件的最后一行，输入以下内容，可以让vim变得更漂亮、舒服，使用也更方便。



```
  set nu   " 设置左侧行号
 set tabstop=4 " 设置tab键长度为4(注意=和数字4之间不要有空格，否则打开vim会报错)
  set cursorline  " 突出显示当前行
 set ruler " 在右下角显示光标位置的状态行
  set autoindent  " 自动缩进
 set nobackup " 覆盖文件时不备份


```

编辑完成后使用命令 :wq 进行保存退出。


vim编辑器快捷指令：



```
a/i 进入编辑模式
ESC 返回命令模式
：wq 保存并退出
dd 删除整行
yy 复制整行
p 粘贴
u 撤销
/字符串 从上到下搜索
？字符串 从下到上搜索

```

### shell常用命令



```
echo $HOSTNAME
echo $HOME
date
ifconfig
uname -a
ubuntu-release
uptime
who
tar -xvzf  tar -cvzf
grep 
| 管道符是命令的进阶
> 输出重定向（>> 追加）
* 命令通配符
systemctl restart network 重启网卡
if、then、fi 、case条件语句
for、while-do 循环语句

```

以上。





