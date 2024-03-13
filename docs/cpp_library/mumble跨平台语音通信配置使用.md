







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍mumble跨平台语音通信。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__52)
	+ [:blush:4. 命令行客户端talkkonnect](#blush4_talkkonnect_62)




### 😏1. 项目介绍


项目Github地址：`https://github.com/mumble-voip/mumble`


官网：`https://www.mumble.info/`


Mumble是一个基于Qt和Opus的开源语音通信软件，旨在提供高质量的实时语音通信功能。它是一种专为游戏玩家和在线社交群体设计的软件，可用于团队协作、多人游戏、在线会议等场景。


以下是Mumble的一些主要特点和功能：



> 
> 1.低延迟实时通信：Mumble通过使用Opus音频编解码器和自定义的网络协议，提供了非常低的语音传输延迟，使得用户在语音聊天中几乎感觉不到任何延迟。
> 
> 
> 



> 
> 2.高音质：Mumble支持高质量的音频编码，可提供清晰、逼真的声音。Opus编解码器具有广泛的音频带宽和动态比特率调整能力，使得Mumble的语音质量非常出色。
> 
> 
> 



> 
> 3.位置音效：Mumble允许用户通过立体声定位音效来模拟在虚拟环境中的空间位置。这对于游戏玩家来说非常有用，可以通过声音来感知其他玩家的位置和方向。（设置-音频输出；类似的还有OpenAL、Wwise等）
> 
> 
> 



> 
> 4.权限和身份管理：Mumble提供了强大的权限系统，允许管理员对用户进行细粒度的控制和配置。用户可以被指派不同角色和权限，从而实现更好的团队协作和管理。
> 
> 
> 



> 
> 5.多服务器支持：Mumble可以同时连接多个服务器，用户可以轻松切换并与不同群体或团队进行语音交流。
> 
> 
> 



> 
> 6.开放源代码：Mumble是一个开源项目，遵循自由软件许可证。这意味着任何人都可以查看、修改和分发Mumble的源代码，使得它成为一个透明和可定制的解决方案。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置，可将服务器安装在ubuntu，然后ubuntu和windows都可以安装客户端，进行语音通信。


Ubuntu上：



```
# 服务端
sudo apt-get install mumble-server
# 配置（自启动yes、网络优先级yes、管理员密码设置）
sudo dpkg-reconfigure mumble-server
# 高级配置
sudo gedit /etc/mumble-server.ini
# 重启
sudo service mumble-server restart

# 客户端
sudo apt-get install mumble
mumble

```

Windows上：



```
# 官网下载client即可
https://www.mumble.info/

```

### 😆3. 使用说明


下面进行使用分析：


ubuntu：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/edfbd0dd5f994db39b7ed043fb68f7ee.png)


windows：


![在这里插入图片描述](https://img-blog.csdnimg.cn/153d6b2fd54949f4b79131ca965ee6b0.png)


### 😊4. 命令行客户端talkkonnect


另外，如果想要让ubuntu端是命令行启动（mumble只有GUI界面），可以尝试用`talkkonnect`，适合在树莓派等场景下。


官网：`https://talkkonnect.com/`


Github：`https://github.com/talkkonnect/talkkonnect`


具体配置可参考：`http://t.csdn.cn/rB39M`


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





