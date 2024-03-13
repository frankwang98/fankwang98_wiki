







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍OpenDDS配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__42)




### 😏1. 项目介绍


项目Github地址：`https://github.com/OpenDDS/OpenDDS`


官网：`https://opendds.org/`


`OpenDDS`（`Open Data Distribution Service`）是一个开源的、高性能的实时数据分发和通信框架，符合`OMG`（`Object Management Group`）发布的`Data Distribution Service（DDS）`标准。它提供了分布式系统中实时通信和数据交换的基础设施，支持发布者-订阅者模型，使分布式应用程序能够可靠地交换数据。


以下是OpenDDS的一些主要特点和功能：



> 
> 1.数据分发：OpenDDS提供了可靠的数据分发机制，可以在分布式系统中高效地传输数据。它支持灵活的QoS（Quality of Service）策略，可以根据应用程序的需求配置数据交换的可靠性、传输速率、延迟、带宽等参数。
> 
> 
> 



> 
> 2.发布者-订阅者模型：OpenDDS基于发布者-订阅者模型，发布者将数据发布到特定的主题（Topic），而订阅者通过订阅相应的主题来接收数据。这种模型使得多个应用程序能够以异步、解耦的方式进行实时数据交换。
> 
> 
> 



> 
> 3.多种数据类型支持：OpenDDS支持多种数据类型的交换，包括结构体、数组、枚举和序列等。它使用IDL（Interface Definition Language）来定义数据类型，并自动生成相应的代码和类型支持。
> 
> 
> 



> 
> 4.可扩展性：OpenDDS具有良好的可扩展性，可以处理大规模分布式系统中的复杂通信需求。它支持动态发现和自适应性，可以自动发现和适应系统中的节点和资源变化。
> 
> 
> 



> 
> 5.平台支持：OpenDDS可在多个平台上运行，包括Linux、Windows和macOS等。它提供了对不同操作系统和网络协议的支持，并且可以与其他编程语言（如C++、Java和Python）进行集成。
> 
> 
> 



> 
> 6.社区支持：OpenDDS是一个活跃的开源项目，拥有一个积极的社区，提供了广泛的文档、示例代码和讨论论坛，以帮助开发人员学习和使用OpenDDS。
> 
> 
> 


`OpenDDS`是一个功能强大的实时数据分发和通信框架，适用于构建要求高性能、可靠性和实时性的分布式应用程序。它提供了丰富的功能和配置选项，可以根据应用程序的需求进行灵活的配置和定制。


### 😊2. 环境配置


下面进行环境配置：



```
sudo apt-get install build-essential libace-dev libssl-dev
# 下载对应版本
https://opendds.org/downloads.html
# configure会下载ACE+TAO网络包，如果访问github慢，可以在configure的878和886行添加镜像源https://mirror.ghproxy.com/
./configure
# 编译
make

```

### 😆3. 使用说明


官方示例：



```
source setenv.sh
cd DevGuideExamples/DCPS/Messenger
./run_test.pl

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





