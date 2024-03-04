






大家都知道，车辆底盘系统是通过CAN进行通信的，而常见的有USB-CAN和SocketCAN两种，前者是通过USB口接入PC的，代表的有周立功、创芯等（较便宜），后者是通过网口接入PC的，代表的有Kvaser。


USB-CAN一般Windows端的资料会多些，有做好的上位机和Qt、C#、MATLAB等二次开发接口，但Linux的支持只有一个测试样例，需要自己去写；SocketCAN对全平台支持都很好，除了Windows的丰富资料外，在Linux端只需安装net-tools和SocketCAN驱动，然后就可以使用utils的命令去控制。




#### 文章目录


* + [CAN认识](#CAN_5)
	+ [ICSim总线设备模拟器](#ICSim_10)
	+ - [车门测试](#_43)
		- [转向灯测试](#_45)
		- [实车CAN测试](#CAN_48)
	+ [CAN-Utils](#CANUtils_51)
	+ [Wireshark](#Wireshark_84)
	+ [SavvyCAN](#SavvyCAN_95)




### CAN认识


CAN 是控制器局域网络（`Controller Area Network`）的简称，是实现汽车所有或部分部件之间通信的中枢神经系统，由以研发和生产汽车电子产品著称的德国 BOSCH 公司开发并最终成为国际标准（ISO 11898），是国际上应用最广泛的现场总线之一。在使用 CAN 作为车内通信系统之前，汽车制造商使用的是点对点布线系统，当汽车内部电子单元越来越多时，这种布线系统会显得特别庞大且维护成本太高，后来通过使用 CAN 来解决这个问题。


CAN 总线可以被认为是一个嘈杂、拥挤、慢速版的以太网局域网，只是流量是 UDP 而不是 TCP。值得注意的是，并不是所有的汽车控制系统都使用 CAN，而且 CAN 不仅仅是汽车系统中使用的通信协议，还可能有其它协议，如蓝牙、LIN、MOST、FlexRay等。在实际场景中，CAN 并不是唯一的攻击面，可能还存在很多其它的攻击面。


### ICSim总线设备模拟器


参考了[这篇](https://mp.weixin.qq.com/s/jvzLn2ZTmId4cfqBNJxYyg)，补充完善一些内容。


对于没有SocketCAN设备的童鞋来说，ICSim为我们研究开发提供了一种可能，同时可以练习can-utils操作，另外，使用模拟器可以隔离硬件环境，防止硬件受损。


简单来说，ICSim（Instrument Cluster Simulator for SocketCAN）是一个开源的车辆仪表模拟器，该模拟器包含controls和ICSim两个模块，其中controls负责生成模拟的车辆数据，以CAN报文的方式发送给虚拟的CAN接口，ICSim从虚拟CAN接口（vcan0）读取CAN报文，并在仪表上更新对应零件的状态，如车速、转向、车门状态等等。


ICSim安装：



```

sudo apt-get install libsdl2-dev libsdl2-image-dev can-utils
git clone https://github.com/zombieCraig/ICSim.git

```

ICSim配置和编译：



```
./setup_vcan.sh
make

```

配置完后会生成两个可执行文件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/cc4979ac3b664ba6b2470cb4a1f07487.png)


ICSim启动：



```
./icsim vcan0
./controls vcan0

```

会生成汽车模拟仪表盘和控制面板：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2af12a8b7a8545a6b28d4a7dbf43dc39.png)


#### 车门测试


车门状态报文一直在定时发送，但每当按下一次开门按钮，报文中的数据(DATA)会发生一次变化。通过变化找到CAN报文（CAN逆向）。


#### 转向灯测试


转向灯报文也是一直在定时发送，当按下按钮，报文发生一次变化。通过变化找到CAN报文（CAN逆向）。


#### 实车CAN测试


通过OBD接口测试。


### CAN-Utils


Linux 内核中内置了 SocketCAN、can-utils、vcan等工具链，作用是发送和接收 CAN 数据，对数据进行编码或解码。


can-utils 是一套 Linux 特有的实用工具，它可以让 Linux 与车辆上的 CAN 网络进行通信，为了发送、接收和分析 CAN 数据包，需要安装 CAN utils：



```
sudo apt-get install can-utils

```

canutils 主要包括 5 个常用的工具：


1. cansniffer 用于嗅探数据包（只显示正在变化的报文）（cansniffer -c can0）
2. cansend 发送一条报文数据（cansend can0 0C9#8021C0071B101000）
3. candump 转储所有接收的数据包（candump can0）
4. canplayer 重播 CAN 数据包
5. cangen 随机生成 CAN 数据包


回环测试（自发自收）：



```
candump can0&
cansend can0 123#0011223344556677

```

在实车中，将CAN设备插入汽车的OBD-II端口和计算机的USB端口。在Linux提示符中运行以下命令启动CAN接口：



```
sudo ip link set can0 up type can bitrate 500000

```

这将以500 kbps的比特率打开can0接口(如果你只有一个设备连接，总是can0)，这是标准的。


Linux通过SocketCAN在内核中内置CAN支持，使得编写自己的附加程序变得很容易。你可以与can总线交互，就像你与任何其他网络交互一样，即通过套接字socket。


### Wireshark


Wireshark是一个网络测试工具，支持Linux。安装启动如下：



```
sudo apt-get install wireshark
sudo wireshark

```

启动后，可以读取对应网卡：


![在这里插入图片描述](https://img-blog.csdnimg.cn/43adfff1ca954c02997880f6bbfaf689.png)


### SavvyCAN


前面介绍了can-utils和wireshark，下面介绍一种更加专业、好用的CAN分析工具。


[SavvyCAN](https://www.savvycan.com/) 是一个 CAN 总线逆向和捕获工具提供了更多额外的功能，它除了能够轻轻的浏览、过滤数据包和仲裁 ID，还可以在 CAN 帧上执行脚本、Fuzzing，以及内置了几个逆向工具。


安装SaccyCAN（二进制文件）：



```
wget https://github.com/collin80/SavvyCAN/releases/download/V199.1/SavvyCAN-305dafd-x86_64.AppImage
chmod 744 SavvyCAN-305dafd-x86_64.AppImage
./SavvyCAN-305dafd-x86_64.AppImage

```

启动后如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/af9fb60e48534f1b9b0e238ee595a1c8.png)


以上是打开了一个CAN Log文件。要想获取ICSim的数据，会发现二进制版的SavvyCAN没有开启接口：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7393b80c2bd9422083a13e71bf83e104.png)


因此需要编译源代码获得完整功能（首先安装Qt5）：



```
git clone https://github.com/collin80/SavvyCAN
cd SavvyCAN
/opt/Qt5.12.4/5.12.4/gcc_64/bin/qmake CONFIG+=debug
make
./SavvyCAN

```

参考了这个[博客](https://mlog.club/article/5773657)。


对汽车安全有兴趣的可以多了解一下，最好有机会去采集一下实车数据。


以上。





