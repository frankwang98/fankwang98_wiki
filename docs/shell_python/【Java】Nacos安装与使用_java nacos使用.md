







> 
> 阿里巴巴发起的开源项目Nacos，是一个注册、配置中心（Naming And Config），支持各种语言的客户端。一般用于配置Spring Boot项目。
> 
> 
> 




#### 文章目录


* + [1. Windows安装Nacos](#1_WindowsNacos_3)
	+ [2. Nacos使用](#2_Nacos_49)




### 1. Windows安装Nacos


地址：`https://github.com/alibaba/nacos/releases`


下载：


![在这里插入图片描述](https://img-blog.csdnimg.cn/8580b2d182d74f4186ec0f68f9bdd518.png)


解压后：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6053263ab99a413d9eed1a22d1ab238f.png)



```
bin里面是启动和关闭nacos命令文件；
conf存储的nacos相关的配置文件；
logs日志信息
target里有一个springboot的jar包

```

配置数据库：


本地创建MYSQL数据库nacos，导入conf文件夹中的nacos-mysql.sql脚本。


![在这里插入图片描述](https://img-blog.csdnimg.cn/11111cbe93d54c55859deddb950772f3.png)


修改配置文件：



```
修改conf路径下的配置文件application.properties
 将下图中的数据库配置注释放开，同时修改数据库账户和密码；
 此中的数据库nacos与建立的数据库名保持一致；

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7f050a1c26d1422b86be9691cf05370c.png)


启动服务：


在bin文件夹下执行命令：`startup.cmd -m standalone`  
 其中-m standalone指定为单机模式，否则以cluster集群模式启动。


![在这里插入图片描述](https://img-blog.csdnimg.cn/307885844eb44f4d813c7f41d23d22ac.png)


访问nacos：


地址: `http://localhost:8848/nacos`  
 账号/密码: `nacos/nacos`


### 2. Nacos使用


C++版本的看这个大佬：`https://github.com/nacos-group/nacos-sdk-cpp`


以上。





