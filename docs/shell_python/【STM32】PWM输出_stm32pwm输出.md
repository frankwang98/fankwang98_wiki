






PWM输出学习。  
 



#### 文章目录


* + [PWM认识](#PWM_2)
	+ [硬件电路](#_25)
	+ [软件编写](#_34)




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





