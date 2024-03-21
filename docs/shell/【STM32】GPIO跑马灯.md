






最近入手了正点原子的STM32精英开发板，记录一下自己学习单片机开发的过程。


自动化专业毕业后基本上有两个选择，一是做嵌入式开发，常见的有家电、消费电子、机械电子等；二是做PLC开发，如机床，工厂等的自动化。而嵌入式开发的基础又是单片机，单片机也是汽车电控的基础。


对于开发板的选择也多说两句，某宝有好多家卖的，知名的有正点原子和野火，都可以选择，这里我的是原子STM32精英，记得一定要买下载器，有能力的买个LCD屏，消息都可以打印出来，视觉效果会好一点，当然更有能力的买个全家桶更好（带各种外设）。


STM32的学习与51类似，先了解硬件，装好软件，调完例程，在这之后才算是入门了，后续就可以自己搭一些硬件电路，写代码，不断去修改和完善工程了。


大多数人已经习惯库函数编程，因此在这里也统一用**库函数**编程。


好了，废话不多说，单片机的学习是一门实践科学，所以大家拿到硬件，装好相关软件（MDK、ST-Link、CH340驱动、串口助手）后就跟着我一起来新建一个跑马灯工程吧。




#### 文章目录


* + [GPIO认识](#GPIO_13)
	+ [硬件电路](#_78)
	+ [软件编写](#_83)




### GPIO认识


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





