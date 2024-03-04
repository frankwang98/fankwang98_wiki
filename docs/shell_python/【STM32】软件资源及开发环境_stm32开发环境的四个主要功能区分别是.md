








#### 文章目录


* + [MDK5](#MDK5_1)
	+ [固件库](#_17)
	+ [新建工程模板](#_53)
	+ [程序下载](#_116)




### MDK5


MDK5的组成如下（核心包括4个部分：uVision IDE with Editor（编辑器），ARM C/C++ Compiler（编译器），Pack Installer（包安装器），uVision Debugger with Trace（调试跟踪器）。Software Packs（包安装器）又分为：Device（芯片支持），CMSIS（ARM Cortex 微控制器软件接口标准）和 Mdidleware（中间库）三个小部分）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/53632685d11b43beb23dffac7e339bc3.png)


准备好MDK5安装包和F1的芯片支持：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20bafbdb86604c14a6555499a52d4b42.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/1c894a0b1b9446e688fd35cb39c7a7ef.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/d935ecb026df418c87d0602d1a27e066.png)


安装完成后离线导入芯片支持包即可：


![在这里插入图片描述](https://img-blog.csdnimg.cn/3e13b01202c64798b43729548ea29da4.png)


### 固件库


我们下面都用库函数开发，首先介绍一下库函数。固件库就是函数的集合，固件库函数的作用是向下负责与寄存器直接打交道，向上提供用户函数调用的接口（API）。


例如，再51中直接操作寄存器：



```
P0=0x11;

```

32中也可以直接操作寄存器：



```
GPIOx->BRR = 0x0011;

```

但STM32的寄存器太多了，为了方便开发者，官方才推出固件库函数，如下：



```
void GPIO\_ResetBits(GPIO_TypeDef\* GPIOx, uint16\_t GPIO_Pin)
{
 GPIOx->BRR = GPIO_Pin;
}

```

但要精通STM32，还是要了解以下寄存器实现的原理的。


Cortex-M3芯片的结构如下：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/560fa691c2c74c50ba9d78838e9da270.png)


为了让所有使用Cortex-M3芯片的公司软件兼容，ARM和芯片商共同提出了CMSIS标准（Cortex Microcontroller Software Interface Standard），即微控制器软件接口标准。STM32也适用。下面是基于CMSIS的应用程序结构：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f2e8eaa6c6e649bfaeb6e00cf78cace4.png)


CMSIS 分为 3 个基本功能层：


1. 内核外设访问层：ARM 公司提供的访问，定义处理器内部寄存器地址以及功能函数。
2. 中间件访问层：定义访问中间件的通用 API，也是 ARM 公司提供。
3. 器件外设访问层：定义硬件寄存器的地址以及外设的访问函数。


可以看出，CMSIS 层在整个系统中是处于中间层，向下负责与内核和各个外设直接打交道，向上提供实时操作系统用户程序调用的函数接口。通过制定标准，其他公司设计的库函数都得到了规范。例如，CMSIS 规范就规定，系统初始化函数名字必须为 SystemInit。


### 新建工程模板


用Keil新建工程，选择芯片型号STM32F103ZET6：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ae4221fbb4394727910b88c4f1a51366.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/4629caff6fde4a9e944b39d14bb5c37f.png)


到这里，我们还只是建了一个框架，还需要添加启动代码，以及.c 文件等。


![在这里插入图片描述](https://img-blog.csdnimg.cn/78f9056e2075493a9459f089e2c0d13e.png)


创建完成如下，并将相关文件复制到指定文件夹（不赘述）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c83a7e9d75ca4b93926e7dbfef1c2ca1.png)  
 然后进入管理工程，将创建的目录添加进工程：


![在这里插入图片描述](https://img-blog.csdnimg.cn/22931f611d204d92b7306debdb9321fd.png)


然后将相关C代码添加进来：


![在这里插入图片描述](https://img-blog.csdnimg.cn/8e70364ccfe64743aa1a801a54fe9a51.png)


工程目录如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6226b254fee141a9b9879ad39dcbd032.png)


然后编译工程，首先选择中间文件生成目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/db6c9bddb6364de9aa2ca195ee73eed4.png)  
 选择需要包含的头文件目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/af9e10b5ea88411aad7cdc4ee39921e6.png)


另外，库函数在配置和选择外设的时候通过宏定义来选择的，所以我们需要配置一个全局的宏定义变量：`STM32F10X_HD,USE_STDPERIPH_DRIVER`


![在这里插入图片描述](https://img-blog.csdnimg.cn/bf484a57b35e4e3a90d18006de63fe89.png)


然后编译：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f14b386875414e9b8b37359cc959e2b3.png)


工程模板基本建立完毕，然后还需要进行一些额外的配置：


让编译之后能够生成 hex 文件；


![在这里插入图片描述](https://img-blog.csdnimg.cn/a23875b2bc6f42d4aedc08b1a145f622.png)


编译生成hex文件后，此时只接上USB\_232串口就可以下载程序到板上了：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a77a009cbd0f43b5a4ce9a0c8a67e417.png)


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/1e10f1ad0f784eafa2f8d3811d785a11.png)


zdyz还提供了三个函数，我们可以直接加到工程中，后面方便调用：


![在这里插入图片描述](https://img-blog.csdnimg.cn/4c50c7a22c4a4b0eaa5770fd4e193154.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/1ba1c44ea7734738a1da8f101a1a3f03.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0e66773abf114a4c917bf3bde1ab0966.png)


### 程序下载


除了用串口下载hex文件外，还可以通过ST-Link的SW方式下载，这种方式比较快，可以实时跟踪调试，推荐使用。


设置好使用ST-Link及相关频率：


![在这里插入图片描述](https://img-blog.csdnimg.cn/422a7cc18f2749c08241ce0cad8b9c2f.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/10c36ff8f5bd4e28b1394eb75693ea9e.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/203a343e7e594727bea680b56e6781bd.png)


并设置好Dialog DLL，以支持STM32的软硬件仿真：


![在这里插入图片描述](https://img-blog.csdnimg.cn/009b6e991bbf4985a74b801a7c51f311.png)  
 此外，还要安装ST-Link的驱动，否则会显示找不到目标器件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/715c597fa96340289e6a4999cb95641a.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/94749df487744fccb6f9ca3800c40eef.png)


以上。





