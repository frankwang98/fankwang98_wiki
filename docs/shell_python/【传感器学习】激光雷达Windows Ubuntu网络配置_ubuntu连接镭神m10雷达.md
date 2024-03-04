






处理激光雷达数据一般有三种场景，windows、虚拟机和ubuntu双系统，下面介绍如何让电脑连接像激光雷达这样的网络设备。


首先，给Lidar供电，并将其网线插入个人电脑的网口；




#### 激光雷达网络配置


* + [1.windows](#1windows_6)
	+ [2.虚拟机](#2_27)
	+ [3.Ubuntu](#3Ubuntu_44)




### 1.windows


打开网络和Internet设置；


![在这里插入图片描述](https://img-blog.csdnimg.cn/6163370cb1ce478d95f3e4a5f57284c2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_10,color_FFFFFF,t_70,g_se,x_16)  
 更改适配器选项；


![在这里插入图片描述](https://img-blog.csdnimg.cn/59e3a851e857470b92c58dd87ee8014e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_12,color_FFFFFF,t_70,g_se,x_16)  
 进入网络连接页面（找到激光雷达的网络，这里是以太网）；


![在这里插入图片描述](https://img-blog.csdnimg.cn/774e7e5f6c9d4bf7945b69ffe112cc2b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)  
 右击打开属性后，选择TCP/IPv4，并打开其属性；


![在这里插入图片描述](https://img-blog.csdnimg.cn/4ca93765ec8e45fd9309f621cc77a025.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_10,color_FFFFFF,t_70,g_se,x_16)  
 输入IP和子网掩码即可（激光雷达一般是102）；


![在这里插入图片描述](https://img-blog.csdnimg.cn/4cbfc8c266e54e8291a0a1817cb3fb33.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_11,color_FFFFFF,t_70,g_se,x_16)  
 配置完成，可以在终端（cmd）下ping测试一下；  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a4ddbd5e081d47379f95cb3ed87cc5b9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)


### 2.虚拟机


虚拟机是寄托在windows系统上的，因此虚拟机要连接网络设备，必须让windows先连接上，所以，先把windows的网络配置走一遍，然后再配置linux中的网络：


点击添加网络：


![在这里插入图片描述](https://img-blog.csdnimg.cn/141555c2e9234eed884335096a2d11ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)  
 添加名称；


![在这里插入图片描述](https://img-blog.csdnimg.cn/3f9a10194abe4830974f5f735f193626.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)  
 设置IPv4为手动方式，并输入设备的IP、子网掩码，保存即可；


![在这里插入图片描述](https://img-blog.csdnimg.cn/292d17ee91434330bf12a092197f8762.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/ca848d1270a942b78532f4cf30c86bf5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)  
 最后一样，在终端里ping测试一下；  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/29bac31f24b04609b91c721b48d7face.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_11,color_FFFFFF,t_70,g_se,x_16)


### 3.Ubuntu


原生/双系统的Ubuntu直接配置这里的网络即可，同虚拟机。


![在这里插入图片描述](https://img-blog.csdnimg.cn/7f3eaf5935fb4545809eca1ea41183ea.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)


对于网络摄像头、激光雷达这样的设备，配置好网络是进一步开发的前提。


以上。





