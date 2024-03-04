








#### 文章目录


* + [1.为什么需要FileZilla？](#1FileZilla_1)
	+ [2.环境配置](#2_6)
	+ [3.如果是Ubuntu系统，确保打开了FTP服务](#3UbuntuFTP_14)




### 1.为什么需要FileZilla？


最近在玩树莓派的过程中，发现有想把树莓派这个小电脑上的文件拷下来的需求，找了一会，发现了这个神器。  
 使用很方便，只要输入目标主机的IP、用户名和密码就可以连接并显示它的文件目录，右击下载就可以下载到当前电脑了。  
 应该还有更多功能，没有深入去探索。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a60022cb394a4168918faff7f9f80668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


### 2.环境配置


官方下载地址：`https://www.filezilla.cn/download`  
 根据自己的系统（Windows、Linux），下载客户端即可。  
 安装傻瓜式操作一气呵成。


![在这里插入图片描述](https://img-blog.csdnimg.cn/a8bdf9e831134d19b6cfbfd57fc359a7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


### 3.如果是Ubuntu系统，确保打开了FTP服务


1.打开终端（Ctrl+Alt+T），输入如下命令安装FTP服务；



```
sudo apt-get install vsftpd

```

2.安装完成以后使用如下gedit命令打开/etc/vsftpd.conf，命令如下：



```
sudo gedit /etc/vsftpd.conf

```

3.打开以后 vsftpd.conf 文件以后找到如下两行：



```
local\_enable=YES
write\_enable=YES

```

确保这两行前面没有注释符（#）即可。  
 4.修改完 vsftpd.conf 以后保存退出，使用如下命令重启 FTP 服务：



```
sudo /etc/init.d/vsftpd restart

```

这样就可以正常进行文件传输了。


以上。





