







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍基于Hyper-V安装Ubuntu虚拟机。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Hyper-V介绍](#smirk1_HyperV_7)
	+ [:blush:2. Ubuntu虚拟机安装](#blush2_Ubuntu_23)
	+ [:satisfied:3. 其他操作](#satisfied3__36)




### 😏1. Hyper-V介绍


`Hyper-V` 是一种由 `Microsoft` 开发的虚拟化技术和虚拟化平台。它是 `Windows` 操作系统的一部分，并提供了在主机操作系统上运行多个虚拟机的能力。


以下是一些关于 `Hyper-V` 的主要特点和功能：



> 
> 1.虚拟化平台: Hyper-V 允许在物理服务器上创建和管理虚拟机。每个虚拟机都可以运行独立的操作系统，并具有自己的资源分配和配置。
> 
> 
> 



> 
> 2.硬件虚拟化: Hyper-V 使用硬件虚拟化技术（如 Intel VT 或 AMD-V）来提供高性能和隔离的虚拟化环境。这使得虚拟机能够直接访问主机的物理硬件资源。
> 
> 
> 



> 
> 3.虚拟网络: Hyper-V 具有强大的虚拟网络功能，可以创建和管理虚拟交换机、虚拟网络适配器和虚拟网络连接。这样，虚拟机可以连接到虚拟网络并与其他虚拟机或物理网络进行通信。
> 
> 
> 



> 
> 4.快照和复制: Hyper-V 支持创建虚拟机快照，以便在需要时能够还原到以前的状态。此外，它还提供了虚拟机复制功能，允许将虚拟机的副本复制到其他 Hyper-V 主机上。
> 
> 
> 



> 
> 5.动态内存: Hyper-V 具有动态内存功能，可以根据需要自动调整虚拟机的内存分配。这可以提高资源利用率，并允许更多的虚拟机在同一台物理服务器上运行。
> 
> 
> 



> 
> 6.故障转移和负载均衡: Hyper-V 支持虚拟机的故障转移，即在物理服务器发生故障时将虚拟机自动迁移到其他可用的物理服务器上。它还提供了负载均衡功能，可将虚拟机均匀地分布在多个物理服务器上，以提高性能和可靠性。
> 
> 
> 


### 😊2. Ubuntu虚拟机安装


首先，确认我们的windows来开启了`hyper-v`服务，然后打开`hyper-v`管理器。


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/23cc7532389a4e7181ea2e524b345333.png)


可以先设置我们的默认`安装位置`，比如D盘：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/7952cdb643b3475c8edf51bb53076946.png)  
 然后在快速创建里，我们可以选择默认的windows或ubuntu`安装源`，也可以自定义本地的安装源（从官网下载iso镜像），然后继续安装即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/06b110c66da341528130d2cc9acf9285.png)


### 😆3. 其他操作


我是选择的本地安装源，安装完成后，系统分辨率很小，查了下，可以通过修改系统配置来调整默认分辨率：



```
sudo nano /etc/default/grub
# 找到这一行 GRUB\_CMDLINE\_LINUX\_DEFAULT=“quiet splash” 修改为
GRUB\_CMDLINE\_LINUX\_DEFAULT=“quiet splash video=hyperv_fb:1920x1080” # 这里的分辨率根据需要来填
sudo update-grub # 更新配置
sudo reboot # 重启系统生效

```

最终效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e8d7713d10e84a58ae76f5609a5550eb.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





