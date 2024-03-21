






自从装上ubuntu系统后，有时关机老是卡死在那里，应该是某些进程没有结束的原因，系统默认处理超时的时间是90s，但有时电脑一晚上都关不掉，这里先总结2种方法，应该能解决大多数情况：




#### 文章目录


* + [1.通过设置默认停止超时时间](#1_3)
	+ [2.top查看和关闭进程](#2top_23)
	+ [3. 可靠的关机方式](#3__26)
	+ [|、||、&、&&辨析](#_33)
	+ - [tee](#tee_44)
		- [tail](#tail_54)
		- [killall](#killall_64)




### 1.通过设置默认停止超时时间


关机的默认等待时间默认为 90 秒。在这个时间之后，你的系统会尝试强制停止服务。但一般情况下，我们会想让ubuntu的关机和开机一样快，这时我们就可以修改这个时间。


在位于 `/etc/systemd/system.conf` 的配置文件中找到所有的系统设置。该文件中包含很多以 # 开头的行，代表了文件中各条目的默认值。


在开始之前，最好先复制一份原始文件。



```
sudo cp /etc/systemd/system.conf /etc/systemd/system.conf.orig

```

在这里找到 DefaultTimeoutStopSec，它被设置为 90 秒。



```
#DefaultTimeoutStopSec=90s

```

可以更改这个值，比如 5 秒或 10 秒。然后删掉前面的`#`，保存文件并重启系统。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/49b04322b5364c96992bad10f5f16435.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)


### 2.top查看和关闭进程


Ctrl+Alt+F1，进入TTY1终端，终端输入`top`命令查看进程，`kill`命令杀掉卡住的进程即可。


### 3. 可靠的关机方式



```
sudo sync
sudo shutdown -h now

```

### |、||、&、&&辨析


竖线‘|’在linux中是管道符的意思，将‘|’前面命令的输出作为’|'后面的输入；


双竖线‘||’，用双竖线‘||’分割的多条命令，执行的时候遵循如下规则：如果前一条命令为真，则后面的命令不会执行，如果前一条命令为假，则继续执行后面的命令；


&同时执行多条命令，不管命令是否执行成功；


&&可同时执行多条命令，当碰到执行错误的命令时，将不再执行后面的命令。如果一直没有错误的，则执行完毕。


用的时候，先记住‘|’是管道符，&是并行执行，‘||’和&&分别是他们的进阶版。


#### tee


tee是一种文件管理命令，tee命令用于读取标准输入的数据，并将其内容输出成文件。如：



```
tee 1.txt
ls -l | tee 2.txt

```

可用于打印终端输出和日志等。


#### tail


tail 命令可用于查看文件的内容，有一个常用的参数-f，常用于查阅正在改变的日志文件。如：



```
tail 1.txt
tail -f 1.txt

```

可用于查看日志文件变化。


#### killall


kill 命令杀死指定进程 PID，需要配合 ps 使用，而 killall 直接对进程对名字进行操作，更加方便。kill后常跟PID代号，而killall后常跟进程名。如：



```
kill -9 8178
killall -9 bash
killall -9 roscore
killall -9 rosmaster

```

可用于退出进程。


以上。





