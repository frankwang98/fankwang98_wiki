






**问题如下：**  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623092712771.png)  
 **解决方法如下：**  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062309282817.png)



```
sudo rm /var/lib/dpkg/lock
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/cache/apt/archives/lock

```

经过证明，输入以上三个命令即可解除占用。


解除后，继续运行apt命令，已经顺利运行了。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623092959684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)  
 以上。





