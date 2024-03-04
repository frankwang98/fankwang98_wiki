







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍crowcpp微型web框架配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__11)
	+ [:satisfied:3. 使用说明](#satisfied3__26)




### 😏1. 项目介绍


项目Github地址： `https://github.com/CrowCpp/Crow`


（另外，github还有一个 [crow](https://github.com/ipkn/crow) 也是微型web框架，是crowcpp的原版，悄悄说一下，crowcpp的编译较为容易）


### 😊2. 环境配置


下面进行环境配置：



```
# 安装依赖项
sudo apt install build-essential cmake git libboost-all-dev libssl-dev libasio-dev
# 源码编译
git clone https://ghproxy.com/https://github.com/CrowCpp/Crow
cd Crow
mkdir build && cd build
cmake .. -DCROW\_BUILD\_EXAMPLES=OFF -DCROW\_BUILD\_TESTS=OFF
sudo make install
sudo ldconfig

```

### 😆3. 使用说明


crowcpp目前使用也较为广泛，也有丰富的案例。


一个crowcpp创建web示例：



```
#include <crow.h>

int main()
{
    crow::SimpleApp app;

    CROW\_ROUTE(app, "/")
    ([]{
        return "Hello, World!";
    });

    app.port(8080).multithreaded().run();

    return 0;
}


```

编译运行：



```
g++ -o main main.cpp -lpthread （不用-lcrow）
./main
# 浏览器 0.0.0.0:8080

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a8b7d9130add484fa849591b2239c192.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





