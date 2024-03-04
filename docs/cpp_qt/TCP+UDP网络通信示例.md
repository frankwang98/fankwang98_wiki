前面在Unity专题中已经介绍了网络通信TCP和UDP的原理和实现，在Qt中，也常常会用到网络通信（pro工程文件中`+=network`），因此就要学习掌握socket API的使用以及TCP和UDP各自的用处。

TCP和UDP的原理这里不再介绍，感兴趣的可以看[之前文章](http://t.csdn.cn/5uf1U)。

### TCP实现

TCP的实现参考[这个代码](https://gitee.com/qq1753713582/qttcp.git)。


服务端和客户端运行效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6f7de68ae50c41fd918b13938a11b874.png)


下面简要分析一下代码：


#### TCP服务端


1. 界面重要控件如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/ee41f0b4db1c4bdc97082e699657381e.png)


首先需要定义服务端接收和发送，以及服务器的ip地址和端口。


2. 头文件widget.h如下：



```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QtNetwork/QTcpServer>
#include <QtNetwork/QTcpSocket>
#include <QtNetwork/QHostInfo> //得到本机网络信息的头文件
#include <QtNetwork/QNetworkInterface>
#include <QList>
#include <QtNetwork/QHostAddress>
#include <QMessageBox>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

private slots:
    void on_btnBind_clicked();
    void newConnectionSlot();
    void readyReadSlot();
    void on_btnSend_clicked();

private:
    Ui::Widget *ui;
    QTcpServer *tcpServer; //服务器类
    QTcpSocket *tcpClient; //客户端类
    QString myAddress; //服务器的ip地址和端口
    int myPort;
};

#endif // WIDGET_H


```

在功能实现中，获取IP地址有两种方法，一是通过代码自动获取IP地址，二是手动设置IP，默认是手动设置，设置完成后，点击绑定端口，即可开始监听客户端的信息。


3. 功能实现widget.cpp如下：



```cpp
#include "widget.h"
#include "ui_widget.h"
#define MAXConnect 10 //最大的连接数目

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    tcpServer=new QTcpServer(this);  //初始化服务器指针
    myPort=8888;
    ui->btnSend->setEnabled(false);
}

Widget::~Widget()
{
    delete ui;
}

//服务器IP和端口的绑定
void Widget::on_btnBind_clicked()
{

    /**********步骤一：得到需要的IP地址*****************/

// //方法一：通过代码得到本机网络信息
// QString localHostName=QHostInfo::localDomainName(); //得到本机的主机名
// QHostInfo hostInfo=QHostInfo::fromName(localHostName); //根据主机名获得主机的相关信息

// //获得主机的IP地址列表
// QList<QHostAddress> listAddress=hostInfo.addresses();
// if(!listAddress.isEmpty())
// {
// for(int i=0;i<listAddress.count();i++)
// {
// if(listAddress.contains("192"))
// {
// myAddress=listAddress.at(i); //得到满足条件的IP地址
// break;
// }
// }
// }


    //方法二：手动设置IP地址
    myAddress=ui->serverIP->text();

    /**********步骤二：绑定端口和IP地址，开始监听端口***********************************/
   if(tcpServer->listen(QHostAddress(myAddress),myPort))  //绑定IP和端口
   {
       ui->recvMsg->setPlainText("端口绑定成功!");
       ui->btnBind->setEnabled(false);
   }
   else
   {
      ui->recvMsg->setPlainText("端口绑定失败!");
      ui->btnBind->setEnabled(true);
   }

   tcpServer->setMaxPendingConnections(MAXConnect);  //设置最大的绑定数目为10

   connect(tcpServer,SIGNAL(newConnection()),this,SLOT(newConnectionSlot()));
}

//客户端连接上的信号
void Widget::newConnectionSlot()
{
    tcpClient=tcpServer->nextPendingConnection(); //连接对象
    connect(tcpClient,SIGNAL(readyRead()),this,SLOT(readyReadSlot()));  //关联读信息的槽函数

    ui->clientIP->setText(tcpClient->peerAddress().toString());  //得到远程连接上的计算机IP地址
    ui->clientPort->setText(QString::number(tcpClient->peerPort())); //得到远程的端口地址
    ui->btnSend->setEnabled(true);  //可以发送数据
}

//客户端接收消息信号
void Widget::readyReadSlot()
{
    QTcpSocket *client=(QTcpSocket *)this->sender();
    while(!client->atEnd())
    {
        ui->recvMsg->appendPlainText(QString(client->readAll()));//读取接收到的数据
    }
}

//客户端发送消息
void Widget::on_btnSend_clicked()
{
    if(tcpClient->peerAddress().toString()==ui->clientIP->text())
        if(tcpClient->peerPort()==ui->clientPort->text().toUInt())
        {
            tcpClient->write(ui->sendMsg->toPlainText().toLatin1()); //发送数据
        }
        else
        {
            QMessageBox::information(this,tr("提示"),tr("error"));
        }
}


```

#### TCP客户端


1. 客户端重要控件如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f86c0f5aa1424113ae6657f05b694594.png)


客户端头文件中只需去定义客户端指针和绑定端口操作即可。


2. 头文件widget.h如下：



```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QtNetwork/QTcpSocket>
#include <QtNetwork/QHostAddress>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

private slots:
    void on_btnBind_clicked();
    void connectedSlot();
    void readyReadSlot();

    void on_btnSend_clicked();

private:
    Ui::Widget *ui;
    QTcpSocket *tcpClient;  //tcp客户端指针
};

#endif // WIDGET_H


```

3. 功能实现widget.cpp如下：



```cpp
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    tcpClient=new QTcpSocket(this);  //tcp客户端的初始化

    connect(tcpClient,SIGNAL(readyRead()),this,SLOT(readyReadSlot())); //关联读取数据函数
}

Widget::~Widget()
{
    delete ui;
}

//连接服务器的槽函数
void Widget::on_btnBind_clicked()
{
    QString serverIP=ui->serverIP->text();  //获得服务器的IP地址
    QString serverPort=ui->serverPort->text(); //获得服务器的端口号
    tcpClient->connectToHost(QHostAddress(serverIP),serverPort.toUInt());  //和服务器建立连接
    connect(tcpClient,SIGNAL(connected()),this,SLOT(connectedSlot()));  //连接成功的信号处理
}

//连接成功的处理
void Widget::connectedSlot()
{
    ui->recvMsg->setPlainText("success");

    //connect(tcpClient,SIGNAL(readyRead()),this,SLOT(readyReadSlot())); //关联读取数据函数
}

//读取数据的槽函数
void Widget::readyReadSlot()
{
    while(!tcpClient->atEnd())
    {
        ui->recvMsg->appendPlainText(tcpClient->readAll()); //读取数据内容
    }
}

//发送数据
void Widget::on_btnSend_clicked()
{
    tcpClient->write(ui->sendMsg->toPlainText().toLatin1()); //发送数据
}


```

### UDP实现


UDP的实现参考[这个代码](https://gitee.com/qq1753713582/qt_udp.git)。


UDP服务端和客户端运行效果如下（服务端接收到客户端消息后默认回复‘1’）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/eb91a5b32989455da2ce91dde1fa321a.png)


#### UDP服务端


界面控件很简单，一个接受框即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/865d1f6060924b6985b07699db83f8aa.png)


头文件中定义udpServer指针和接收槽函数即可。


1. widget.h



```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QtNetwork/QUdpSocket>
#include <QByteArray>
#include <QMessageBox>

namespace Ui {
class Widget;
}

class UdpServer : public QWidget
{
    Q_OBJECT

public:
    explicit UdpServer(QWidget *parent = 0);
    ~UdpServer();

private slots:
    void recvData();

private:
    Ui::Widget *ui;
    QUdpSocket *udpServer;
};

#endif // WIDGET_H


```

功能函数中定义通信接口、信号槽和接收函数即可。


2. widget.cpp



```cpp
#include "widget.h"
#include "ui_widget.h"

UdpServer::UdpServer(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    udpServer=new QUdpSocket(this); //创建套接字
    udpServer->bind(QHostAddress::Any,8040); //绑定通信端口8040
    connect(udpServer,SIGNAL(readyRead()),this,SLOT(recvData()));  //连接信号接收函数
}

UdpServer::~UdpServer()
{
    delete ui;
}

void UdpServer::recvData()
{
    //接收数据
    while (udpServer->hasPendingDatagrams())
    {
    QByteArray datagram; //定义接收的数组
    datagram.resize(udpServer->pendingDatagramSize()); //设置接收数组的大小
    udpServer->readDatagram(datagram.data(), datagram.size()); //读取数据
    QString s = datagram.data(); //分离出需要的数据
    ui->plainTextEdit->appendPlainText(s); //数据显示
    }

    QString s="1";
// udpServer->write(s.toLatin1(), s.length());
    udpServer->writeDatagram(s.toLatin1(), s.length(), QHostAddress::Broadcast, 8050);
}


```

#### UDP客户端


客户端界面控件如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d5299a9c887e49de9dc212d11926c22d.png)


头文件中定义udpClient指针和发送、接收函数。


1. widget.h



```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QtNetwork/QUdpSocket> //udpsocket头文件
#include <QString>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();

private slots:
    void on_pushButton_clicked();
    void recvData();

private:
    Ui::Widget *ui;
        QUdpSocket *udpClient;
};

#endif // WIDGET_H


```

功能函数中定义通信接口（`服务端8040，客户端8050`），发送和接收槽函数。


3. widget.cpp



```cpp
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    udpClient=new QUdpSocket(this); //实例化QUdpSocket
    udpClient->bind(QHostAddress::Any,8050); //绑定通信端口
    connect(udpClient,SIGNAL(readyRead()),this,SLOT(recvData()));  //连接信号接收函数
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_pushButton_clicked()
{
   //发送数据按钮定义
   QString s= ui->lineEdit->text(); //字符串s为发送数据文本
   udpClient->writeDatagram(s.toLatin1(), s.length(), QHostAddress::Broadcast, 8040);   //写入数据报文
   //ui->lineEdit->clear();
}

void Widget::recvData()
{
    //接收数据定义
    while (udpClient->hasPendingDatagrams())
    {
    QByteArray datagram; //定义接收的数组
    datagram.resize(udpClient->pendingDatagramSize()); //设置接收数组的大小

    udpClient->readDatagram(datagram.data(), datagram.size()); //读取数据
    QString s = datagram.data(); //分离出需要的数据，定义为字符串s
    ui->plainTextEdit->appendPlainText(s); //数据显示到plainTextEdit
    }
}


```

以上。





