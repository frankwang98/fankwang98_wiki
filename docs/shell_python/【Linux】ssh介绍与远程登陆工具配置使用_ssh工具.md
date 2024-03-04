







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍ssh介绍与远程登陆配置使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. ssh介绍](#smirk1_ssh_7)
	+ [:blush:2. ssh工具](#blush2_ssh_35)
	+ [:satisfied:3. ssh-vscode插件](#satisfied3_sshvscode_46)




### 😏1. ssh介绍


`SSH`（Secure Shell）是一种网络协议和安全工具，用于在不安全的网络上安全地进行远程登录和数据传输。它提供了加密的通信通道，以保护敏感数据的机密性和完整性。


SSH协议支持多种应用，其中最常用的是`SSH`（远程登录）、`SCP`（Secure Copy Protocol）和`SFTP`（SSH File Transfer Protocol）。


1. SSH登录：  
 SSH登录是通过SSH协议远程连接到服务器并执行命令的过程。您可以使用SSH客户端（如OpenSSH）连接到远程服务器并提供所需的身份验证信息（用户名和密码或SSH密钥）。以下是使用SSH命令进行远程登录的示例：



```
ssh username@remote_host

```

2. SCP（Secure Copy Protocol）：  
 SCP是基于SSH协议的安全文件传输协议，用于在本地系统和远程服务器之间进行文件传输。它与cp命令类似，但通过加密通道进行数据传输。以下是使用SCP命令将本地文件复制到远程服务器的示例：



```
scp local_file username@remote_host:remote_location # 本地到远程
scp username@remote_host:remote_file local_location # 远程到本地

```

3. SFTP（SSH File Transfer Protocol）：  
 SFTP是基于SSH协议的安全文件传输协议，提供了对远程文件系统的完整访问。它类似于FTP，但使用加密通道进行数据传输。以下是使用SFTP命令进行远程文件操作的示例：



```
sftp username@remote_host
# 这将建立一个SFTP会话，并将您连接到远程服务器的主目录。可以使用各种命令（如`get`、`put`、`ls`、`cd`等）进行文件和目录操作
get remote_file local_location
put local_file remote_location

```

### 😊2. ssh工具


工欲善其事，必先利其器。


ssh在多平台上均可使用。大多数Linux发行版和macOS都默认安装了SSH客户端和服务器，可通过下列命令确认安装：



```
sudo apt install openssh-server openssh-client

```

Windows端可以使用第三方SSH客户端软件，如`PuTTY`、`OpenSSH for Windows`、`secureCRT`、`Xmanager（包含xshell、xftp）`等。


ssh的客户端工具目前可选的还是比较多，但目前我常用的是`Mobaxterm`。它提供了所有重要的远程网络工具（如`SSH、X11、RDP、VNC、FTP`等），以及Windows 上的Unix命令（`bash、ls、cat、sed、grep`等），且登录后默认开启`sftp`模式，终端操作和文件操作都比较方便。


### 😆3. ssh-vscode插件


在日常开发中，除了终端操作和文件上传下载，最令人头疼的是如何远程修改服务器端的文件了。同时也回应很多粉丝要求，来分析远程操作服务端电脑文件的插件使用。


如果远端电脑安装了`nomachine`这类远程图形化桌面工具倒还好，可以直接图形化操作。如果没有的话，推荐使用`vscode`里的`ssh tools`插件来远程访问文件。


当然vscode也有其他ssh远程工具可选择，如官方的`remote-ssh`等，但使用下来感觉`ssh tools`更方便操作。操作示意如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/49ec017a656241e0acc34c3bfe29392b.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/aa30aa00df9f430f9405b05743a00c47.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/c30b276ec1c14e72a13428b080ed18b9.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





