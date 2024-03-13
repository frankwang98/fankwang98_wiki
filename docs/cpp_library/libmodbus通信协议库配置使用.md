







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍libmodbus通信协议库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__38)




### 😏1. 项目介绍


官网：`https://libmodbus.org/`


项目Github地址：`https://github.com/stephane/libmodbus`


`Libmodbus` 是一个用于通信协议 `Modbus` 的`开源 C 语言库`。Modbus 是一种常用的工业通信协议，用于在自动化设备之间进行数据交换。Libmodbus 提供了一组函数和工具，使开发者能够轻松地实现 Modbus 通信功能。


以下是 `Libmodbus` 库的一些主要特点和功能：



> 
> 1.Modbus 协议支持：Libmodbus 实现了 Modbus 协议的基本功能，包括 Modbus RTU（串行）和 Modbus TCP（以太网）两种通信方式。它支持 Modbus 主机和从机的通信，以及读取和写入 Modbus 寄存器的操作。
> 
> 
> 



> 
> 2.跨平台支持：Libmodbus 提供了跨平台的支持，可以在多个操作系统上运行，包括 Linux、Windows、macOS 等。
> 
> 
> 



> 
> 3.简单易用：Libmodbus 提供了简洁的 API，使得开发者能够方便地集成 Modbus 功能到他们的应用程序中。它提供了一组函数，用于建立连接、读写寄存器、处理异常等。
> 
> 
> 



> 
> 4.多种编程语言支持：虽然 Libmodbus 是一个 C 语言库，但还提供了其他编程语言的绑定，如 Python、Java 等。这使得开发者可以使用他们熟悉的编程语言来使用 Libmodbus。
> 
> 
> 


`Libmodbus` 是一个广泛使用的 Modbus 库，适用于各种工业自动化和物联网应用。


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libmodbus-dev

```


```
# 编译
g++ -o main main.cpp  -lmodbus

```

### 😆3. 使用说明


下面进行使用分析：


Modbus RTU串行读取和写入示例：



```
#include <iostream>
#include <modbus/modbus.h>

int main() {
    modbus_t\* modbusContext;
    uint16\_t readBuffer[64];  // 用于存储读取的数据
    const int slaveId = 1;    // 从机 ID
    const int registerAddress = 0;  // 寄存器地址
    const int numRegisters = 1;     // 寄存器数量
    const int coilAddress = 0;  // 线圈地址
    const int numCoils = 1;     // 线圈数量

    // 初始化 - 设备号、波特率、校验位、数据位、停止位
    modbusContext = modbus\_new\_rtu("/dev/ttyUSB0", 9600, 'N', 8, 1);
    if (modbusContext == nullptr) {
        std::cerr << "Failed to create Modbus context" << std::endl;
        return 1;
    }

    // 打开 Modbus 连接
    if (modbus\_connect(modbusContext) == -1) {
        std::cerr << "Modbus connection failed: " << modbus\_strerror(errno) << std::endl;
        modbus\_free(modbusContext);
        return 1;
    }

    // 读取寄存器
    int rc = modbus\_read\_registers(modbusContext, registerAddress, numRegisters, readBuffer);
    if (rc == -1) {
        std::cerr << "Failed to read Modbus registers: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Read value: " << readBuffer[0] << std::endl;
    }

    // 写入寄存器
    const uint16\_t writeValue = 1234;
    rc = modbus\_write\_register(modbusContext, registerAddress, writeValue);
    if (rc == -1) {
        std::cerr << "Failed to write Modbus register: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Write successful" << std::endl;
    }

    // 读取线圈状态
    uint8\_t coilStatus;
    rc = modbus\_read\_bits(modbusContext, coilAddress, numCoils, &coilStatus);
    if (rc == -1) {
        std::cerr << "Failed to read Modbus coils: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Coil value: " << static\_cast<int>(coilStatus) << std::endl;
    }

    // 写入线圈状态
    const uint8\_t writeValue2 = 1;
    rc = modbus\_write\_bit(modbusContext, coilAddress, writeValue2);
    if (rc == -1) {
        std::cerr << "Failed to write Modbus coil: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Write successful" << std::endl;
    }

    // 关闭 Modbus 连接
    modbus\_close(modbusContext);
    modbus\_free(modbusContext);

    return 0;
}

```

Modbus TCP以太网读取写入示例：



```
#include <iostream>
#include <modbus/modbus.h>

int main() {
    modbus_t\* modbusContext;
    const char\* ipAddress = "192.168.1.100"; // Modbus TCP 从机的 IP 地址
    const int port = 502; // Modbus TCP 端口号
    const int slaveId = 1; // 从机 ID

    modbusContext = modbus\_new\_tcp(ipAddress, port);
    if (modbusContext == nullptr) {
        std::cerr << "Failed to create Modbus context" << std::endl;
        return 1;
    }

    // 打开 Modbus 连接
    if (modbus\_connect(modbusContext) == -1) {
        std::cerr << "Modbus connection failed: " << modbus\_strerror(errno) << std::endl;
        modbus\_free(modbusContext);
        return 1;
    }

    // 读取线圈状态
    const int coilAddress = 0; // 线圈地址
    const int numCoils = 1; // 线圈数量
    uint8\_t coilStatus[numCoils];
    int rc = modbus\_read\_bits(modbusContext, coilAddress, numCoils, coilStatus);
    if (rc == -1) {
        std::cerr << "Failed to read Modbus coils: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Coil value: " << static\_cast<int>(coilStatus[0]) << std::endl;
    }

    // 写入线圈状态
    const uint8\_t writeValue = 1;
    rc = modbus\_write\_bit(modbusContext, coilAddress, writeValue);
    if (rc == -1) {
        std::cerr << "Failed to write Modbus coil: " << modbus\_strerror(errno) << std::endl;
    } else {
        std::cout << "Write successful" << std::endl;
    }

    // 关闭 Modbus 连接
    modbus\_close(modbusContext);
    modbus\_free(modbusContext);

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





