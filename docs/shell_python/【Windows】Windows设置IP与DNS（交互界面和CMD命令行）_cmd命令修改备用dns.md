






这部分讲一下windows系统中如何设置IP和DNS，有界面设置和命令行设置两种。  
 



#### 文章目录


* + [一、网络和Internet设置（静态/动态IP）](#InternetIP_2)
	+ [二、CMD命令行网络设置（静态/动态IP）](#CMDIP_15)




### 一、网络和Internet设置（静态/动态IP）


依次打开：


![在这里插入图片描述](https://img-blog.csdnimg.cn/854f8106ad294da5bf86cd0263ebae48.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0254ee2e45e34ba6959b28538cb31e14.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/8a9e01fd5196498cb134e7c893bfbae4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0d69ebeebbbf4e32b8814fd3c08083f6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_10,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/ccb9b56709414f89a207c635278afe9e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_11,color_FFFFFF,t_70,g_se,x_16)


### 二、CMD命令行网络设置（静态/动态IP）


但有时，当你设置静态IP时会蹦出这样一个错误，目前我的解决方法是可以在CMD命令行进行设置（*有其他更好的方法可以私信我*）。


究其原因可能是前面安装过虚拟机自带的虚拟网卡惹的祸，但我删干净后依然会有这个问题。


![在这里插入图片描述](https://img-blog.csdnimg.cn/451429ea1b1749879fca4fbb20587959.png)


Windows+R，输入cmd进入命令行，常用的设置网络命令如下：


**1.设置IP**  
 设置自动获取IP地址（DHCP）——`netsh interface ip set address name="本地连接" source=dhcp`  
 设置固定IP——`netsh interface ip set address name="本地连接" source=static addr=192.168.0.3 mask=255.255.255.0 gateway=192.168.0.1 gwmetric=auto`（gateway和gwmetric默认不设置也可以）


**2.设置DNS**  
 自动获取DNS——`netsh interface ip set dns name="本地连接" source=dhcp`  
 手动设置单个DNS 例218.85.157.99——`netsh interface ip set dns name="本地连接" source=static addr=218.85.157.99 register=primary`  
 需要多加个备用DNS 例202.101.98.55——`netsh interface ip add dns name="本地连接" source=static addr=202.101.98.55 index=2`


以上。





