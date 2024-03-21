






在Linux学习的时候，Windows和Linux之间有文件互传的需求，虚拟机还好，可以拖来拖去，但双系统来回切换肯定不方便，因此需要搭建一个Samba共享服务器来共享文件夹。


1.安装samba：



```
sudo apt-get install samba

```

2.安装smbclient



```
sudo apt-get install smbclient

```

3.创建共享文件夹



```
/home/xxx/share

```

4.修改配置文件



```
sudo gedit /etc/samba/smb.conf

```

在末尾加入：



```
[share]
    comment = shared folder
    browseable = yes
    path = /home/wangzf/share
    create mode = 0775
    directory mode = 0700
    valid user = wangzf
    force user = wangzf
    force group = wangzf
    public = yes
    available = yes
    writeable = yes

```

5.重启服务



```
sudo service smbd restart

```

6.设置samba账号密码



```
sudo smbpasswd -a wangzf

```

直接enter即表示空密码：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/ea26602961b94a0a960e5b8ba26bbe8c.png)


7.查看Windows/Linux的IP



```
ifconfig	//Linux
ipconfig	//Windows

```

8.查看共享文件


Windows端：


写入Linux端的IP和共享文件夹名称即可。



```
\\192.168.126.128\share

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e91226f68702418085cb1ce8093b38e3.png)


以上。





