






此外，更多时候我们是在Simulink中使用CAN信号的传输和接收，例如下面这个用Simulink仿真汽车CAN信号的例子。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e30b93795e35404894fb7da020d6e2d2.png)


为了做出上述功能，我们先来学一下Simulink中CAN模块的基本使用。




#### 文章目录


* + [CAN模块介绍](#CAN_7)
	+ [周期性CAN报文传输](#CAN_37)
	+ - [传输和接收CAN报文](#CAN_42)
		- [可视化不同时间戳的报文](#_62)
	+ [基于事件的CAN报文传输](#CAN_71)
	+ [记录和重播CAN报文](#CAN_73)




### CAN模块介绍


1.CAN Configuration


![在这里插入图片描述](https://img-blog.csdnimg.cn/6b44c2d552d1484aaed83b3b4807e74f.png)


2.CAN Log


![在这里插入图片描述](https://img-blog.csdnimg.cn/83acdb43db3943f6abfbe14c9d2c0ba9.png)


3.CAN Replay


![在这里插入图片描述](https://img-blog.csdnimg.cn/8b19e08d56fd4cdca11354a4c05e5ceb.png)


4.CAN Pack


![在这里插入图片描述](https://img-blog.csdnimg.cn/eac9ef42d79a41f0ac480e4fdfcea4f0.png)


5.CAN Unpack


![在这里插入图片描述](https://img-blog.csdnimg.cn/e6f523aff23d465ebb80160d293c3f7f.png)


6.CAN Receive


![在这里插入图片描述](https://img-blog.csdnimg.cn/a3de7013d7b641e18eb3f9b1fe9394b8.png)


7.CAN Transmit


![在这里插入图片描述](https://img-blog.csdnimg.cn/e6c42de14c0a4954a13836e34a08d2eb.png)


了解了CAN相关Simulink模块的基本信息，再来搭建下面的模型。


### 周期性CAN报文传输


使用 MathWorks 虚拟 CAN 通道在 Simulink 中设置 CAN 报文的周期性传输和接收。虚拟通道以环回配置形式连接。


Vehicle Network Toolbox™ 提供了 Simulink 模块，用于通过 Simulink 模型在控制器局域网 (CAN) 上传输和接收实时报文。此示例使用 `CAN Configuration、CAN Pack、CAN Transmit、CAN Receive 和 CAN Unpack 模块`来执行 CAN 总线上的数据传输。


#### 传输和接收CAN报文


创建一个模型，以2个不同的周期传输报文（也就是选择两个不同的波特率），并仅接收指定的报文和解包具有指定 ID 的报文。


* CAN Configuration设置波特率为500000（500K）。
* 使用一个 CAN Transmit 模块传输 ID 为 250 的 CAN 报文，每 1 秒传输一次报文（Transmit）。
* 使用另一个 CAN Transmit 模块传输 ID 为 500 的 CAN 报文，每 0.5 秒传输一次报文（Transmit）。
* 向两个 CAN Pack 模块各输入一个信号以使计数器自动递增，计数上限为 50。
* 两个 CAN Transmit 模块都连接到 MathWorks 虚拟通道 1。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e62c26500f5a4c17a0b273cd667332a8.png)


使用一个 CAN Receive 模块从 MathWorks 虚拟通道 2 接收 CAN 报文。将该模块设置为：


* 仅接收 ID 为 250 和 500 的报文。
* 如果 Receive 模块在任何特定时间步接收到新报文，该模块会生成一个函数调用触发器（function）。


CAN Unpack 模块位于函数调用子系统中。子系统仅当 CAN Receive 模块在特定时间步接收到新报文时才执行。


![在这里插入图片描述](https://img-blog.csdnimg.cn/a3ba358e8f2f469384817e2ca4520cd2.png)


#### 可视化不同时间戳的报文


配置好通道1和通道2后的模型如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0e0e56efa3e34d89a9595362ab9cda14.png)


绘制结果以查看每个解包报文的计数器值和时间戳。图上的 X 轴对应于仿真时间步。时间戳图显示报文是在指定时间发送的。还可以看出，由于指定了不同周期性速率，传输的 ID 为 250 的报文数量是 ID 为 500 的报文数量的一半。


![在这里插入图片描述](https://img-blog.csdnimg.cn/b31b756c21d54bfd90effa8857c01eee.png)


### 基于事件的CAN报文传输


。。。


### 记录和重播CAN报文


。。。


以上。





