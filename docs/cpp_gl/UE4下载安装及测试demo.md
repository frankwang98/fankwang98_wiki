






UE4 的全名是 Unreal Engine 4，中文译为“虚幻引擎4”。UE4 是一款由 Epic Games 公司开发的开源、商业收费、学习免费的游戏引擎。（已经更新到UE5了，但网上4的教程较多）


虚幻引擎主要用来制作主机游戏，风靡全球的吃鸡游戏 “绝地求生” 也是由UE4 引擎开发。


UE4 有蓝图和C++两种编辑方式，但底层是由 C++ 实现，我们编写的代码也是 C++，但是 UE C++ 经过 Epic 的封装，难度没有那么大，甚至会变得有趣，不过也需要你有 C++ 的基础知识，一般项目是用C++配合蓝图实现。


这里我用的UE4 版本是 4.26.2 Visual Studio 的版本是 2017。




#### 文章目录


* + [UE下载安装](#UE_9)
	+ [VS安装](#VS_22)
	+ [电脑配置](#_31)
	+ [测试UE4游戏demo](#UE4demo_38)




### UE下载安装


进入UE的官网：`https://www.unrealengine.com/zh-CN/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/31bffd1c224f4da58b7d5f78f166b2e9.png)


首先注册一个账号，然后下载Epic启动程序：


![在这里插入图片描述](https://img-blog.csdnimg.cn/173dd09c1f81439a93a860417112e30a.png)


然后打开启动程序，找到虚幻引擎的库，下载你需要的UE版本：


![在这里插入图片描述](https://img-blog.csdnimg.cn/4cd885389f1345f2a329a17c7493bdf4.png)


### VS安装


安装VS2017社区版即可。


可参考这个链接：`http://c.biancheng.net/view/456.html`


UE4 和 VS2017 会自动配置，不需要我们手动去设置。


UE4虽然底层是C++，但在此基础上还扩展了其他特性，类似Qt对C++的扩展（信号和槽机制），因此在Windows平台需要用VS编译器。


### 电脑配置


i7 16G以上


显卡最好20系以上


不仅玩游戏贵，开发游戏也贵，emmm


### 测试UE4游戏demo


demo工程中有个第一人称射击游戏，我们用它来测试一下打开工程到打包发布的流程：


新建游戏项目：


![在这里插入图片描述](https://img-blog.csdnimg.cn/72a3db2aeb5045c2b08ca1641dfa3b70.png)


选择第一人称游戏：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7fb3aa6487c34f56a0940e1a771f13a3.png)


创建项目：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2b5e77dc1d394980a2e9c082c7e793f5.png)  
 项目视图如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/91a009741bca477c9aa213c4daca3dfe.png)


视图介绍如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2e5c6859bfbd4733a6921bf96a8f496c.png)


另外，在4.24以上版本，地形、植被等模式都移动到工具栏的模式中了，使用更方便快捷。


选择打包项目：


![在这里插入图片描述](https://img-blog.csdnimg.cn/55a6e22aa30b4dbcb4457bd23d05077a.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/353293436ddd4f13a5d1693eec3a1f65.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/abb353ea2692433895caf410543192f6.png)


打包好的程序会放在这里：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c02dac2b39404f4f8a35a93299967870.png)


运行如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/99767f40ad1447f78b8a60e2e255b4c5.png)


以上。





