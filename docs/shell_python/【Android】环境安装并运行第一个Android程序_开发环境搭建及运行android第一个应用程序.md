







> 
> 安卓（Android）是一种基于Linux内核的开源操作系统，只要应用于移动设备，如智能手机和平板电脑。近年来也常用于智能移动机器人，如在手机端控制ROS机器人等（基于此原因，学习一下）。
> 
> 
> 




#### 文章目录


* + [1. 环境安装](#1__3)
	+ [2. 运行第一个Android程序](#2_Android_25)




### 1. 环境安装


Android最近的版本号：




| 版本号 | 对应API | 发布时间 |
| --- | --- | --- |
| Android 13 | 33 | 2022年2月 |


Android Studio是官方推荐的Android应用的开发工具。


下载地址：`https://developer.android.google.cn/studio/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/f058679116934a0c899de3e0199fe1fe.png)


安装完Studio后还需要安装SDK（类似Java的JDK）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7cd8fee90c92461d9997f9ed135a9d5a.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/cd11c6447283420f912a37346bd2565b.png)


安装完成后如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e19f4c16fe9e41079ccde3d79c77bfe2.png)


### 2. 运行第一个Android程序


新建空白工程：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d6b5326146a44dd5be6a3c8b8b2d146f.png)


配置工程参数：


![在这里插入图片描述](https://img-blog.csdnimg.cn/60291220cbfe45a59b04ddcfb9cd8636.png)


工程会默认创建`MainActivity.java`和`activity_main.xml`两个文件，第一次创建工程`Gradle`下载的时间会比较长。


配置gradle国内源可以参考：`http://t.csdn.cn/tLCN8`


![在这里插入图片描述](https://img-blog.csdnimg.cn/a2c9e1aeb4884732890b2c4febb37245.png)


接下来，我们创建一个`内置模拟器`，用于显示我们工程APP的效果。


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cd6ca27b73545f391f37abd84d30c44.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a620babc501740ae96f620c0fa95f31f.png)


运行效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0a0dd6a0d814439d8ce442346f9aeea8.png)  
 如果内置模拟器出现闪烁，参考如下地址：`http://t.csdn.cn/CdNxR`


执行示例程序`HelloWorld`，运行效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/982cee3617394ea5a62bd791b3b16a96.png)


以上。





