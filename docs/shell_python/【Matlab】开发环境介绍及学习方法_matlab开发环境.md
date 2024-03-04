








#### 文章目录


* + [1.简介](#1_1)
	+ [2.开发环境](#2_7)
	+ [3.MATLAB编程入门](#3MATLAB_31)
	+ [4.Simulink动态仿真环境介绍](#4Simulink_77)
	+ [5.学习方法](#5_90)




### 1.简介


理工科的学生相信大家对MATLAB都不陌生了。


MATLAB是是矩阵实验室（Matrix Laboratory）的意思，在数学和工程分析中经常要用到，实用性很强。MATLAB具有数值分析、[数值和符号计算](https://blog.csdn.net/Dust_Evc/article/details/109005459)、工程与科学绘图、控制系统的设计与仿真、数字图像处理、数字信号处理、财务与金融工程等功能。尤其是在控制系统的设计和仿真方面，甚至催生出一个单独的Simulink设计模块。它将数值分析、矩阵计算、科学数据可视化以及非线性动态系统的建模和仿真等诸多强大功能集成在一个易于使用的视窗环境中，为科学研究、工程设计以及必须进行有效数值计算的众多科学领域提供了一种全面的解决方案（主要是它的指令表达式与数学、工程中常用的形式十分相似），并在很大程度上摆脱了传统非交互式程序设计语言（如C、Fortran）的编辑模式（但有少量学校好像还在学Fortran，可能是更需要效率还是什么），代表了当今国际科学计算软件的先进水平（当前数学类软件主要分为数值计算型和符号计算型/数学分析型，前者MATLAB是绝对主力，后者还有Mathematica,Maple等）。在高校，MATLAB已经成为线性代数，自动控制理论，数理统计，数字信号处理，时间序列分析，动态系统仿真等高级课程的基本教学工具。


MATLAB的发展历史这里不再赘述，有兴趣的自己去了解。总的来说，MATLAB是一种交互式程序编写和可视化编程相结合的开发软件。


### 2.开发环境


这里我以MATLAB 2018b为例介绍：


![在这里插入图片描述](https://img-blog.csdnimg.cn/66527106328344efba4e74e0d631f5b1.png)  
 MATLAB每年更新两个版本，上半年3月份更新的是a版，下半年9月份更新的是b版：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ad59a00f84ed406abf529acfd5ac8c53.png)


感兴趣的可以先看一下官方对[MATLAB](https://ww2.mathworks.cn/products/matlab.html)和[Simulink](https://ww2.mathworks.cn/products/simulink.html)的定义和介绍，不过我们最常用到的还是它的[官方文档和示例](https://ww2.mathworks.cn/help/?s_tid=hp_ff_l_doc)。


安装好MATLAB后，打开工作界面如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2dfa61bedec744c2afeeea796be56988.png)  
 然后再看一下Simulink的工作环境，可以通过工具栏打开或直接在命令行窗口输入Simulink打开，Simulink起始页如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b21d1e8f40e844fb8401d255b1b750dc.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/e9fd96f1db22403ba56a8ad74f950c8a.png)


Simulink的工作区如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d01e74e18503448c824be6fa90620cb4.png)


熟悉了MATLAB & Simulink的开发环境，下面就开始做几个简单的案例来入门。


### 3.MATLAB编程入门


除了在命令行窗口直接输入命令外，MATLAB更常用的编程方式是创建m文件脚本（后缀是.m），类似于Linux中的shell：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f3341ce3a39a415388384e24006ab82d.png)  
 m文件有脚本和函数两种，也就是说它既可以创建一个脚本集合，也可以创建一个函数，两者的区别如下：


* 脚本：脚本文件是以.m扩展名的程序文件，在这些文件中，可以编写一系列要一起执行的命令。脚本不接  
 受输入，不返回任何输出。它们对工作空间中的数据进行操作。
* 函数：函数文件也是扩展名为.m的程序文件。函数可以接受输入和返回输出。内部变量是函数的局部变量。


m文件可以通过MATLAB编辑器或其他任意编辑器创建，文件包含多个连续的MATLAB命令行或函数调用。可以通过在命令行中键入其名称来运行脚本。


除了在IDE创建m文件外，还可以在命令行窗口通过命令来创建，键入：



```
edit
%或者%
edit newfile.m

```

edit命令是创建一个未命名的m文件，后面加上文件名称，即创建一个指定名称的文件。


![在这里插入图片描述](https://img-blog.csdnimg.cn/07cd208402024fa085cc9011e396f8c9.png)


命令行中也可以创建文件夹，进入指定目录创建m文件，然后运行；下面演示一下：



```
mkdir demo
cd demo
edit demo1.m

```

输入以下代码：



```
a = 10;
b = 5;
c = a + b;
disp(c);

```

输入完成后，点击工具栏的运行或者在命令行窗口键入文件名（demo1）运行脚本。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ccb69a7348274a24a532071f5f7420a0.png)  
 此外，MATLAB编辑器也支持断点调试功能，如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a6276ba694354482a12f041a8cfb4d14.png)


关于MTALAB的其他变量、语句、基本语法等信息，网上有许多资料，这里不再赘述。


### 4.Simulink动态仿真环境介绍


Simulink 是一种可视化模块图编辑环境，用于多域仿真以及基于模型的设计（这在汽车行业中已经成为共识）。它支持系统级设计、仿真、自动代码生成以及嵌入式系统的连续测试和验证（从MIL到HIL）。Simulink 提供图形编辑器、可自定义的模块库以及求解器，能够进行动态系统建模和仿真。Simulink 与 MATLAB 集成，不仅能够在 Simulink 中将 MATLAB 算法融入模型，还能将仿真结果导出至 MATLAB 做进一步分析（两种编程环境数据共享）。


下面基于Simulink做一个简单的正弦信号的仿真：


在Simulink模块库中找到Sine Wave正弦信号模块和Scope示波器模块，并用连接线连接，然后点击运行：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/ed54d6ac9c77423480807afa8af5bd26.png)


修改仿真时长为20，然后双击Scope模块，效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e0b0ba9ec9b64aba951352632fe7bf41.png)


关于Simulink更多模块和操作说明，将在后面介绍。


### 5.学习方法


MATLAB的学习方法通过总结主要有以下3种：


1. 通过MATLAB官方文档学习（付费软件的help服务就是好）
2. 通过MATLAB自带的函数学习（自带的代码永远很规范）
3. 通过MATLAB第三方代码学习（download别人的代码）


MATLAB官方文档详细介绍了各种指令操作、函数说明和源码示例等，遇到问题直接找文档比百度都管用。


![在这里插入图片描述](https://img-blog.csdnimg.cn/50a7f320857c4d85b7ec04b97b075007.png)


除内部函数以外，所有MATLAB的核心文件和工具箱文件都是可读可改的源文件，用户可通过对源文件的修改以及加入自己的文件构成新的工具箱。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ad3fcfd8b812459092e1ce34b7c9dedf.png)


MATLAB还支持用户自己上传代码，在获取附加功能中可以下载别人上传的代码学习。


![在这里插入图片描述](https://img-blog.csdnimg.cn/da9fa1765bbe46f5a0384b474f315c26.png)


以上。





