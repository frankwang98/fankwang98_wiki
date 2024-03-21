







> 
> Joystick Input是Simulink软件中`3D Animation`库中的一个模块，Joystick是操纵杆的意思，通过模型的搭建，它可以将现实中赛车模拟器或者游戏手柄等的输入传递到simulink中，实现信号输入的功能，常用于硬件在环仿真与测试。
> 
> 
> 




#### 文章目录


* + - [Joystick Input模块介绍](#Joystick_Input_4)
		- [测试（以游戏手柄为例）](#_18)
		- [简单应用（HIL）](#HIL_22)




#### Joystick Input模块介绍


在matlab软件的help文档中，我们可以对他进行初步了解。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210621093712145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)  
 通过这一模块的加入，使得simulink模型与3D虚拟场景之间可以进行交互。


这一模块包含了`Axes（轴）`、`Button（按键）`和`Point of view（角度）`三个输出，部分情况下只有两路但也够用；对于具有力反馈的设备，这一模块还可以设置Force（力反馈输入）。


**包含参数如下：**


* ID：分配给给定操纵杆设备的系统 ID。 您可以在系统控制面板的游戏控制器部分找到连接到系统的操纵杆的属性。
* 根据操纵杆配置调整I/O口：如果选中此复选框，则每次打开模型时，Simulink 3D Animation 软件都会根据所连接操纵杆的功能动态调整端口。
* 启用力反馈输入：如果选中此复选框，则 Simulink 3D Animation 软件可以支持力反馈操纵杆、方向盘和触觉（一种启用触觉反馈的）设备。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210621094709904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)  
 **具体的数据类型和取值范围如下：**  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210621094832441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


#### 测试（以游戏手柄为例）


通过在simulink中搭建如下模型（包含Joystick input、demux、display），运行并测试信号变化。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210621095056803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


#### 简单应用（HIL）


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210621100428794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


以上。





