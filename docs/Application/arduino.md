# arduino学习笔记

## 一、arduino是什么
一个开源的控制器平台，社区资源较为丰富，可用于做各种电子模型。

## 二、arduino安装
官方下载：[arduino](https://www.arduino.cc/en/software/)

Windows安装较为简单，记得提前安装好 ch340 驱动就行。

Ubuntu安装步骤如下：

	1.官网下载对应版本
	2.tar -xvf 安装包名
	3.sudo mv 解压过后的包名 /opt
	4.sudo chmod +x install.sh
	5.sudo ./install.sh
	
安装完后，插入开发版烧录时会提示串口没有权限，这里提供一种永久修改串口权限的方法：
`sudo gedit /etc/udev/rules.d/70-ttyusb.rules`		
打开文件后添加一行即可：`KERNEL==“ttyUSB[0-9]*”, MODE=“777”`
