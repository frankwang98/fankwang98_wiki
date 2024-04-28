
> 嵌入式系统（Embedded System），是一种嵌入机械或电气系统内部、具有专一功能和实时计算性能的计算机系统。

### 1.嵌入式系统介绍

嵌入式系统是一种特殊的计算机系统，被嵌入到其他设备或系统中，以执行专门的任务。这些系统通常备有较小、低功耗、高性能和实时响应等特点。


嵌入式系统广泛应用于各个领域，包括消费电子、医疗设备、工业自动化、交通运输、军事和航空航天等等。举例来说，智能手机、数字相机、车载导航和嵌入在汽车控制单元中的电子系统都是嵌入式系统的应用。


嵌入式系统通常由硬件和软件两部分组成。硬件部分包括处理器、内存、IO接口和外围设备等。而软件部分则包括操作系统、应用程序和设备驱动程序等。


嵌入式系统的开发需要综合考虑硬件和软件的设计，以及系统性能和可靠性等方面。因此，嵌入式系统的开发一般需要多学科知识的综合运用。


嵌入式系统通过外设与外部通信：



> 
> **串口**：RS-232、RS-422、RS-485等  
>  **同步串口**：I2C、SPI、ESSI等  
>  **USB**：usb模块  
>  **多媒体卡**：SD卡、CF卡等  
>  **网络**：以太网、LonWorks等  
>  **现场总线**：CAN总线、LIN总线、PROFIBUS等  
>  **定时器**：PLL、捕获比较模块和时间处理单元  
>  **分立IO**：GPIO  
>  **ADC/DAC**：AD  
>  **调试接口**：JTAG、ISP、ICSP、BDM端口、BITP、DP9端口等
> 
> 
> 


### 2.嵌入式软件架构


常用的嵌入式软件架构有几种不同的基本类型。


1.控制循环


在这种设计中，软件有一个简单的循环，这个循环调用各个子程序，每个子程序管理硬件或者软件的某一部分。中断通常用来设置标记或者更新软件其他部分能够读取的寄存器。


系统使用简单的API来完成允许和禁止中断设置。如果处理得当的话，它能够在嵌套子程序中处理嵌套调用，在最外面的中断允许嵌套中恢复前面的中断状态。这种方法是实现Exokernel的一个最简单的方法。


通常在循环中有一些子程序使用周期性的实时中断控制一组软件定时器，当一个定时器时间到的时候就会运行相应的子程序或者设置相应的标志。


2.非抢先式任务


非抢先式任务系统非常类似于上面的系统，只是这个循环是隐藏在API中的。我们定义一系列的任务，每个任务获得自己的子程序栈；然后，当一个任务空闲的时候，它调用一个空闲子程序（通常调用“暂停”、“等候”、“交出（yield）”等等）。


带有类似属性的架构都带有一个事件队列，有一个循环根据队列列表中的一个域确定删除时间和调用子程序。


这种架构的优点和缺点都非常类似于控制环，只是这种方法添加新的软件更加简单，只需要简单地编写新的任务或者将它添加到队列解释器中。


3.抢先式定时器


使用上面的任何一种系统，但是添加一个按照定时器中断运行子程序的定时器系统，这样就给系统添加了崭新的能力，这样定时器子程序第一次能在一个有保证的时间内运行。


另外，代码第一次能够在非预期的时间访问自己的数据结构。定时器子程序必须要象中断子程序一样进行处理。


4.抢先式任务


使用上面的非抢先式任务系统，从一个抢先式定时器或者其他中断运行。


这样系统就突然变得很不一样了。任何一个任务的代码都有可能损害其他任务的数据，所以它们必须进行切缺的切分。对于共享数据的访问必须使用一些同步策略进行控制，如消息队列、信号灯或者非阻塞同步机制。


5.微内核与外内核


这种方法试图将系统组织得比宏内核更易于配置，而同时提供类似的特点。


微内核是实时操作系统的一个逻辑发展，通常的组织方式是操作系统内核分配内存并且将CPU在不同的线程之间进行切换。用户模式的进程实现如文件系统、用户接口等主要的功能。


微内核在二十世纪五十年代开始首次尝试，但是由于计算机在任务间切换以及在任务间交换数据速度非常缓慢，所以人们放弃了微内核而钟情于MULTICS和UNIX风格的大内核。总体上来说，微内核在任务切换以及任务间通信速度快的时候是比较成功的，在速度慢的时候是失败的。


外内核通过使用普通的子程序调用获得的通信效率很高，硬件以及系统中的软件都是程序员能用也能扩展的。资源内核（可能是库的一部分）分配CPU时间、内存以及其他资源。如多任务、网络以及文件系统这样的大内核特性通过代码库来提供。库可以进行动态的连接、扩展或者共享。不同的应用甚至可以使用的不同的库，但是所有的资源都来自于资源内核。


6.虚拟机


一些航空电子系统使用几个商用计算机。这样更进一步，每个计算机都在模拟它们自身的几个副本，重要的程序同时在几个计算机上运行并且进行投票控制。


模拟环境的优点就是即使一个计算机出现故障，软件的不同例程能够迁移到正常工作的软件分区，表决的票数并不受影响。


通常虚拟软件运行在计算机的用户模式下，它捕捉、模拟硬件访问和不在用户模式下运行的指令。


### 3.嵌入式学习路线



> 
> 1.计算机基础知识：首先需要掌握计算机基础知识，包括数据结构、计算机组成原理、操作系统和编程语言等。
> 
> 
> 2.硬件设计：了解嵌入式系统的硬件设计原理，学习电子电路设计、数字信号处理和模拟信号处理等知识。
> 
> 
> 3.嵌入式系统架构设计：熟悉各种微处理器、嵌入式系统体系结构以及各种外部设备接口标准，如串口、网络、USB、SPI和I2C等。
> 
> 
> 4.嵌入式软件开发：学习嵌入式系统软件开发，掌握C/C++语言和汇编语言，了解RTOS实时操作系统的原理以及驱动程序的编写方法。
> 
> 
> 5.集成开发环境：了解常见的嵌入式集成开发环境，例如Keil、IAR、Code Composer Studio等。
> 
> 
> 6.实践项目经验：通过完成嵌入式系统项目，例如开发智能家居、智能穿戴设备或者自动化控制系统等项目，来积累实践经验和技能。
> 
> 
> 


![在这里插入图片描述](https://img-blog.csdnimg.cn/2d326f43576842acaff8ac1da0d8b6cb.png)


以上。

### MDK5软件介绍


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

### 硬件资源介绍
以精英板STM32F103为例。STM32是Cortex M3架构，拥有更强劲的性能、更高的代码密度、位带操作、可嵌套中断、低成  
 本、低功耗等众多优势。


了解架构方面的知识可以查看以下文档：


* 《STM32 参考手册》中文版 V10.0
* 《Cortex-M3 权威指南》中文版（宋岩 译）


STM32 拥有非常多的寄存器，对于新手来说，直接操作寄存器有很大的难度，所以 ST 官方提供了一套固件库函数，方面开发者进行程序编写，库函数入门后，最好也对寄存器操作有所了解。 

精英板STM32的硬件资源如下（尺寸115mm\*117mm）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0de65593e93244f3ac49f50782b6e52e.png)


板载资源详细如下：  
 ◆ CPU：STM32F103ZET6，LQFP144（CPU封装1.4mm厚），FLASH：512K（闪存，结合了ROM和RAM的长处），SRAM：64K（静态随机存取存储器）；  
 ◆ 外扩 SPI FLASH：W25Q128，16M 字节（存储经常读取的数据）  
 ◆ 1 个电源指示灯（蓝色PWR）  
 ◆ 2 个状态指示灯（DS0：红色，DS1：绿色）  
 ◆ 1 个红外接收头，并配备一款小巧的红外遥控器  
 ◆ 1 个 EEPROM 芯片，24C02，容量 256 字节  
 ◆ 1 个光敏传感器  
 ◆ 1 个无线模块接口（可接 NRF24L01/RFID 模块等）  
 ◆ 1 路 CAN 接口，采用 TJA1050 芯片  
 ◆ 1 路 485 接口，采用 SP3485 芯片  
 ◆ 1 路数字温湿度传感器接口，支持 DS18B20 /DHT11 等  
 ◆ 1 个 ATK 模块接口，支持 ALIENTEK 蓝牙/GPS 模块/MPU6050 模块等  
 ◆ 1 个标准的 2.4/2.8/3.5/4.3/7 寸 LCD 接口，支持触摸屏  
 ◆ 1 个摄像头模块接口  
 ◆ 1 个 OLED 模块接口（与摄像头接口共用）  
 ◆ 1 个 USB 串口，可用于程序下载和代码调试（USMART 调试）（USB\_232）  
 ◆ 1 个 USB SLAVE 接口，用于 USB 通信（USB\_SLAVE）  
 ◆ 1 个有源蜂鸣器  
 ◆ 1 个 RS485 选择接口  
 ◆ 1 个 CAN/USB 选择接口  
 ◆ 1 个串口选择接口  
 ◆ 1 个 SD 卡接口（在板子背面，SDIO 接口）  
 ◆ 1 个标准的 JTAG/SWD 调试下载口（20针）  
 ◆ 1 组 AD/DA 组合接口（DAC/ADC/ TPAD）  
 ◆ 1 组 5V 电源供应/接入口  
 ◆ 1 组 3.3V 电源供应/接入口  
 ◆ 1 个直流电源输入接口（输入电压范围：6~24V）  
 ◆ 1 个启动模式选择配置接口  
 ◆ 1 个 RTC 后备电池座，并带电池  
 ◆ 1 个复位按钮，可用于复位 MCU 和 LCD  
 ◆ 3 个功能按钮，其中 KEY\_UP 兼具唤醒功能  
 ◆ 1 个电容触摸按键  
 ◆ 1 个电源开关，控制整个板的电源  
 ◆ 独创的一键下载功能  
 ◆ 除晶振占用的 IO 口外，其余所有 IO 口全部引出


开发板的核心芯片（U1），型号为：STM32F103ZET6。该芯片具有 64KB SRAM、512KB FLASH、2 个基本定时器、4 个通用定时器、2 个高级定时器、2 个 DMA 控制器（共 12 个通道）、3 个 SPI、2 个 IIC、5 个串口、1 个 USB、1 个 CAN、3 个 12 位 ADC、1 个 12 位 DAC、1 个SDIO 接口、1 个 FSMC 接口以及 112 个通用 IO 口。


最好跟着原理图一个个都认识一遍：


![在这里插入图片描述](https://img-blog.csdnimg.cn/31e3057ad26e4c389fe03a937bae94f9.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/43200806a16e4f23ab79b774370c86c9.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/5f6f5f058ba244009e331b122aedfe3c.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/7aae69f27d254ecabff0d6f93aa5df47.png)


此外，在编写程序的时候，可以再对着引脚IO表再强化一遍认知。针对具体的功能，可以回过头再来详细查看模块的说明。


### STM32学习方法


STM32 作为目前最热门的 ARM Cortex M3 处理器，正在被越来越多的公司选择使用。没有学过51的也可以直接上手STM32，万事开头难，可以先通过例程进行学习，找到自己点亮一个LED灯的乐趣，然后再熟悉外设模块，实时系统等。下面是几个学习STM32的要点：


1. 选择一款合适的开发板作为软件载体；
2. 两本参考资料，即《STM32 中文参考手册》和《Cortex-M3 权威指南》；
3. 掌握方法，勤学慎思。


以上。

### 编辑器配置
#### 文本美化


设置编码格式为UTF8（ANSI会出现中文乱码，Chinese太丑），缩进为4；


![在这里插入图片描述](https://img-blog.csdnimg.cn/ad544511491943c3b77a9d5721305f43.png)


如果其他文件有乱码，可以在notepad++打开并转码为UTF8即可；


![在这里插入图片描述](https://img-blog.csdnimg.cn/e568fa864f6043eda92402af0f0b258b.png)


改变元素的颜色，这里将数字的颜色高亮为红色；


![在这里插入图片描述](https://img-blog.csdnimg.cn/661fa5c9127543499306abc87df05581.png)


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b328fd0d4b0744babae0c3b2e14677bf.png)


将用户自定义关键字定义为蓝色，并加入用户关键字；


![在这里插入图片描述](https://img-blog.csdnimg.cn/cbd7cab1cf9049e5a7600db9c2252dbf.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/74c6334647fd4e52990675670d6c2a80.png)


#### 语法检测&代码提示


设置输入3个字符后开始代码提示和语法检测；


![在这里插入图片描述](https://img-blog.csdnimg.cn/9556d0c1b2284b79b529f357fd525e33.png)


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/38f87a0b42e64b6a94aa4b98817f2b29.png)


#### 代码编辑技巧


除了TAB缩进外，还可以右键直接找到函数定义的位置：


![在这里插入图片描述](https://img-blog.csdnimg.cn/3bd18beeba984fb58b45408c9a9acba0.png)


此外，还可以快速多行注释和取消注释：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7237dc712a5a42a39672ed9c8c90baca.png)


以上。

### C语言基础-位操作


计算机的位是Bit，即数字在计算机中的二进制表示（0和1）。十六进制用于缩写二进制，将二进制从后向前每4位数转换为1位十六进制。


C语言支持6种位操作：


![在这里插入图片描述](https://img-blog.csdnimg.cn/60012290ca1946afb89189acb88954a8.png)


#### 1.对某些位进行设值（& |）


改变 GPIOA 的状态，对寄存器的值进行&清零操作：



```
GPIOA->CRL&=0XFFFFFF0F; //将第 4-7 位清 0

```

再与需要设置的值进行|或运算：



```
GPIOA->CRL|=0X00000040; //设置相应位的值，不改变其他位的值

```

#### 2.移位操作提高代码可读性


某些时候，直接赋值操作可读性不强，就是看代码的人不知道你要干什么，而移位操作就能很清晰地表达想法：


如将 BSRR 寄存器的第 pinpos 位设置为 1：



```
GPIOx->BSRR = (((uint32\_t)0x01) << pinpos);

```

再如将第6个端口设置高位：



```
GPIOA->ODR|=1<<5; //PA.5 输出高,不改变其他位

```

5 告诉我们是第 5 位也就是第 6 个端口，1 告诉我们是设置为 1 了。


#### 3.取反操作


SR 寄存器的每一位都代表一个状态，某个时刻我们希望去设置某一位的值为 0，同时其他位都保留为 1，简单的作法是直接给寄存器设置一个值：



```
TIMx->SR=0xFFF7；	//第3位设置为0

```

但这样可读性差，在库函数中可以这样表示：



```
TIMx->SR = (uint16\_t)~TIM_FLAG;	//TIM\_FLAG是宏定义的值

```

这样可以直接对第3位取0。


### 宏定义


define 是 C 语言中的预处理命令，它用于宏定义，可以提高源代码的可读性，为编程提供方便。格式如下：



```
#define 标识符 字符串
#define SYSCLK\_FREQ\_72MHz 72000000 //定义72MHz

```

### ifdef条件编译


当满足条件时对一组语句进行编译，否则对另一段语句进行编译：



```
#ifdef 标识符
程序段 1 
#else 
程序段 2 
#endif

```

### extern变量声明


C 语言中 extern 可以置于变量或者函数前，以表示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。这里面要注意，对于 extern 申明变量可以多次，但定义只有一次。如：



```
//定义
u8 id;//定义只允许一次
main()
{
id=1;
printf("d%",id);//id=1
test();
printf("d%",id);//id=2
}

```


```
//申明
extern u8 id;//申明变量 id 是在外部定义的，申明可以在很多个文件中进行
void test(void){
id=2;
}

```

### typedef类型别名


typedef 用于为现有类型创建一个新的名字，或称为类型别名，用来简化变量的定义。typedef 在 MDK 用得最多的就是定义结构体的类型别名和枚举类型。



```
//定义结构体变量
typedef struct
{
 __IO uint32\_t CRL;
 __IO uint32\_t CRH;
…
} GPIO_TypeDef;

```


```
GPIO_TypeDef _GPIOA,_GPIOB;

```

### 结构体


结构体简单来说就是将不同类型的变量组合在一起。定义如下：



```
Struct 结构体名{
成员列表;
}变量名列表；

```

例如，申明结构体并定义变量：



```
Struct U_TYPE {
Int BaudRate
Int WordLength; 
}usart1,usart2;

```

以上。

### GPIO认识-跑马灯
最近入手了正点原子的STM32精英开发板，记录一下自己学习单片机开发的过程。


自动化专业毕业后基本上有两个选择，一是做嵌入式开发，常见的有家电、消费电子、机械电子等；二是做PLC开发，如机床，工厂等的自动化。而嵌入式开发的基础又是单片机，单片机也是汽车电控的基础。


对于开发板的选择也多说两句，某宝有好多家卖的，知名的有正点原子和野火，都可以选择，这里我的是原子STM32精英，记得一定要买下载器，有能力的买个LCD屏，消息都可以打印出来，视觉效果会好一点，当然更有能力的买个全家桶更好（带各种外设）。


STM32的学习与51类似，先了解硬件，装好软件，调完例程，在这之后才算是入门了，后续就可以自己搭一些硬件电路，写代码，不断去修改和完善工程了。


大多数人已经习惯库函数编程，因此在这里也统一用**库函数**编程。


好了，废话不多说，单片机的学习是一门实践科学，所以大家拿到硬件，装好相关软件（MDK、ST-Link、CH340驱动、串口助手）后就跟着我一起来新建一个跑马灯工程吧。

STM32 的 IO 口相比 51 而言要复杂得多，所以使用起来也困难很多。首先 STM32 的 IO 口可以由软件配置成以下 8 种模式：


1、输入浮空  
 2、输入上拉  
 3、输入下拉  
 4、模拟输入  
 5、开漏输出  
 6、推挽输出  
 7、推挽式复用功能  
 8、开漏复用功能


推挽输出的最大特点是可以真正的输出高电平和低电平，在两种电平下都具有驱动能力，一般用于挂负载。而开漏输出无法真正输出高电平，即高电平时没有驱动能力，需要借助外部上拉电阻完成对外驱动。（具体原理请自行了解）


每个IO口都是5V兼容，由 7 个寄存器来控制（详细寄存器配置请查看参考手册）。CRL 和 CRH 控制着每个 IO 口的模式及输出速率。


STM32输出配置模式如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2a24624890c34c7680da152ee2415759.png)


端口低配置寄存器CRL各位描述（CRH作用类似）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e7f6a3b46ade468dbf3d339c265f99de.png)


这里我们记住几个常用的配置就好，如 0X0 表示模拟输入模式（ADC 用）、0X3 表示推挽输出模式（做输出口用，50M 速率）、0X8 表示上/下拉输入模式（做输入口用）、0XB 表示复用推挽输出（使用 IO 口的第二功能，50M 速率）。


下面看一个库函数初始化GPIO的实例：



```
GPIO_InitTypeDef GPIO_InitStructure;
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5; //LED0-->PB.5 端口配置
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; //推挽输出
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;//速度 50MHz
GPIO\_Init(GPIOB, &GPIO_InitStructure);//根据设定参数配置 GPIO

```

设置 GPIOB 的第 5 个端口为推挽输出模式，同时速度为 50M。从上面初始化代码可以看出，结构体 GPIO\_InitStructure 的第一个成员变量 GPIO\_Pin 用来设置是要初始化哪个或者哪些 IO 口；第二个成员变量 GPIO\_Mode 是用来设置对应 IO 端口的输出输入模式，这些模式是上面我们讲解的 8 个模式，在 MDK 中是通过一个枚举类型定义的：



```
typedef enum
{ GPIO_Mode_AIN = 0x0, //模拟输入
 GPIO_Mode_IN_FLOATING = 0x04, //浮空输入
 GPIO_Mode_IPD = 0x28, //下拉输入
 GPIO_Mode_IPU = 0x48, //上拉输入
 GPIO_Mode_Out_OD = 0x14, //开漏输出
 GPIO_Mode_Out_PP = 0x10, //通用推挽输出
 GPIO_Mode_AF_OD = 0x1C, //复用开漏输出
 GPIO_Mode_AF_PP = 0x18 //复用推挽
}GPIOMode_TypeDef;

```

第三个参数是 IO 口速度设置，有三个可选值，在 MDK 中同样是通过枚举类型定义：



```
typedef enum
{ 
 GPIO_Speed_10MHz = 1,
 GPIO_Speed_2MHz, 
 GPIO_Speed_50MHz
}GPIOSpeed_TypeDef;

```

总的来说，IO的操作步骤如下：


1） 使能 IO 口时钟。调用函数为 RCC\_APB2PeriphClockCmd()；  
 2） 初始化 IO 参数。调用函数 GPIO\_Init()；  
 3） 操作 IO。如调用函数GPIO\_SetBits()。


### 硬件电路


STM32F1中两个LED灯的连接如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6b8e76d94b2e4741ae5f0d79ab8958de.png)


### 软件编写


首先，复制一个工程模板，在模板中，一些相关的STM32库和系统文件（sys、delay、usart）已经添加好了，需要我们做的就是写好外设函数，然后再用户主函数main.c中调用，工程树如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/bea208e088e1486a9049c60659d5bb2f.png)  
 在跑马灯工程中，首要要编写好led.c灯的外设驱动，添加到HARDWARE组中，这样方便每个外设模块化，代码如下：  
 **led.h**



```
#ifndef \_\_LED\_H
#define \_\_LED\_H
#include "sys.h"
//LED 端口定义
#define LED0 PBout(5)// DS0
#define LED1 PEout(5)// DS1
void LED\_Init(void);//初始化 
#endif


```

**led.c**



```
#include "led.h"
//初始化 PB5 和 PE5 为输出口(推挽输出).并使能这两个口的时钟 
//LED IO 初始化
void LED\_Init(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;
	RCC\_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB|
	RCC_APB2Periph_GPIOE, ENABLE); //使能 PB,PE 端口时钟
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5; //LED0-->PB.5 推挽输出
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; //推挽输出
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO\_Init(GPIOB, &GPIO_InitStructure);
	GPIO\_SetBits(GPIOB,GPIO_Pin_5); //PB.5 输出高
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5; //LED1-->PE.5 推挽输出
	GPIO\_Init(GPIOE, &GPIO_InitStructure);
	GPIO\_SetBits(GPIOE,GPIO_Pin_5); //PE.5 输出高
}


```

**main.c主程序**



```
#include "led.h"
#include "delay.h"
#include "sys.h"

int main(void)
{
	delay\_init(); //延时函数初始化 
	LED\_Init(); //初始化与 LED 连接的硬件接口
	while(1)
	{ 
		LED0=0; 
		LED1=1;
		delay\_ms(300); //延时 300ms
		LED0=1;
		LED1=0;
		delay\_ms(300); //延时 300ms
	} 
}


```

代码编写完成后，编译完后可以先在软件中仿真一下，然后就是通过仿真器或者串口下载到板子上了。


![在这里插入图片描述](https://img-blog.csdnimg.cn/b82e7872e4fb4820bb40fa3a5050d1a7.png)


现象：精英板的DS0和DS1交替闪烁。


**以下是工程源文件：**


1.库函数工程模板：



> 
> 链接：https://pan.baidu.com/s/15QvMvPkkNjd\_IEVdT8E-aw  
>  提取码：j6xn
> 
> 
> 


2.跑马灯工程：



> 
> 链接：https://pan.baidu.com/s/1Xfm2QvDvzaAsKc7cxeuqvQ  
>  提取码：6w9c
> 
> 
> 


以上。

### PWM认识


PWM是“Pulse Width Modulation”的缩写，即脉冲宽度调制，简称脉宽调制。是利用微处理器的数字输出来对模拟电路进行控制的一种非常有效的技术。简单来说，就是对脉冲宽度的控制。


![在这里插入图片描述](https://img-blog.csdnimg.cn/cce536304c804867a3859d9bb0c86b01.png)


这里会用到定时器，STM32有多个定时器，这里我们仅利用 TIM3的 CH2 产生一路 PWM 输出。如果要产生多路输出，请查阅文档。


要使 STM32 的通用定时器 TIMx 产生 PWM 输出，还需要用到 3 个寄存器：捕获/比较模式寄存器  
 （TIMx\_CCMR1/2）、捕获/比较使能寄存器（TIMx\_CCER）、捕获/比较寄存器（TIMx\_CCR1~4）。简单来说，通过修改捕获/比较寄存器的值就能控制PWM的输出脉宽。


这个例子我们使用的是 TIM3的通道 2，所以我们需要修改 `TIM3_CCR2` 以实现脉宽控制 DS0 的亮度。


TIM3\_CH2 默认是接在 PA7上面的，而我们的 DS0 接在 PB5 上面，普通MCU是需要飞线的，而STM32有重映射功能，可以将TIM3\_CH2映射到 PB5 上。


STM32 的重映射控制是由复用重映射和调试 IO 配置寄存器（AFIO\_MAPR）控制的。自己编程序时可查看重映射控制表。


PWM有两个重要参数：  
 arr-自动装载值  
 psc-预分频数，即对时钟频率的分频，去顶定时时长


eg：stm32时钟频率为72MHz，设计定时100ms。  
 分析：设置预分频为：7199，则72M/(7199+1)=10KHz,即一秒计数10K次，想要定时100ms，则自动重装值为10K\*0.1s - 1=999。


### 硬件电路


这里，我们将使用 TIM3 的通道 2，把通道 2 重映射到 PB5，产生 PWM 来控制 DS0 的亮度，实现PWM呼吸灯的效果。


硬件资源：


* 指示灯DS0（低电平亮）
* 定时器TIM3（重映射PB5，连接DS0）


![在这里插入图片描述](https://img-blog.csdnimg.cn/7e20f8c1a92f4f7a91a71a7421b0e918.png)


### 软件编写


PWM 相关的函数设置在库函数文件 stm32f10x\_tim.h 和 stm32f10x\_tim.c文件中。


timer.h



```
#ifndef \_\_TIMER\_H
#define \_\_TIMER\_H
#include "sys.h"

void TIM3\_Int\_Init(u16 arr,u16 psc);
void TIM3\_PWM\_Init(u16 arr,u16 psc);
#endif


```

timer.c



```
#include "timer.h"
#include "led.h"
#include "usart.h"
   	  
//通用定时器3中断初始化
//这里时钟选择为APB1的2倍，而APB1为36M
//arr：自动重装值。
//psc：时钟预分频数
//这里使用的是定时器3!
void TIM3\_Int\_Init(u16 arr,u16 psc)
{
  TIM_TimeBaseInitTypeDef  TIM_TimeBaseStructure;
	NVIC_InitTypeDef NVIC_InitStructure;

	RCC\_APB1PeriphClockCmd(RCC_APB1Periph_TIM3, ENABLE); //时钟使能

	TIM_TimeBaseStructure.TIM_Period = arr; //设置在下一个更新事件装入活动的自动重装载寄存器周期的值 计数到5000为500ms
	TIM_TimeBaseStructure.TIM_Prescaler =psc; //设置用来作为TIMx时钟频率除数的预分频值 10Khz的计数频率 
	TIM_TimeBaseStructure.TIM_ClockDivision = 0; //设置时钟分割:TDTS = Tck\_tim
	TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;  //TIM向上计数模式
	TIM\_TimeBaseInit(TIM3, &TIM_TimeBaseStructure); //根据TIM\_TimeBaseInitStruct中指定的参数初始化TIMx的时间基数单位
 
	TIM\_ITConfig(TIM3,TIM_IT_Update,ENABLE ); //使能指定的TIM3中断,允许更新中断

	NVIC_InitStructure.NVIC_IRQChannel = TIM3_IRQn;  //TIM3中断
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;  //先占优先级0级
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 3;  //从优先级3级
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE; //IRQ通道被使能
	NVIC\_Init(&NVIC_InitStructure);  //根据NVIC\_InitStruct中指定的参数初始化外设NVIC寄存器

	TIM\_Cmd(TIM3, ENABLE);  //使能TIMx外设
							 
}
//定时器3中断服务程序
void TIM3\_IRQHandler(void)   //TIM3中断
{
	if (TIM\_GetITStatus(TIM3, TIM_IT_Update) != RESET) //检查指定的TIM中断发生与否:TIM 中断源 
		{
		TIM\_ClearITPendingBit(TIM3, TIM_IT_Update  );  //清除TIMx的中断待处理位:TIM 中断源 
		LED1=!LED1;
		}
}

//TIM3 PWM部分初始化 
//PWM输出初始化
//arr：自动重装值
//psc：时钟预分频数
void TIM3\_PWM\_Init(u16 arr,u16 psc)
{  
	GPIO_InitTypeDef GPIO_InitStructure;
	TIM_TimeBaseInitTypeDef  TIM_TimeBaseStructure;
	TIM_OCInitTypeDef  TIM_OCInitStructure;
	

	RCC\_APB1PeriphClockCmd(RCC_APB1Periph_TIM3, ENABLE);	//使能定时器3时钟
 	RCC\_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB  | RCC_APB2Periph_AFIO, ENABLE);  //使能GPIO外设和AFIO复用功能模块时钟
	
	GPIO\_PinRemapConfig(GPIO_PartialRemap_TIM3, ENABLE); //Timer3部分重映射 TIM3\_CH2->PB5 
 
   //设置该引脚为复用输出功能,输出TIM3 CH2的PWM脉冲波形 GPIOB.5
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5; //TIM\_CH2
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;  //复用推挽输出
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO\_Init(GPIOB, &GPIO_InitStructure);//初始化GPIO
 
   //初始化TIM3
	TIM_TimeBaseStructure.TIM_Period = arr; //设置在下一个更新事件装入活动的自动重装载寄存器周期的值
	TIM_TimeBaseStructure.TIM_Prescaler =psc; //设置用来作为TIMx时钟频率除数的预分频值 
	TIM_TimeBaseStructure.TIM_ClockDivision = 0; //设置时钟分割:TDTS = Tck\_tim
	TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;  //TIM向上计数模式
	TIM\_TimeBaseInit(TIM3, &TIM_TimeBaseStructure); //根据TIM\_TimeBaseInitStruct中指定的参数初始化TIMx的时间基数单位
	
	//初始化TIM3 Channel2 PWM模式 
	TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM2; //选择定时器模式:TIM脉冲宽度调制模式2
 	TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable; //比较输出使能
	TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High; //输出极性:TIM输出比较极性高
	TIM\_OC2Init(TIM3, &TIM_OCInitStructure);  //根据T指定的参数初始化外设TIM3 OC2

	TIM\_OC2PreloadConfig(TIM3, TIM_OCPreload_Enable);  //使能TIM3在CCR2上的预装载寄存器
 
	TIM\_Cmd(TIM3, ENABLE);  //使能TIM3
	

}


```

main函数如下：



```
 int main(void)
 {		
 	u16 led0pwmval=0;	//PWM比较值
	u8 dir=1;	
	delay\_init();	    	 //延时函数初始化 
	NVIC\_PriorityGroupConfig(NVIC_PriorityGroup_2); 	 //设置NVIC中断分组2:2位抢占优先级，2位响应优先级
	uart\_init(115200);	 //串口初始化为115200
 	LED\_Init();			     //LED端口初始化
 	TIM3\_PWM\_Init(899,0);	 //不分频。PWM频率=72000000/900=80Khz
   	while(1)
	{
 		delay\_ms(10);	 
		if(dir)led0pwmval++;
		else led0pwmval--;

 		if(led0pwmval>300)dir=0;
		if(led0pwmval==0)dir=1;										 
		TIM\_SetCompare2(TIM3,led0pwmval);		   
	}	 
 }

```

以上。

### CAN认识


CAN通讯是车辆底盘域的主要通信方式，1986年由博世开发，CAN控制器根据双绞线上的电位差来判断总线电平（显性/隐性），通过电平的变化，实现消息（报文）的发送。


CAN通信具有以下特点：


* 多主控制：即底盘网络内的所有ECU单元均可发送消息。
* 通信速度较快，通信距离远：最高1Mbps（距离小于40m），最远可达10KM（速率低于5Kbps）。
* 可连接节点多。


一般而言，125Kbps以下速率的称为低速CAN通信，125Kbps-1Mbps的称为高速CAN通信。


为了保持通信稳定，在CAN网络的两端需要并联2个120欧电阻，使得总线电阻保持在60欧左右。


显性电平对应逻辑0，CANH和CANL之差为2.5V左右；隐形电平对应逻辑1，CANH和CANL之差为0V。


CAN协议有5种类型的帧：


* 数据帧：用于发送单元向接收单元传送数据的帧（标准-11位和扩展-29位）
* 遥控帧：用于接收单元向具有相同 ID 的发送单元请求数据的帧
* 错误帧：用于当检测出错误时向其它单元通知错误的帧
* 过载帧：用于接收单元通知其尚未做好接收准备的帧
* 间隔帧：用于将数据帧及遥控帧与前面的帧分离开来的帧


数据帧一般由7个段组成：


* 帧起始：表示数据帧开始的段。
* 仲裁段：表示该帧优先级的段。
* 控制段：表示数据的字节数及保留位的段。
* 数据段：数据的内容，一帧可发送 0~8 个字节的数据。从最高位（MSB）开始输出。
* CRC段：检查帧的传输错误的段。
* ACK段：表示确认正常接收的段。
* 帧结束：表示数据帧结束的段。


数据段有Intel和Motorola两种格式。


STM32的位时序图：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0a7585ddeded452591bf1cd3245e5154.png)  
 我们只需要知道 BS1 和 BS2 的设置，以及 APB1的时钟频率（一般为 36Mhz），就可以方便的计算出波特率。比如设置 TS1=6、TS2=7 和 BRP=4，在 APB1 频率为 36Mhz 的条件下，即可得到 CAN 通信的波特率=36000000/[(7+8+1)*5]=450Kbps。


STM32 提供了两种测试模式，环回模式和静默模式。


### 硬件电路


通过 WK_UP 按键选择 CAN 的工作模式（正常模式/环回模式），然后通过 KEY0控制数据发送，并通过查询的办法，将接收到的数据显示在 LCD 模块上。如果是环回模式，我们不需要 2 个开发板。如果是正常模式，我们就需要 2 个精英开发板，并且将他们的 CAN 接口对接起来（高对高，低对低），然后一个开发板发送数据，另外一个开发板将接收到的数据显示在 LCD 模块上。


另外，也可以用USB-CAN分析仪来测试。


硬件资源：  
 1） 指示灯 DS0  
 2） KEY0 和 KEY_UP 按键  
 3） TFTLCD 模块  
 4） CAN  
 5） CAN 收发芯片 JTA1050


![在这里插入图片描述](https://img-blog.csdnimg.cn/2f259da58de8484696faa4d025de8558.png)


另外，我们要改变开发板上 P6 排针的连接，通过跳线帽将 PA11 和 PA12 分别连接到 CRX 和 CTX 上面。


### 软件程序


* CAN_Mode_Init 函数用于CAN初始化，该函数带有 5 个参数，可以设置 CAN 通信的波特率和工作模式等；
* Can_Send_Msg 函数。该函数用于 CAN 报文的发送，主要是设置标识符 ID等信息，写入数据长度和数据，并请求发送，实现一次报文的发送。
* Can_Receive_Msg 函数。用来接受数据并且将接受到的数据存放到 buf 中。


main函数：



```
int main(void)
 {	 
	u8 key;
	u8 i=0,t=0;
	u8 cnt=0;
	u8 canbuf[8];
	u8 res;
	u8 mode=CAN_Mode_Normal;//CAN工作模式;CAN_Mode_Normal(0)：普通模式，CAN_Mode_LoopBack(1)：环回模式

	delay_init();	    	 //延时函数初始化 
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);//设置中断优先级分组为组2：2位抢占优先级，2位响应优先级
	uart_init(115200);	 	//串口初始化为115200
	LED_Init();		  		//初始化与LED连接的硬件接口
	LCD_Init();			   	//初始化LCD 
	KEY_Init();				//按键初始化 
   
	CAN_Mode_Init(CAN_SJW_1tq,CAN_BS2_8tq,CAN_BS1_9tq,4,CAN_Mode_LoopBack);//CAN初始化环回模式,波特率500Kbps 

 	POINT_COLOR=RED;//设置字体为红色 
	LCD_ShowString(60,50,200,16,16,"WarShip STM32");	
	LCD_ShowString(60,70,200,16,16,"CAN TEST");	
	LCD_ShowString(60,90,200,16,16,"ATOM@ALIENTEK");
	LCD_ShowString(60,110,200,16,16,"2015/1/11");
	LCD_ShowString(60,130,200,16,16,"LoopBack Mode");	 
	LCD_ShowString(60,150,200,16,16,"KEY0:Send WK_UP:Mode");//显示提示信息 
  POINT_COLOR=BLUE;//设置字体为蓝色 
	LCD_ShowString(60,170,200,16,16,"Count:");			//显示当前计数值 
	LCD_ShowString(60,190,200,16,16,"Send Data:");		//提示发送的数据 
	LCD_ShowString(60,250,200,16,16,"Receive Data:");	//提示接收到的数据 
 	while(1)
	{
		key=KEY_Scan(0);
		if(key==KEY0_PRES)//KEY0按下,发送一次数据
		{
			for(i=0;i<8;i++)
			{
				canbuf[i]=cnt+i;//填充发送缓冲区
				if(i<4)LCD_ShowxNum(60+i*32,210,canbuf[i],3,16,0X80);	//显示数据
				else LCD_ShowxNum(60+(i-4)*32,230,canbuf[i],3,16,0X80);	//显示数据
 			}
			res=Can_Send_Msg(canbuf,8);//发送8个字节 
			if(res)LCD_ShowString(60+80,190,200,16,16,"Failed");		//提示发送失败
			else LCD_ShowString(60+80,190,200,16,16,"OK ");	 		//提示发送成功 
		}else if(key==WKUP_PRES)//WK_UP按下，改变CAN的工作模式
		{	   
			mode=!mode;
  			CAN_Mode_Init(CAN_SJW_1tq,CAN_BS2_8tq,CAN_BS1_9tq,4,mode);//CAN普通模式初始化, 波特率500Kbps 
			POINT_COLOR=RED;//设置字体为红色 
			if(mode==0)//普通模式，需要2个开发板
			{
				LCD_ShowString(60,130,200,16,16,"Nnormal Mode ");	    
			}else //回环模式,一个开发板就可以测试了.
			{
 				LCD_ShowString(60,130,200,16,16,"LoopBack Mode");
			}
 			POINT_COLOR=BLUE;//设置字体为蓝色 
		}		 
		key=Can_Receive_Msg(canbuf);
		if(key)//接收到有数据
		{			
			LCD_Fill(60,270,130,310,WHITE);//清除之前的显示
 			for(i=0;i<key;i++)
			{									    
				if(i<4)LCD_ShowxNum(60+i*32,270,canbuf[i],3,16,0X80);	//显示数据
				else LCD_ShowxNum(60+(i-4)*32,290,canbuf[i],3,16,0X80);	//显示数据
 			}
		}
		t++; 
		delay_ms(10);
		if(t==20)
		{
			LED0=!LED0;//提示系统正在运行 
			t=0;
			cnt++;
			LCD_ShowxNum(60+48,170,cnt,3,16,0X80);	//显示数据
		}		   
	}
}

```

以上。