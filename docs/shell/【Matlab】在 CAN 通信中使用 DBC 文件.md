






下面继续学习车辆网络工具箱，在工程中，CAN通信一般都是用 DBC 文件来存储CAN报文和信号。下面看看Matlab如何处理 DBC 文件（创建、接收、处理）。




#### 文章目录


* + [打开 DBC 文件](#_DBC__3)
	+ [查看报文信息](#_22)
	+ [查看信号信息](#_30)
	+ [使用数据库定义创建报文](#_38)
	+ [查看新报文信号信息](#_49)
	+ [更改信号信息](#_59)
	+ [接收具有数据库信息的报文](#_86)
	+ [接收报文](#_95)
	+ [检查收到的报文](#_113)
	+ [提取指定报文的所有实例](#_126)
	+ [绘制物理信号值（报文解析）](#_135)
	+ [关闭 DBC 文件](#_DBC__157)




### 打开 DBC 文件


使用 `canDatabase` 打开文件 `demoVNT_CANdbFiles.dbc`，这个文件是官方示例给出的，也可以用自己的DBC文件。接下来我们主要用到 EngineMsg 这个报文：


![在这里插入图片描述](https://img-blog.csdnimg.cn/28f252e9573a47d3a8a72c839a03676d.png)



```
db = canDatabase("demoVNT\_CANdbFiles.dbc")

```

matlab读取dbc如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0b0c73e030794a7cacad28611ab59307.png)


检查 Messages 属性，可以查看该文件定义的所有报文的名称（与CANoe的一致）：



```
db.Messages

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/de3d630ac94e4675bc2525594cac5c96.png)


### 查看报文信息


使用 `messageInfo` 查看报文 EngineMsg 的信息，包括标识符、数据长度和信号列表。



```
messageInfo(db, "EngineMsg")

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/93cf2aebc534440b86cf59ef197e908c.png)


### 查看信号信息


使用 `signalInfo` 查看报文 EngineMsg 中信号 EngineRPM 的信息，包括用于将原始信号转换为物理值的类型、字节顺序、大小和系数等。



```
signalInfo(db, "EngineMsg", "EngineRPM")

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/90d8b2b14f2141b1b82adb10be6e6958.png)


### 使用数据库定义创建报文


通过指定要应用的DBC和报文名称来创建新报文。此报文中的 CAN 信号除了以原始数据字节表示外，还以工程单位来表示。



```
msgEngineInfo = canMessage(db, 'EngineMsg')

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ebc1d55eec03404cb36a0aa192710895.png)


此外，`canMessage` 还可以创建指定ID的报文：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6f24b791f49e497591fb50bcf00d64da.png)


### 查看新报文信号信息


查看新报文的信号值，并可以直接对这些信号进行写入和读取，以打包和解包报文中的数据。



```
msgEngineInfo.Signals

```

初始值如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/817d7294bad545338fa0bf1c0b794d18.png)


### 更改信号信息


直接写入信号以更改其值。可以看到Data处的改动：



```
msgEngineInfo.Signals.EngineRPM = 5500.25

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b282d7bf31e24584a811d5704aa58039.png)


读回当前信号值：



```
msgEngineInfo.Signals

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/926fae23fa6f4b5c9680780e713c9361.png)  
 当直接写入信号时，它会自动转换并使用数据库定义打包到报文数据（**十进制-十六进制**）中。下面再写入`VehicleSpeedData`：



```
msgEngineInfo.Signals.VehicleSpeed = 70.81

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6527be4d4ffe472f96daf8ea64fc80e3.png)  
 查看当前信号：



```
msgEngineInfo.Signals

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a6f1ebe2d444e91a478532cbac74847.png)


### 接收具有数据库信息的报文


将数据库连接到 CAN 通道，该通道接收报文以自动将数据库定义应用于传入报文。数据库仅解析已定义的报文。其他报文则以其原始形式接收。



```
rxCh = canChannel("MathWorks", "Virtual 1", 2);
rxCh.Database = db

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c064574016e34ba9a0cd3cb7010e6726.png)


### 接收报文


启动通道，生成一些报文流（**随机**），然后通过报文解码来接收报文。并查看接收到的报文的前几行。



```
start(rxCh);
generateMsgsDb();
rxMsg = receive(rxCh, Inf, "OutputFormat", "timetable");
rxMsg(1:15, :)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0b3f87a391144faeb95bbe85a06e9485.png)


停止接收通道并将其从工作区中清除。



```
stop(rxCh);
clear rxCh

```

### 检查收到的报文


检查收到的报文并用DBC解码：



```
rxMsg(1, :)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9dbcab4ec8bc4daab142a6390bcb52e5.png)



```
rxMsg.Signals{1}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/34adca3de2c742f594f110ec25039fd6.png)


### 提取指定报文的所有实例


提取报文 的所有实例，并查看此特定报文的前几个实例。



```
allMsgEngine = rxMsg(strcmpi('EngineMsg', rxMsg.Name), :);
allMsgEngine(1:15, :)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6dae78aeb72545aa9e380e4b1bce94ef.png)


### 绘制物理信号值（报文解析）


使用 `canSignalTimetable` 将报文中的信号数据重新打包为一个信号时间表，并查看信号时间表的前几行。



```
signalTimetable = canSignalTimetable(rxMsg, 'EngineMsg');
signalTimetable(1:15, :)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/be57747a3bbf448cb35291cc0a2e5e51.png)  
 绘制信号随时间变化的曲线：



```
plot(signalTimetable.Time, signalTimetable.VehicleSpeed)
title('Vehicle Speed from EngineMsg', 'FontWeight', 'bold')
xlabel('Timestamp')
ylabel('Vehicle Speed')

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/13aa2784c9d14f1d89d4fdb0325df800.png)


此外，可以在工作区任意修改报文或信号量的值。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ddd55704bf0b482daa21400d4ab03deb.png)


### 关闭 DBC 文件


从工作区中清除 DBC 文件的变量，关闭对该 DBC 文件的访问。



```
clear db

```

以上。





