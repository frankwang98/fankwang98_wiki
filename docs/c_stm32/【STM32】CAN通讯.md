






学习CAN通讯。




#### 文章目录


* + [CAN认识](#CAN_3)
	+ [硬件电路](#_42)
	+ [软件程序](#_58)




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
 我们只需要知道 BS1 和 BS2 的设置，以及 APB1的时钟频率（一般为 36Mhz），就可以方便的计算出波特率。比如设置 TS1=6、TS2=7 和 BRP=4，在 APB1 频率为 36Mhz 的条件下，即可得到 CAN 通信的波特率=36000000/[(7+8+1)\*5]=450Kbps。


STM32 提供了两种测试模式，环回模式和静默模式。


### 硬件电路


通过 WK\_UP 按键选择 CAN 的工作模式（正常模式/环回模式），然后通过 KEY0控制数据发送，并通过查询的办法，将接收到的数据显示在 LCD 模块上。如果是环回模式，我们不需要 2 个开发板。如果是正常模式，我们就需要 2 个精英开发板，并且将他们的 CAN 接口对接起来（高对高，低对低），然后一个开发板发送数据，另外一个开发板将接收到的数据显示在 LCD 模块上。


另外，也可以用USB-CAN分析仪来测试。


硬件资源：  
 1） 指示灯 DS0  
 2） KEY0 和 KEY\_UP 按键  
 3） TFTLCD 模块  
 4） CAN  
 5） CAN 收发芯片 JTA1050


![在这里插入图片描述](https://img-blog.csdnimg.cn/2f259da58de8484696faa4d025de8558.png)


另外，我们要改变开发板上 P6 排针的连接，通过跳线帽将 PA11 和 PA12 分别连接到 CRX 和 CTX 上面。


### 软件程序


* CAN\_Mode\_Init 函数用于CAN初始化，该函数带有 5 个参数，可以设置 CAN 通信的波特率和工作模式等；
* Can\_Send\_Msg 函数。该函数用于 CAN 报文的发送，主要是设置标识符 ID等信息，写入数据长度和数据，并请求发送，实现一次报文的发送。
* Can\_Receive\_Msg 函数。用来接受数据并且将接受到的数据存放到 buf 中。


main函数：



```
int main(void)
 {	 
	u8 key;
	u8 i=0,t=0;
	u8 cnt=0;
	u8 canbuf[8];
	u8 res;
	u8 mode=CAN_Mode_Normal;//CAN工作模式;CAN\_Mode\_Normal(0)：普通模式，CAN\_Mode\_LoopBack(1)：环回模式

	delay\_init();	    	 //延时函数初始化 
	NVIC\_PriorityGroupConfig(NVIC_PriorityGroup_2);//设置中断优先级分组为组2：2位抢占优先级，2位响应优先级
	uart\_init(115200);	 	//串口初始化为115200
	LED\_Init();		  		//初始化与LED连接的硬件接口
	LCD\_Init();			   	//初始化LCD 
	KEY\_Init();				//按键初始化 
   
	CAN\_Mode\_Init(CAN_SJW_1tq,CAN_BS2_8tq,CAN_BS1_9tq,4,CAN_Mode_LoopBack);//CAN初始化环回模式,波特率500Kbps 

 	POINT_COLOR=RED;//设置字体为红色 
	LCD\_ShowString(60,50,200,16,16,"WarShip STM32");	
	LCD\_ShowString(60,70,200,16,16,"CAN TEST");	
	LCD\_ShowString(60,90,200,16,16,"ATOM@ALIENTEK");
	LCD\_ShowString(60,110,200,16,16,"2015/1/11");
	LCD\_ShowString(60,130,200,16,16,"LoopBack Mode");	 
	LCD\_ShowString(60,150,200,16,16,"KEY0:Send WK\_UP:Mode");//显示提示信息 
  POINT_COLOR=BLUE;//设置字体为蓝色 
	LCD\_ShowString(60,170,200,16,16,"Count:");			//显示当前计数值 
	LCD\_ShowString(60,190,200,16,16,"Send Data:");		//提示发送的数据 
	LCD\_ShowString(60,250,200,16,16,"Receive Data:");	//提示接收到的数据 
 	while(1)
	{
		key=KEY\_Scan(0);
		if(key==KEY0_PRES)//KEY0按下,发送一次数据
		{
			for(i=0;i<8;i++)
			{
				canbuf[i]=cnt+i;//填充发送缓冲区
				if(i<4)LCD\_ShowxNum(60+i\*32,210,canbuf[i],3,16,0X80);	//显示数据
				else LCD\_ShowxNum(60+(i-4)\*32,230,canbuf[i],3,16,0X80);	//显示数据
 			}
			res=Can\_Send\_Msg(canbuf,8);//发送8个字节 
			if(res)LCD\_ShowString(60+80,190,200,16,16,"Failed");		//提示发送失败
			else LCD\_ShowString(60+80,190,200,16,16,"OK ");	 		//提示发送成功 
		}else if(key==WKUP_PRES)//WK\_UP按下，改变CAN的工作模式
		{	   
			mode=!mode;
  			CAN\_Mode\_Init(CAN_SJW_1tq,CAN_BS2_8tq,CAN_BS1_9tq,4,mode);//CAN普通模式初始化, 波特率500Kbps 
			POINT_COLOR=RED;//设置字体为红色 
			if(mode==0)//普通模式，需要2个开发板
			{
				LCD\_ShowString(60,130,200,16,16,"Nnormal Mode ");	    
			}else //回环模式,一个开发板就可以测试了.
			{
 				LCD\_ShowString(60,130,200,16,16,"LoopBack Mode");
			}
 			POINT_COLOR=BLUE;//设置字体为蓝色 
		}		 
		key=Can\_Receive\_Msg(canbuf);
		if(key)//接收到有数据
		{			
			LCD\_Fill(60,270,130,310,WHITE);//清除之前的显示
 			for(i=0;i<key;i++)
			{									    
				if(i<4)LCD\_ShowxNum(60+i\*32,270,canbuf[i],3,16,0X80);	//显示数据
				else LCD\_ShowxNum(60+(i-4)\*32,290,canbuf[i],3,16,0X80);	//显示数据
 			}
		}
		t++; 
		delay\_ms(10);
		if(t==20)
		{
			LED0=!LED0;//提示系统正在运行 
			t=0;
			cnt++;
			LCD\_ShowxNum(60+48,170,cnt,3,16,0X80);	//显示数据
		}		   
	}
}

```

以上。





