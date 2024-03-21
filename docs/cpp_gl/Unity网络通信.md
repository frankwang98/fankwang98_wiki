






Unity/C#要想和其他电脑或者软件程序通讯，最好的方式是通过网络进行通讯，下面简要介绍以下其原理和实现：




#### 文章目录


* + [TCP和UDP](#TCPUDP_3)
	+ [TCP Unity简单实现](#TCP_Unity_10)
	+ - [TCP服务端](#TCP_11)
		- [TCP客户端](#TCP_234)
	+ [UDP Unity简单实现](#UDP_Unity_417)
	+ - [UDP服务端](#UDP_418)
		- [UDP客户端](#UDP_536)




### TCP和UDP


TCP和UDP是传输层协议，使用IP协议从一个网络传送数据包到另一个网络。把IP想像成一种高速公路，它允许其它协议在上面行驶并找到到其它电脑的出口。


两者的不同是：TCP能提供有保证的数据传输，而UDP不提供，即UDP的数据容易丢失。


Socket 接口是TCP/IP网络的API，Socket接口定义了许多函数或例程，用以开发TCP/IP网络上的应用程序。也就是说Socket是TCP/UDP通讯的实现方式。


### TCP Unity简单实现


#### TCP服务端


新建U3D项目，创建空场景，添加server脚本到相机：



```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Net;
using System.Net.Sockets;	//调用socket
using System.Text;
using System.Threading;	//调用线程
using UnityEngine;
using System.IO;

public class server : MonoBehaviour
{
    //定义变量（与GUI一致）
    private string info = "NULL";                          //状态信息
    private string recMes = "NULL";                        //接收到的信息
    private int recTimes = 0;                              //接收到的信息次数 

    private string inputIp = "127.0.0.1";                   //ip地址（本地）
    private string inputPort = "8080";                     //端口值
    private string inputMessage = "NULL";                  //用以发送的信息 

    private Socket socketWatch;                            //用以监听的套接字
    private Socket socketSend;                             //用以和客户端通信的套接字

    private bool isSendData = false;                       //是否点击发送数据按钮
    private bool clickConnectBtn = false;                  //是否点击监听按钮

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    //建立tcp通信链接
    private void ClickConnect()
    {
        try
        {
            int _port = Convert.ToInt32(inputPort);         //获取端口号（32位，4个字节）
            string _ip = inputIp;                           //获取ip地址

            Debug.Log(" ip 地址是 ：" + _ip);
            Debug.Log(" 端口号是 ：" + _port);

            clickConnectBtn = true;                         //点击了监听按钮，更改状态

            info = "ip地址是 ： " + _ip + "端口号是 ： " + _port;

            //点击开始监听时 在服务端创建一个负责监听IP和端口号的Socket
            socketWatch = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            IPAddress ip = IPAddress.Parse(_ip);
            IPEndPoint point = new IPEndPoint(ip, _port);   //创建对象端口

            socketWatch.Bind(point);                        //绑定端口号

            Debug.Log("监听成功!");
            info = "监听成功";

            socketWatch.Listen(10);                         //设置监听，最大同时连接10台

            //创建监听线程
            Thread thread = new Thread(Listen);
            thread.IsBackground = true;
            thread.Start(socketWatch);
        }
        catch { }
    }

    /// <summary>
    /// 等待客户端的连接 并且创建与之通信的Socket
    /// </summary>
    void Listen(object o)
    {
        try
        {
            Socket socketWatch = o as Socket;
            while (true)
            {
                socketSend = socketWatch.Accept();           //等待接收客户端连接

                Debug.Log(socketSend.RemoteEndPoint.ToString() + ":" + "连接成功!");
                info = socketSend.RemoteEndPoint.ToString() + " 连接成功!";

                Thread r_thread = new Thread(Received);      //开启一个新线程，执行接收消息方法
                r_thread.IsBackground = true;
                r_thread.Start(socketSend);

                Thread s_thread = new Thread(SendMessage);   //开启一个新线程，执行发送消息方法
                s_thread.IsBackground = true;
                s_thread.Start(socketSend);
            }
        }
        catch { }
    }

    // 服务器端不停的接收客户端发来的消息
    void Received(object o)
    {
        try
        {
            Socket socketSend = o as Socket;
            while (true)
            {
                byte[] buffer = new byte[1024 \* 6];         //客户端连接服务器成功后，服务器接收客户端发送的消息
                int len = socketSend.Receive(buffer);       //实际接收到的有效字节数
                if (len == 0)
                {
                    break;
                }
                string str = Encoding.UTF8.GetString(buffer, 0, len);

                Debug.Log("接收到的消息：" + socketSend.RemoteEndPoint + ":" + str);
                recMes = str;

                recTimes++;
                info = "接收到一次数据，接收次数为：" + recTimes;
                Debug.Log("接收数据次数：" + recTimes);
            }
        }
        catch { }
    }

    // 服务器端不停的向客户端发送消息
    void SendMessage(object o)
    {
        try
        {
            Socket socketSend = o as Socket;
            while (true)
            {
                if (isSendData)
                {
                    isSendData = false;

                    byte[] sendByte = Encoding.UTF8.GetBytes(inputMessage);

                    Debug.Log("发送的数据为 :" + inputMessage);
                    Debug.Log("发送的数据字节长度 :" + sendByte.Length);

                    socketSend.Send(sendByte);
                }
            }
        }
        catch { }
    }

    // 关闭连接，释放资源
    private void OnDisable()
    {
        Debug.Log("begin OnDisable()");

        if (clickConnectBtn)
        {
            try
            {
                socketWatch.Shutdown(SocketShutdown.Both);    //禁用Socket的发送和接收功能
                socketWatch.Close();                          //关闭Socket连接并释放所有相关资源

                socketSend.Shutdown(SocketShutdown.Both);     //禁用Socket的发送和接收功能
                socketSend.Close();                           //关闭Socket连接并释放所有相关资源 
            }
            catch (Exception e)
            {
                Debug.Log(e.Message);
            }
        }

        Debug.Log("end OnDisable()");
    }

    //交互界面（代码创建）
    void OnGUI()
    {
        GUI.color = Color.black;	//字体颜色

        GUI.Label(new Rect(65, 10, 80, 20), "状态信息");

        GUI.Label(new Rect(155, 10, 80, 70), info);

        GUI.Label(new Rect(65, 80, 80, 20), "接收到消息：");

        GUI.Label(new Rect(155, 80, 80, 20), recMes);

        GUI.Label(new Rect(65, 120, 80, 20), "发送的消息：");

        inputMessage = GUI.TextField(new Rect(155, 120, 100, 20), inputMessage, 20);

        GUI.Label(new Rect(65, 160, 80, 20), "本机ip地址：");

        inputIp = GUI.TextField(new Rect(155, 160, 100, 20), inputIp, 20);

        GUI.Label(new Rect(65, 200, 80, 20), "本机端口号：");

        inputPort = GUI.TextField(new Rect(155, 200, 100, 20), inputPort, 20);

        if (GUI.Button(new Rect(65, 240, 60, 20), "开始监听"))
        {
            ClickConnect();	//点击开始
        }

        if (GUI.Button(new Rect(65, 280, 60, 20), "发送数据"))
        {
            isSendData = true;	//发送数据
        }
    }
}


```

测试如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/cc905aba53f94b2f80663ce61c046389.png#pic_center)


#### TCP客户端


客户端与服务端实现类似，贴上脚本：



```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.IO;

using System.Threading;
using UnityEngine;
using UnityEngine.UI;

public class client : MonoBehaviour
{
    private string staInfo = "NULL";             //状态信息
    private string inputIp = "127.0.0.1";   //输入ip地址
    private string inputPort = "8080";           //输入端口号
    public string inputMes = "NULL";             //发送的消息
    private int recTimes = 0;                    //接收到信息的次数
    private string recMes = "NULL";              //接收到的消息
    private Socket socketSend;                   //客户端套接字，用来链接远端服务器
    private bool clickSend = false;              //是否点击发送按钮

    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {

    }

    //建立链接
    private void ClickConnect()
    {
        try
        {
            int _port = Convert.ToInt32(inputPort);             //获取端口号
            string _ip = inputIp;                               //获取ip地址

            //创建客户端Socket，获得远程ip和端口号
            socketSend = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            IPAddress ip = IPAddress.Parse(_ip);
            IPEndPoint point = new IPEndPoint(ip, _port);

            socketSend.Connect(point);
            Debug.Log("连接成功 , " + " ip = " + ip + " port = " + _port);
            staInfo = ip + ":" + _port + " 连接成功";

            Thread r_thread = new Thread(Received);             //开启新的线程，不停的接收服务器发来的消息
            r_thread.IsBackground = true;
            r_thread.Start();

            Thread s_thread = new Thread(SendMessage);          //开启新的线程，不停的给服务器发送消息
            s_thread.IsBackground = true;
            s_thread.Start();
        }
        catch (Exception)
        {
            Debug.Log("IP或者端口号错误......");
            staInfo = "IP或者端口号错误......";
        }
    }

    /// <summary>
    /// 接收服务端返回的消息
    /// </summary>
    void Received()
    {
        while (true)
        {
            try
            {
                byte[] buffer = new byte[1024 \* 6];
                //实际接收到的有效字节数
                int len = socketSend.Receive(buffer);
                if (len == 0)
                {
                    break;
                }

                recMes = Encoding.UTF8.GetString(buffer, 0, len);

                Debug.Log("客户端接收到的数据 ： " + recMes);

                recTimes ++;
                staInfo = "接收到一次数据，接收次数为 ：" + recTimes;
                Debug.Log("接收次数为：" + recTimes);
            }
            catch { }
        }
    }

    /// <summary>
    /// 向服务器发送消息
    /// </summary>
    /// <param name="sender"></param>
    /// <param name="e"></param>
    void SendMessage()
    {
        try
        {
            while (true)
            {
                if (clickSend)                              //如果点击了发送按钮
                {
                    clickSend = false;
                    string msg = inputMes;
                    byte[] buffer = new byte[1024 \* 6];
                    buffer = Encoding.UTF8.GetBytes(msg);
                    socketSend.Send(buffer);
                    Debug.Log("发送的数据为：" + msg);
                }
            }
        }
        catch { }
    }


    private void OnDisable()
    {
        Debug.Log("begin OnDisable()");

        if (socketSend.Connected)
        {
            try
            {
                socketSend.Shutdown(SocketShutdown.Both);    //禁用Socket的发送和接收功能
                socketSend.Close();                          //关闭Socket连接并释放所有相关资源
            }
            catch (Exception e)
            {
                print(e.Message);
            }
        }

        Debug.Log("end OnDisable()");
    }

    //用户界面
    void OnGUI()
    {
        GUI.color = Color.black;

        GUI.Label(new Rect(65, 10, 60, 20), "状态信息");

        GUI.Label(new Rect(135, 10, 80, 60), staInfo);

        GUI.Label(new Rect(65, 70, 50, 20), "服务器ip地址");

        inputIp = GUI.TextField(new Rect(125, 70, 100, 20), inputIp, 20);

        GUI.Label(new Rect(65, 110, 50, 20), "服务器端口");

        inputPort = GUI.TextField(new Rect(125, 110, 100, 20), inputPort, 20);

        GUI.Label(new Rect(65, 150, 80, 20), "接收到消息：");

        GUI.Label(new Rect(155, 150, 80, 20), recMes);

        GUI.Label(new Rect(65, 190, 80, 20), "发送的消息：");

        inputMes = GUI.TextField(new Rect(155, 190, 100, 20), inputMes, 20);

        if (GUI.Button(new Rect(65, 230, 60, 20), "开始连接"))
        {
            ClickConnect();
        }

        if (GUI.Button(new Rect(65, 270, 60, 20), "发送信息"))
        {
            clickSend = true;
        }
    }
}

```

### UDP Unity简单实现


#### UDP服务端


同样，新建U3D项目，创建脚本：



```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;
using UnityEngine;
using UnityEngine.UI;

public class server : MonoBehaviour
{
    private Socket socket; //目标socket
    private EndPoint clientEnd; //客户端
    private IPEndPoint ipEnd; //侦听端口
    private string recvStr; //接收的字符串
    private string sendStr; //发送的字符串
    private byte[] recvData = new byte[1024]; //接收的数据，必须为字节
    private byte[] sendData = new byte[1024]; //发送的数据，必须为字节
    private int recvLen; //接收的数据长度
    private Thread connectThread; //连接线程

    // Start is called before the first frame update
    void Start()
    {
        InitSocket(); //在这里初始化server
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    //初始化
    void InitSocket()
    {
        //定义侦听端口,侦听任何IP
        ipEnd = new IPEndPoint(IPAddress.Any, 8999);
        Debug.Log(ipEnd);
        //定义套接字类型,在主线程中定义
        socket = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);
        //服务端需要绑定ip
        socket.Bind(ipEnd);
        //定义客户端
        IPEndPoint sender = new IPEndPoint(IPAddress.Any, 9000);
        clientEnd = (EndPoint)sender;
        Debug.Log(clientEnd);
        print("waiting for UDP dgram");

        //开启一个线程连接，必须的，否则主线程卡死
        connectThread = new Thread(new ThreadStart(SocketReceive));
        connectThread.Start();
    }

    //服务器发送
    void SocketSend(string sendStr)
    {
        //清空发送缓存
        sendData = new byte[1024];
        //数据类型转换
        sendData = Encoding.ASCII.GetBytes(sendStr);
        //发送给指定客户端
        socket.SendTo(sendData, sendData.Length, SocketFlags.None, clientEnd);
    }

    //服务器接收
    void SocketReceive()
    {
        //进入接收循环
        while (true)
        {
            //对data清零
            recvData = new byte[1024];
            //获取客户端，获取客户端数据，用引用给客户端赋值
            recvLen = socket.ReceiveFrom(recvData, ref clientEnd);
            print("message from: " + clientEnd.ToString()); //打印客户端信息
                                                            //输出接收到的数据
            recvStr = Encoding.ASCII.GetString(recvData, 0, recvLen);
            print("我是服务器，接收到客户端的数据" + recvStr);
            //将接收到的数据经过处理再发送出去
            sendStr = "From Server: " + recvStr;
            SocketSend(sendStr);
        }
    }

    //连接关闭
    void SocketQuit()
    {
        //关闭线程
        if (connectThread != null)
        {
            connectThread.Interrupt();
            connectThread.Abort();
        }
        //最后关闭socket
        if (socket != null)
            socket.Close();
        print("disconnect");
    }

    void OnApplicationQuit()
    {
        SocketQuit();
    }

}


```

测试结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/8f5ac42fafd84130acff040165991a29.png#pic_center)


#### UDP客户端


客户端与服务端类似，脚本如下：



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;
using UnityEngine.UI;

public class UdpClient : MonoBehaviour
{
public string editString = “hello wolrd”; //编辑框文字
//以下默认都是私有的成员
private Socket socket; //目标socket
private EndPoint serverEnd; //服务端
private IPEndPoint ipEnd; //服务端端口
private string recvStr; //接收的字符串
private string sendStr; //发送的字符串
private byte[] recvData = new byte[1024]; //接收的数据，必须为字节
private byte[] sendData = new byte[1024]; //发送的数据，必须为字节
private int recvLen; //接收的数据长度
private Thread connectThread; //连接线程

//初始化
void InitSocket()
{
    //定义连接的服务器ip和端口，可以是本机ip，局域网，互联网
    ipEnd = new IPEndPoint(IPAddress.Parse("172.168.1.103"), 8999);
    //定义套接字类型,在主线程中定义
    socket = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);
    //定义服务端
    IPEndPoint sender = new IPEndPoint(IPAddress.Any, 0);
    serverEnd = (EndPoint)sender;
    print("waiting for sending UDP dgram");

    //建立初始连接，这句非常重要，第一次连接初始化了serverEnd后面才能收到消息
    SocketSend("hello");

    //开启一个线程连接，必须的，否则主线程卡死
    connectThread = new Thread(new ThreadStart(SocketReceive));
    connectThread.Start();
}

void SocketSend(string sendStr)
{
    //清空发送缓存
    sendData = new byte[1024];
    //数据类型转换
    sendData = Encoding.ASCII.GetBytes(sendStr);
    //发送给指定服务端
    socket.SendTo(sendData, sendData.Length, SocketFlags.None, ipEnd);
}

//服务器接收
void SocketReceive()
{
    //进入接收循环
    while (true)
    {
        //对data清零
        recvData = new byte[1024];
        //获取客户端，获取服务端端数据，用引用给服务端赋值，实际上服务端已经定义好并不需要赋值
        recvLen = socket.ReceiveFrom(recvData, ref serverEnd);
        print("message from: " + serverEnd.ToString()); //打印服务端信息
                                                        //输出接收到的数据
        recvStr = Encoding.ASCII.GetString(recvData, 0, recvLen);
        print("我是客户端，接收到服务器的数据" + recvStr);
    }
}

//连接关闭
void SocketQuit()
{
    //关闭线程
    if (connectThread != null)
    {
        connectThread.Interrupt();
        connectThread.Abort();
    }
    //最后关闭socket
    if (socket != null)
        socket.Close();
}

// Use this for initialization
void Start()
{
    InitSocket(); //在这里初始化
}

void OnGUI()
{
    editString = GUI.TextField(new Rect(10, 10, 100, 20), editString);
    if (GUI.Button(new Rect(10, 30, 60, 20), "send"))
        SocketSend(editString);
}

// Update is called once per frame
void Update()
{

}

void OnApplicationQuit()
{
    SocketQuit();
}


```

以上。





