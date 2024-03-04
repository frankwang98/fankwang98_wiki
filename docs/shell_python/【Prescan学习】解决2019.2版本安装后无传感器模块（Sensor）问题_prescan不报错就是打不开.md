








#### 目录


* + [2022.1.4-软件更新](#202214_1)
	+ [2021第一版-无sensor问题](#2021sensor_11)




### 2022.1.4-软件更新


**解决2022.1.1后prescan打不开的问题，感谢评论区大佬。**  
 问题如下：  
 ![请添加图片描述](https://img-blog.csdnimg.cn/a3a616020f78469e9e4ddfb2f40890be.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_7,color_FFFFFF,t_70,g_se,x_16)


解决方法：将lic文件里的2021全部替换为2022，然后重启软件即可。


![请添加图片描述](https://img-blog.csdnimg.cn/20e93f244acd4080a5a98b404772a714.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)  
 以上。


### 2021第一版-无sensor问题


今天需要用到Prescan传感器的时候，突然发现装的软件里没有`Sensor`模块，网上找了好久，把解决方案放在下面：


1.当时我们第一步都运行了这个install的bat程序，现在把它卸载掉，也就是运行`remove.bat`。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623144409735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


2.简单来说就是，当时那个服务程序里没有解封Sensor模块，这里我们需要下载最新版的`license`。如下：↓↓↓ 下载后将其放在安装目录的Prescan\_2019.2文件夹中（后面用）



> 
> 链接：https://pan.baidu.com/s/1NLy5Wnrd\_agoWStlOBkSOg  
>  提取码：3ayk  
>  （2022.7.28更新链接）
> 
> 
> 


3.设置环境变量（这个大家都不陌生了），`我的电脑-系统属性-高级系统设置--环境变量-新建系统变量`



> 
> 变量名：MADLIC\_LICENSE\_FILE  
>  变量值：上面下载的license所在地址
> 
> 
> 


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623145101110.png)


4.完成后，打开Prescan，Sensor模块已出现


![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062314515611.png)


以上。





