网络编程是C++ API操作中很重要的一部分，包含`TCP`和`UDP`。

网络传输模型可以抽象为7个层：`物理层、数据链路层、网络层、传输层、会话层、表示层、应用层`。

但在使用TCP/IP协议时，可以简化为这4层：`网络接口、网络层、传输层、应用层`。

### 名词介绍

TCP：可靠传输，三次握手建立连接，传出去一定接受的到（如聊天软件）；

UDP：不可靠传输，不需要建立连接，只管发送，实时性好（如视频会议）；

套接字：表示通信的端点。就像用电话通信，套接字相当于电话，IP地址相当于总机号码，而端口号则相当于分机号码。

### TCP

服务端创建流程：

1. 调用socket函数创建监听socket
2. 调用bind函数将socket绑定到某个IP和端口号组成的二元组上
3. 调用listen函数开启监听
4. 当有客户端连接请求时，调用accept函数接受连接，产生一个新的socket（与客户端通信的socket）
5. 基于新产生的socket调用send或recv函数开始与客户端进行数据交流
6. 通信结束后，调用close函数关闭socket

客户端创建流程：

1. 调用socket函数创建客户端socket
2. 调用connect函数尝试连接服务器
3. 连接成功后调用send或recv函数与服务器进行数据交流
4. 通信结束后，调用close函数关闭监听socket

服务端代码：

```cpp
#include <iostream>
#include <sys/types.h> //基本系统数据类型
#include <arpa/inet.h> //网络信息转换
#include <unistd.h> //POSIX系统API访问
#include <string.h>

using namespace std;
int main() {

    // 创建一个监听socket
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);	
	//常见的AF_INET──指定为IPv4协议，AF_INET6──指定为IPv6，AF_LOCAL──指定为UNIX 协议域
	//套接口可能的类型有：SOCK_STREAM字节流、SOCK_DGRAM数据报、SOCK_SEQPACKET有序分组、SOCK_RAW原始套接口
	//传输协议TCP/UDP，这里默认0
    if (listenfd == -1) {
        cout << " create listen socket error " << endl;
        return -1;
    }
    // 初始化服务器地址
    struct sockaddr_in bindaddr;
    bindaddr.sin_family = AF_INET;
    bindaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    bindaddr.sin_port = htons(8081);
	//如果只想在本机上进行访问，bind函数地址可以使用本地回环地址
	//如果只想被局域网的内部机器访问，那么bind函数地址可以使用局域网地址
	//如果希望被公网访问，那么bind函数地址可以使用INADDR_ANY or 0.0.0.0
    if (bind(listenfd, (struct sockaddr *)& bindaddr, sizeof(bindaddr)) == -1) {
        cout << "bind listen socket error" << endl;
        return -1;
    }
    // 启动监听
    if (listen(listenfd, SOMAXCONN) == -1) {
        cout << "listen error" << endl;
        return -1;
    }
	cout << "开始监听" << endl;

    while (true) {
        // 创建一个临时的客户端socket
        struct sockaddr_in clientaddr;
        socklen_t clientaddrlen = sizeof(clientaddr);
        // 接受客户端连接
        int clientfd = accept(listenfd, (struct sockaddr *)& clientaddr, &clientaddrlen);
        if (clientfd != -1) {
            char recvBuf[32] = {0};
            // 从客户端接受数据
            int ret = recv(clientfd, recvBuf, 32, 0);
            if (ret > 0) {
                cout << "recv data from cilent , data:" << recvBuf << endl;
                // 将接收到的数据原封不动地发给客户端
                ret = send(clientfd, recvBuf, strlen(recvBuf), 0);
                if (ret != strlen(recvBuf)) {
                    cout << "send data error" << endl;
                } else {
                    cout << "send data to client successfully, data " << recvBuf <<endl;
                }
            } else {
                cout << "recv data error" <<endl;
            }
            close(clientfd);
        }
    }

    // 关闭监听socket
    close(listenfd);
    return 0;
}

```

客户端代码：

```cpp
#include <iostream>
#include <sys/types.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>

#define SERVER_ADDRESS "127.0.0.1"
#define SERVER_PORT 8081
#define SEND_DATA "helloworld"

using namespace std;

int main() {
    // 创建一个socket
    int clientfd = socket(AF_INET, SOCK_STREAM, 0);
    if (clientfd == -1) {
        cout << " create client socket error " << endl;
        return -1;
    }
    // 连接服务器
    struct sockaddr_in serveraddr;
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr(SERVER_ADDRESS);
    serveraddr.sin_port = htons(SERVER_PORT);
    if (connect(clientfd, (struct sockaddr *)& serveraddr, sizeof(serveraddr)) == -1) {
        cout << "connect socket error" << endl;
        return -1;
    }
    // 向服务器发送数据
    int ret = send(clientfd, SEND_DATA, strlen(SEND_DATA), 0);
    if (ret != strlen(SEND_DATA)) {
        cout << "send data error" << endl;
        return -1;
    } else {
        cout << "send data to client successfully, data " << SEND_DATA <<endl;
    }
    // 从服务器拉取数据
    char recvBuf[32] = {0};
    ret = recv(clientfd, recvBuf, 32, 0);
    if (ret > 0) {
        cout << "recv data to client successfully, data " << recvBuf <<endl;
    } else {
        cout << "recv data to client error" << endl;
    }
    // 关闭socket
    close(clientfd);
    return 0;
}

```

### UDP

接收端创建流程：

1. 创建套接字
2. 将套接字绑定到一个本地地址和端口上（bind）
3. 等待接受数据（recv）
4. 关闭套接字。

发送端创建流程：

1. 创建套接字
2. 向服务器发送数据（send）
3. 关闭套接字

UDP通信时，不强调server和client，重在实现两者互通；接收端需要bind，而发送端不需要。bind的一方只有接收到消息后才能开始发送。

设置两个端口实现互通：

接收端：

```cpp
#include <sys/select.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <cstdlib> //标准库头文件
#include <cstdio> //c标准库
#include <cstring> //c字符操作
#include <iostream>

int main(){

    //同一台电脑测试，需要两个端口
    int port_in  = 12321;
    int port_out = 12322;
    int sockfd;

    // 创建socket
    sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(-1==sockfd){
        return false;
        puts("Failed to create socket");
    }

    // 设置地址与端口
    struct sockaddr_in addr;
    socklen_t          addr_len=sizeof(addr);

    memset(&addr, 0, sizeof(addr));
    addr.sin_family = AF_INET;       // Use IPV4
    addr.sin_port   = htons(port_out);    //
    addr.sin_addr.s_addr = htonl(INADDR_ANY);

    // Time out
    struct timeval tv;
    tv.tv_sec  = 0;
    tv.tv_usec = 200000;  // 200 ms
    setsockopt(sockfd, SOL_SOCKET, SO_RCVTIMEO, (const char*)&tv, sizeof(struct timeval));

    // Bind 端口，用来接受之前设定的地址与端口发来的信息,作为接受一方必须bind端口，并且端口号与发送方一致
    if (bind(sockfd, (struct sockaddr*)&addr, addr_len) == -1){
        printf("Failed to bind socket on port %d\n", port_out);
        close(sockfd);
        return false;
    }

    char buffer[128];
    memset(buffer, 0, 128);

    int counter = 0;
    while(1){
        struct sockaddr_in src;
        socklen_t src_len = sizeof(src);
        memset(&src, 0, sizeof(src));

        int sz = recvfrom(sockfd, buffer, 128, 0, (sockaddr*)&src, &src_len);
        if (sz > 0){
            buffer[sz] = 0;
            printf("Get Message %d: %s\n", counter++, buffer);
        }
        else{
            puts("timeout");
        }
    }

    close(sockfd);
    return 0;
}

```

发送端：

```cpp
#include<sys/select.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<arpa/inet.h>
#include<netinet/in.h>
#include <cstdlib>
#include <cstdio>
#include <cstring>
#include <iostream>

int main(){
    int port_in  = 12321;
    int port_out = 12322;
    int sockfd;

    // 创建socket
    sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(-1==sockfd){
        return false;
        puts("Failed to create socket");
    }

    // 设置地址与端口
    struct sockaddr_in addr;
    socklen_t  addr_len=sizeof(addr);

    memset(&addr, 0, sizeof(addr));
    addr.sin_family = AF_INET;       // Use IPV4
    addr.sin_port   = htons(port_in);    //
    addr.sin_addr.s_addr = htonl(INADDR_ANY);

    // Time out
    struct timeval tv;
    tv.tv_sec  = 0;
    tv.tv_usec = 200000;  // 200 ms
    setsockopt(sockfd, SOL_SOCKET, SO_RCVTIMEO, (const char*)&tv, sizeof(struct timeval));

    // 绑定获取数据的端口，作为发送方，不绑定也行
    if (bind(sockfd, (struct sockaddr*)&addr, addr_len) == -1){
        printf("Failed to bind socket on port %d\n", port_in);
        close(sockfd);
        return false;
    }

    int counter = 0;
    while(1){


        addr.sin_family = AF_INET;
        addr.sin_port   = htons(port_out);
        addr.sin_addr.s_addr = htonl(INADDR_ANY);

        sendto(sockfd, "hello world", 11, 0, (sockaddr*)&addr, addr_len);
        printf("Sended %d\n", ++counter);
        sleep(1);
    }

    close(sockfd);
    return 0;
}

```

设置一个端口实现互通：

接收端：

```cpp
//只有在server接收到消息后才能实现互发数据
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>
#define SERVER_PORT 8888//唯一端口号
int main(){
    int ser_sockfd;
    int len;
    fd_set rfds;
    socklen_t addrlen;
    char seraddr[100];
    struct sockaddr_in ser_addr;
    int retval, maxfd;
    //建立socket
    ser_sockfd=socket(AF_INET,SOCK_DGRAM,0);
    if(ser_sockfd<0){
        printf("I cannot socket successn");
        return 1;
     }
    //设置地址与端口
    addrlen=sizeof(struct sockaddr_in);
    bzero(&ser_addr,addrlen);
    ser_addr.sin_family=AF_INET;
    ser_addr.sin_addr.s_addr=htonl(INADDR_ANY);
    ser_addr.sin_port= htons (SERVER_PORT);
    //server绑定，才能接受client的数据
    if(bind(ser_sockfd,(struct sockaddr *)&ser_addr,addrlen)<0){
        printf("connect");
        return 1;
    }
    while(1){
        bzero(seraddr,sizeof(seraddr));
        len=recvfrom(ser_sockfd,seraddr,sizeof(seraddr),0,(struct sockaddr*)&ser_addr,&addrlen);
        /*显示client端的网络地址*/
        printf("receive from %sn",inet_ntoa(ser_addr.sin_addr));
        /*显示客户端发来的字串*/
        printf("recevce:%s",seraddr);
        /*输入字串返回给client端*/
        while(1)
        {
            /*把可读文件描述符的集合清空*/
            FD_ZERO(&rfds);
            /*把标准输入的文件描述符加入到集合中*/
            FD_SET(0, &rfds);
            maxfd = 0;
            /*把当前连接的文件描述符加入到集合中*/
            FD_SET(ser_sockfd, &rfds);
            /*找出文件描述符集合中最大的文件描述符*/
            if(maxfd < ser_sockfd)
            maxfd = ser_sockfd;
            retval = select(maxfd+1, &rfds, NULL, NULL, NULL);
            if(FD_ISSET(ser_sockfd,&rfds))//client发消息来会出发进入
            {
                len=recvfrom(ser_sockfd,seraddr,sizeof(seraddr),0,(struct sockaddr*)&ser_addr,&addrlen);                   
                printf("recevce:%s",seraddr);
            }
            if(FD_ISSET(0, &rfds))//键盘输入会触发进入
            {
                len=read(STDIN_FILENO,seraddr,sizeof(seraddr));
                sendto(ser_sockfd,seraddr,len,0,(struct sockaddr*)&ser_addr,addrlen);
            }
        }
    }
    return 0;
}

```

发送端：

```cpp
#include <netinet/in.h>
#include <string.h>
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <arpa/inet.h>
#define SERVER_PORT 8888 //唯一端口
int main(int argc,char **argv){
    int cli_sockfd;
    int len;
    fd_set rfds;
    socklen_t addrlen;
    struct sockaddr_in cli_addr;
    char buffer[256];
    char buffer1[256];
    int retval, maxfd;
    //创建socket
    cli_sockfd=socket(AF_INET,SOCK_DGRAM,0);
    if(cli_sockfd<0){
        printf("I cannot socket successn");
        return 1;
    }
    //配置地址与端口
    addrlen=sizeof(struct sockaddr_in);
    bzero(&cli_addr,addrlen);
    cli_addr.sin_family=AF_INET;
    cli_addr.sin_addr.s_addr=htonl(INADDR_ANY);//任何主机地址
    cli_addr.sin_port=htons(SERVER_PORT);
	//这里作为发送方，不需要绑定bind()
    while(1)
    {
        bzero(buffer,sizeof(buffer));
        /* 从标准输入设备取得字符串*/   len=read(STDIN_FILENO,buffer,sizeof(buffer));
        /* 将字符串传送给server端*/
        sendto(cli_sockfd,buffer,len,0,(struct sockaddr*)&cli_addr,addrlen);
        /* 接收server端返回的字符串*/
        while(1)
        {
            /*把可读文件描述符的集合清空*/
            FD_ZERO(&rfds);
            /*把标准输入的文件描述符加入到集合中*/
            FD_SET(0, &rfds);
            maxfd = 0;
            /*把当前连接的文件描述符加入到集合中*/
            FD_SET(cli_sockfd, &rfds);
            /*找出文件描述符集合中最大的文件描述符*/
            if(maxfd < cli_sockfd)
                maxfd = cli_sockfd;

            retval = select(maxfd+1, &rfds, NULL, NULL, NULL);

            if(FD_ISSET(cli_sockfd,&rfds))//server发来数据将会触发进入循环
            {

                len=recvfrom(cli_sockfd,buffer1,sizeof(buffer1),0,(struct sockaddr*)&cli_addr,&addrlen);
                printf("receive: %s",buffer1);
            }
            if(FD_ISSET(0, &rfds))//键盘输入会触发进入
            {
                len=read(STDIN_FILENO,buffer,sizeof(buffer));
                sendto(cli_sockfd,buffer,len,0,(struct sockaddr*)&cli_addr,addrlen);
            }

        }
    }
    close(cli_sockfd);
    return 0;
}

```

以上。