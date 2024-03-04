






今天早上打开虚拟机，莫名上不了网了，打开网络设置一看是未识别到网卡/网络服务被禁用了，而Windows下的网络是正常的，于是可以判断是ubuntu虚拟机的网络服务出现问题了。


通过重启虚拟机下的网络服务解决了这一问题：


![在这里插入图片描述](https://img-blog.csdnimg.cn/1888877d34c9482b9daf43a05c424d12.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_19,color_FFFFFF,t_70,g_se,x_16)



```
sudo service network-manager stop
sudo rm /var/lib/NetworkManager/NetworkManager.state
sudo service network-manager start

```

以上。





