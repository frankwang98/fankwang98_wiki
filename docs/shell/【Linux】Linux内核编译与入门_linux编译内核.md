







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Linux内核编译。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Linux内核介绍](#smirk1_Linux_7)
	+ [:blush:2. Linux内核编译](#blush2_Linux_15)
	+ [:satisfied:3. Linux内核使用](#satisfied3_Linux_83)




### 😏1. Linux内核介绍


Linux内核是一种开源操作系统内核，它是基于Unix系列操作系统的设计思想和原则。与其他操作系统内核相比，Linux内核具有很多特点，例如高度可定制、模块化设计、强大的网络支持、多处理器支持、安全性、稳定性等。


Linux内核最初由芬兰程序员Linus Torvalds于1991年创建，并在全球范围内得到了广泛的使用和支持。现在，Linux内核已经成为许多流行的操作系统的核心，包括Ubuntu、Red Hat、Debian、CentOS等。


在Linux内核中，各种设备和功能都采用模块化设计，这使得内核可以灵活扩展，只需加载必要的模块即可实现所需功能。此外，Linux内核还支持多种文件系统和文件系统类型，例如ext4、xfs、btrfs、nfs等。


总体来说，Linux内核是一个高度可定制的、功能丰富的、稳定的操作系统内核，其开放源代码和广泛的社区支持使其成为开发者和用户的首选之一。


### 😊2. Linux内核编译


首先准备一台Linux机器，查看内核版本：`uname -r`


根据获取的linux kernel版本，在`www.kernel.org`上面下载合适的kernel版本。（如我的Ubuntu18.04内核版本是5.4.0，安装版本选择5.4.244）


![在这里插入图片描述](https://img-blog.csdnimg.cn/5ed3aec436864b319305262812db2d18.png)


解压后，将boot下config文件拷贝到本地：`cp -v /boot/config-$(uname -r) .config`


然后编辑`.config`文件：



```
vim .config
# 将该项原有内容删掉即可，如下
CONFIG\_SYSTEM\_TRUSTED\_KEYS=""

```

输入`make menuconfig` 启动配置界面，小白直接保存即可；


安装依赖：



```
sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison dwarves

```

开始编译内核：



```
# 根据机器选择核数 -j x
make -j 10	#（时间很长）
# 安装模块
sudo make modules_install
# 安装内核
sudo make install

```

完成后的结果如下（不容易呀）：


![请添加图片描述](https://img-blog.csdnimg.cn/6239ac6ec093425b9a2755baa530caef.png)


然后重启电脑：



```
sudo update-grub
sudo reboot

```

如果出现`error vmlinuz has invalid signature 【或者】 mmx64.efi not found`这种错误，是因为bios开启了安全启动，去`bios-secure boot`，设置为`disable`（禁用安全模式）即可。


正常启动后，查看当前内核版本：`uname -r`


![在这里插入图片描述](https://img-blog.csdnimg.cn/735c1d530811435fb747997bbdf5a123.png)


内核编译成功。


卸载内核：



```
# 改为自己的内核版本，我这里是5.4.244
sudo rm -rf  /boot/vmlinuz*5.4.244* /boot/initrd*5.4.244* /boot/System-map*5.4.244* /boot/config-*5.4.244* /lib/modules/*5.4.244*/ /var/lib/initramfs-tools/*5.4.244*/
sudo apt-get purge linux-image-****-generic
# 更新grub配置
sudo update-grub2

```

查看自己安装了哪些内核：



```
sudo dpkg --get-selections  |  grep  'linux'

```

### 😆3. Linux内核使用


内核目录如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/06cad79fb55f4bbab447968ea266ed80.png)


可以基于此学习Linux驱动开发、应用开发等，还可以在新内核的基础上进行裁剪等操作。


嵌入式Linux学习路线：



> 
> 1.嵌入式开发基础知识：学习 C/C++ 编程语言、计算机体系结构、操作系统和嵌入式系统的基础概念。 Linux 系统管理员技能：熟悉  
>  2.Linux 操作系统的基本命令行和文件系统，了解如何管理用户帐户和权限，如何安装软件包等。  
>  3.嵌入式 Linux 知识：学习如何配置和定制Linux 内核、驱动程序和 bootloaders，以及嵌入式设备的文件系统和启动过程。  
>  4.嵌入式开发板的选择和使用：学习如何选择适合您项目需求的嵌入式开发板，了解如何调试和测试硬件和软件。  
>  5.特殊的应用场景：如实时操作系统、网络编程、多线程编程、图像处理等。 项目经验：完成一些小型嵌入式项目，如控制LED、读取传感器数据等，并逐步提高难度，最终达到完成完整项目的能力。
> 
> 
> 


参考：



```
http://t.csdn.cn/7F656
https://zhuanlan.zhihu.com/p/378149586

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





