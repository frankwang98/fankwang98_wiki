








#### 文章目录


* + [1.功能说明](#1_6)
	+ [2.硬件组成](#2_10)
	+ [3.软件安装](#3_15)
	+ [4.功能调试](#4_23)




**小车展示：**


![在这里插入图片描述](https://img-blog.csdnimg.cn/4047200034cc455eb79390a4582bec4e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_18,color_FFFFFF,t_70,g_se,x_16)


### 1.功能说明


Arduino智能循迹停障小车是自动驾驶车辆的微型化，用几个简单的电子元器件实现循迹、停障、绕障等功能，由于时间精力有限，后期还可以在PID控制、各种交叉路口决策、多功能集成等方面做一些工作。


### 2.硬件组成


Arduino智能小车主要由小车底盘、Arduino Uno R3单片机、Gravity IO扩展板（集成2路电机驱动口）、URM32 V5.0超声波模块、3路灰度循迹模块、7.4V可充电锂电池包等器件组成。


由于硬件与底盘孔位的不对应，部分器件调试期间采用扎带临时固定。


### 3.软件安装


软件使用arduino自己的软件，版本不限，基本上没有用其他的外接库，所有只要这个软件可以正常使用后面的代码就没有问题。


学习路线：


![在这里插入图片描述](https://img-blog.csdnimg.cn/015ad30554c64a0f9df826dd14227ec6.png)


### 4.功能调试


由于部分硬件采购DFRobot商家，相关代码可参考DF创客空间网页的Tutorial。  
 相关的模块接好线后，应该先进行单个功能的测试，最后用集成代码测试小车跑动。


相关的模块接好线后，应该先进行单个功能的测试，最后用集成代码测试小车跑动。  
 代码都放在ArduinoCar\_Code，重点参考以下几个程序：  
 1-blink-点灯  
 3-URM37-超声波测试  
 4-duoji-function-舵机测试  
 5-xunji-PID-循迹模块小车跑动代码  
 6-zonghe-5-xt-循迹+停障小车代码  
 8-huoer-霍尔模块测试


blink点灯示例：



```
 // digital pin 2 has a LED\_BLINK attached to it. Give it a name:
int LED_BLINK = 2;
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED\_BUILTIN as an output.
  pinMode(LED_BLINK, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BLINK, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(5000);                       // wait for a second
  digitalWrite(LED_BLINK, LOW);    // turn the LED off by making the voltage LOW
  delay(5000);                       // wait for a second
}

```

以上。提供思路（21.9.14）。





