CAN分析仪有上位机，能够满足我们大多数情况下的使用，但当我们想扩展CAN的使用，如对消息进行封装，实现特定的执行功能时，就需要根据库文件进行二次开发。下面是使用`zlg`进行二次开发的一次尝试。


请提前准备好这三个文件（库函数说明、头文件、lib库），确认是32位还是64位：


![在这里插入图片描述](https://img-blog.csdnimg.cn/374ff707a8684be1aeb5d9b97d391c87.png)


首先，新建Qt工程


![在这里插入图片描述](https://img-blog.csdnimg.cn/e5c42ae368754931b801baebfbd875a6.png)


添加库文件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/851dd3e892284a31b10085c89e4af789.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/e5e9da7b1ca0478c83030495791fe515.png)


编辑ui文件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e9b1eda360d94c9bb63c2a844c311ddb.png)


创建CANMsg类，并把ControlCAN.h加入进来：


![在这里插入图片描述](https://img-blog.csdnimg.cn/361defc314874df9bdf5fd02aba55a81.png)


代码示例如下：

canmsg.h

```cpp
#ifndef CANMSG_H
#define CANMSG_H

#include "ControlCAN.h"
#include <QThread>
#include <QMetaType>

/*can发送类型*/
enum CAN_SEND_TYPE
{
    CAN_SEND_NORMAL = 0,//正常
    CAN_SEND_SIGNAL,//单次
    CAN_SEND_SELF,//自发自收
    CAN_SEND_SELF_SIGNAL//单次自发自收
};

/*can数据类型*/
enum CAN_DATA_TYPE
{
    CAN_DATA_INFO=0,//数据帧
    CAN_DATA_REMOTE//远程帧
};

/*是否扩展帧*/
enum CAN_EXTERN_TYPE
{
    CAN_FRAM_STANDARD=0,//标准帧
    CAN_FRAM_EXTERN//扩展帧
};

Q_DECLARE_METATYPE(PVCI_CAN_OBJ*);

/*这个类主要用来接收和发送can总线数据*/
class CANMsg : public QObject
{
public:
    CANMsg();

    BOOL open(DWORD baudIdx);
    BOOL close();
    void send(VCI_CAN_OBJ info);

protected:
    void run();

private slots:

signals:
    void sendedInfoSignal(PVCI_CAN_OBJ obj);
    void getCanData(PVCI_CAN_OBJ objs,quint32 count);//把接收到的多帧can数据发给解析线程

private:

    DWORD m_Type;
    DWORD m_Idx;
    DWORD m_Chl;
    BOOL m_IsOpen;
    VCI_CAN_OBJ recvObj[10];

};

#endif // CANMSG_H


```

canmsg.cpp



```cpp
#include "CANMsg.h"

#include <QMessageBox>
#include <QThread>
#include <QDebug>
//#include <QTimer>
#include <QTime>

CANMsg::CANMsg():QThread()
{
    m_Type = VCI_USBCAN2;
    m_Idx = 0;
    m_Chl = 0;

    m_IsOpen = false;
}

BOOL CANMsg::open(DWORD baudIdx)
{
    if(m_IsOpen == false)
    {
        DWORD ret = STATUS_OK;
        ret = VCI_OpenDevice(m_Type,m_Idx,0);//打开设备,只需一次
        if(ret != STATUS_ERR)
        {
            VCI_INIT_CONFIG initConfig;
            memset(&initConfig,0,sizeof(initConfig));
            initConfig.AccMask = 0xFFFFFFFF;
            initConfig.Mode = 0;
            initConfig.Timing0 = 0x00;  //1Mbps
            initConfig.Timing1 = 0x14;

            switch(baudIdx)
            {
               case 0:
                initConfig.Timing0 = 0x00;  //1Mbps
                initConfig.Timing1 = 0x14;
                break;
            case 1:
                initConfig.Timing0 = 0x00;  //800Kbps
                initConfig.Timing1 = 0x16;
                break;
            case 2:
                initConfig.Timing0 = 0x00;  //500Kbps
                initConfig.Timing1 = 0x1c;
                break;
            case 3:
                initConfig.Timing0 = 0x03;  //250Kbps
                initConfig.Timing1 = 0x1c;
                break;
            case 4:
                initConfig.Timing0 = 0x04;  //100Kbps
                initConfig.Timing1 = 0x1c;
                break;
            default:
                break;
            }

            ret = VCI_InitCAN(m_Type,m_Idx,m_Chl,&initConfig);//初始化设备
            if(ret != STATUS_ERR)
            {
                ret = VCI_StartCAN(m_Type,m_Idx,m_Chl);//开始采集
                if(ret != STATUS_ERR)
                {
                    qDebug()<<"Open CAN device success!"<<endl;
                    m_IsOpen = true;
                }
                else
                {
                    qDebug()<<"VCI_StartCAN ERR!"<<endl;
                }
            }
            else
            {
                qDebug()<<"VCI_InitCAN ERR!"<<endl;
            }
        }
        else
        {
            qDebug()<<"VCI_OpenDevice ERR!"<<ret<<endl;
        }
    }
    return m_IsOpen;
}

BOOL CANMsg::close(void)
{
    if(m_IsOpen != false)
    {
        DWORD ret = STATUS_OK;
        ret = VCI_CloseDevice(m_Type,m_Idx);
        if(ret != STATUS_ERR)
        {
            m_IsOpen = false;
            qDebug()<<"Close CAN device success!"<<endl;
        }
        else
        {
            qDebug()<<"Close CAN device fail!"<<endl;
        }
    }
    return !m_IsOpen;
}


/***********************************************/
// z 函数名称:直接发送操函数
// h 函数作用:NULL
// u 函数参数:NULL
// x 函数返回值:NULL
// y 备注:NULL
/***********************************************/
void CANMsg::send(VCI_CAN_OBJ info)
{
    DWORD Ret = VCI_Transmit(m_Type,m_Idx,m_Chl,&info,1);
    if(STATUS_OK == Ret)
    {
        qDebug()<<"send OK"<<endl;
#if 0
                QTime current_time =QTime::currentTime();
                int second = current_time.second(); //当前的秒
                int msec = current_time.msec();     //当前的毫秒
                qDebug()<<"time:"<< second << "."<< msec <<endl;
#endif
        emit sendedInfoSignal(&info);
    }
    else
    {
        qDebug()<<"send fail,ret:"<<Ret<<endl;
    }
}

/**
 *函数名:线程
 *函数参数:NULL
 *函数作用:NULL
 *函数返回值:NULL
 *备注:NULL
 */
void CANMsg::run()
{
    VCI_ClearBuffer(m_Type,m_Idx,m_Chl);

    while(m_IsOpen)
    {
        if(VCI_GetReceiveNum(m_Type,m_Idx,m_Chl) > 0)
        {
            quint32 recvCount = VCI_Receive(m_Type,m_Idx,m_Chl,recvObj,10);
            if(0xFFFFFFFF == recvCount)//读取失败
            {
                qDebug()<<"VCI_Receive err!"<<endl;
            }
            else
            {
                //发送给数据线程处理
                qDebug()<<"received msg!"<<endl;
#if 0
                QTime current_time =QTime::currentTime();
                int second = current_time.second(); //当前的秒
                int msec = current_time.msec();     //当前的毫秒
                qDebug()<<"time:"<< second << "."<< msec <<endl;
#endif
                emit getCanData(recvObj,recvCount);
            }
        }
        else
        {
            Sleep(1);
        }
    }
}



```

mainwindow.h



```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include "canmsg.h"

namespace Ui {
class MainWindow;
}

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    Ui::MainWindow *ui;

    CANMsg *m_pObjCanMgr;
    void showCanInfo(bool isSend,PVCI_CAN_OBJ obj);

private slots:
    void on_BtnOpen_clicked();
    void on_BtnSend_clicked();

    void onRecvCanData(PVCI_CAN_OBJ objs,quint32 count);
    void onSendCanData(PVCI_CAN_OBJ obj);

    void on_pushButtonClear_clicked();
};

#endif // MAINWINDOW_H


```

mainwindow.cpp



```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

#include <QMessageBox>
#include <QDebug>

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    m_pObjCanMgr = new CANMsg();
    qRegisterMetaType<PVCI_CAN_OBJ>("PVCI_CAN_OBJ");

}

MainWindow::~MainWindow()
{
    delete ui;
    delete m_pObjCanMgr;
}

void MainWindow::on_BtnOpen_clicked()
{
    if(ui->BtnOpen->text() == "打开")
    {
        DWORD baudIdx = ui->comboBoxBaud->currentIndex();
        qDebug()<<"baudIdx:"<<baudIdx<<endl;

        if(m_pObjCanMgr->open(baudIdx))
        {
            ui->BtnOpen->setText("关闭");
            ui->comboBoxBaud->setEnabled(false);
            //启动线程
            //if(!m_pObjCanMgr->isRunning())
            //{
            // m_pObjCanMgr->start();
            //}
            connect(m_pObjCanMgr,SIGNAL(sendedInfoSignal(PVCI_CAN_OBJ)),this,SLOT(onSendCanData(PVCI_CAN_OBJ)));
            connect(m_pObjCanMgr,SIGNAL(getCanData(PVCI_CAN_OBJ,quint32)),this,SLOT(onRecvCanData(PVCI_CAN_OBJ,quint32)));
        }
    }
    else
    {
        //停止线程
        //m_pObjCanMgr->quit();
        m_pObjCanMgr->close();
        ui->BtnOpen->setText("打开");
        ui->comboBoxBaud->setEnabled(true);
        disconnect(m_pObjCanMgr,SIGNAL(sendedInfoSignal(PVCI_CAN_OBJ)),this,SLOT(onSendCanData(PVCI_CAN_OBJ)));
        disconnect(m_pObjCanMgr,SIGNAL(getCanData(PVCI_CAN_OBJ,quint32)),this,SLOT(onRecvCanData(PVCI_CAN_OBJ,quint32)));
    }
}


void MainWindow::on_BtnSend_clicked()
{
    if(ui->BtnOpen->text() == "关闭")
    {
        VCI_CAN_OBJ sendObj;
        memset(&sendObj,0,sizeof(sendObj));

        QString IdStr = ui->lineEditId->text().simplified();

        if(IdStr.isEmpty())
        {
            QMessageBox::information(this,"提示","id不能为空");
            return;
        }

        UINT canID = IdStr.toUInt(nullptr,16);
        qDebug()<< "canID:"<<canID<<endl;
        sendObj.ID = canID;

        //发送类型
        sendObj.SendType = CAN_SEND_NORMAL;
        //数据类型
        sendObj.RemoteFlag = CAN_DATA_INFO;
        //是否扩展帧
        sendObj.ExternFlag = CAN_FRAM_EXTERN;

        QString DataStr = ui->lineEditData->text().simplified();
        //数据长度
        sendObj.DataLen = DataStr.remove(QRegExp("s")).size()/2;
        //数据内容
        QByteArray DataByte = QByteArray::fromHex(DataStr.toUtf8());
        memcpy(sendObj.Data,DataByte.data(),sendObj.DataLen);
        m_pObjCanMgr->send(sendObj);
    }
    else
    {
        QMessageBox::information(this,"提示","未打开设备！");
    }
}

void MainWindow::onRecvCanData(PVCI_CAN_OBJ objs,quint32 count)
{
    //qDebug()<< "count"<<count<<endl;

    for(quint32 i = 0;i < count;i++)
    {
        showCanInfo(false,objs+i);
    }
}


void MainWindow::onSendCanData(PVCI_CAN_OBJ obj)
{
    showCanInfo(true,obj);
}

void MainWindow::showCanInfo(bool isSend,PVCI_CAN_OBJ obj)
{
    QString StrPrefix;
    QString StrText;
    QString StrData;

    if(isSend)
    {
        StrPrefix.sprintf("Tx:");
    }
    else
    {
        StrPrefix.sprintf("Rx:");
    }

    StrText.sprintf("id:0x%08x len:%d data:0x",obj[0].ID,obj[0].DataLen);
    for(quint32 j = 0;j < obj[0].DataLen;j++)
    {
        QString StrTmp;
        StrTmp.sprintf("%02x ",obj[0].Data[j]);
        StrData.append(StrTmp);
    }

    StrText.append(StrData);
    StrPrefix.append(StrText);

    ui->textEditInfo->append(StrPrefix);
    qDebug()<<StrPrefix<<endl;
}


void MainWindow::on_pushButtonClear_clicked()
{
    ui->textEditInfo->clear();
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/b3a2ace8d64b4c38a57ca65d762e9885.png#pic_center)


以上。





