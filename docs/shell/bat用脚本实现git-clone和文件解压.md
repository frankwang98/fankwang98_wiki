






今天有一个简单的需求，从git上clone下来程序包，然后有的文件还需要解压，在Windows下，想着用bat程序就可以解决，`bat`是适合解决一些程序的自动化处理的，类似于Linux中的`shell`脚本，在解决过程中遇到几个问题记录一下：




#### 文章目录


* + - [bat基本结构](#bat_3)
		- [调用git实现clone](#gitclone_23)
		- [调用Bandizip实现文件解压](#Bandizip_40)




#### bat基本结构


首先，我这个bat自动化脚本不需要和用户交互，所以关闭回显：`@echo off`


rd是删除目录，del是删除文件，这里我要确认当前目录下这个文件夹不存在，存在的话就给他删掉，所以用`rd /s /q D:\xxx`


程序的最后，如果写的是`exit`的话执行完后会自动退出窗口，如果写的是`pause`，会等待你输入一个任意按键。


![在这里插入图片描述](https://img-blog.csdnimg.cn/bd9709d449144c1fbce36913069fde1f.png)


如：



```
::------------------------------
::注释
::------------------------------
@echo off
::echo success
::exit
pause

```

#### 调用git实现clone


首先，需要安装好git软件。


![在这里插入图片描述](https://img-blog.csdnimg.cn/0c567667cdb7487b8987988413e0ab99.png)


然后，在bat程序里设置好环境变量，也就是说，要让命令行知道我可以去哪里调用git这个命令，找到git的安装目录，然后添加：`set GIT_HOME=D:\Program Files\Git\bin`


然后就使用`git clone xxx`这个命令了。


如：



```
set GIT\_HOME=D:\Git\bin
cd /d C:\Users\dev\Desktop
git clone https://gitee.com/heyuchick/hello-world.git


```

#### 调用Bandizip实现文件解压


如果有zip压缩文件，怎么用bat脚本自动解压呢。


首先还是要定义环境变量：`set ZIP_HOME=C:\Program Files\Bandizip`，让cmd能找到命令。


然后解压：`Bandizip.exe x photo.zip`


解压完之后，会保留解压完成的窗口，如果不关掉它后面的程序无法执行，刚开始我是用串行处理，发现不行，然后准备新开一个窗口，去关掉bandizip这个进程：



```
Bandizip.exe x 1.zip
taskkill /f /im Bandizip.exe

```

新开窗口也是串行处理，所以处理这种情况就需要进行并行处理，并行处理时，一般是先定义cmd需要执行哪些命令，然后start开启一个线程，算是多线程处理吧。


如：



```
set cmd1=Bandizip.exe x 1.zip C:\Users\dev\Desktop
set cmd2=taskkill /f /im Bandizip.exe
start %cmd1%
sleep 3
start %cmd2%

```

这样就基本实现了想要的功能，脚本如下：



```
@echo off

@echo off
set GIT\_HOME=D:\Git\bin
set ZIP\_HOME=C:\Program Files\Bandizip
rd /s /q C:\Users\dev\Desktop\hello

cd /d C:\Users\dev\Desktop
git clone https://gitee.com/jutopia/hello.git

set cmd1=Bandizip.exe x 1.zip 
set cmd2=taskkill /f /im Bandizip.exe
start %cmd1%
sleep 3
start %cmd2%

echo success
::exit
pause

```

以上。





