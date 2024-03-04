








#### 文章目录


* + [nano编辑器](#nano_2)
	+ [vim编辑器](#vim_12)
	+ [shell常用命令](#shell_50)



  
 Linux的文本编辑器有nano、gedit、vi、vim等。 

### nano编辑器


nano是ubuntu系统自带的编辑器，也是最容易学习上手的编辑器，但初始配置在使用起来有点不尽如人意，好在官方已经给我们留好了一些配置选项，下面就记录一下我常用的编辑器环境配置：


首先，新建文件`nano ~/.nanorc`，输入以下配置指令；


![在这里插入图片描述](https://img-blog.csdnimg.cn/df943d374f6b41ea9b2bfde0b4cac182.png)  
 然后，进入系统变量配置文件，`sudo nano /etc/nanorc`，将该文件中以上几条指令前的`#`去掉即可；


再次进入nano，行号、缩进、鼠标等就生效了。


### vim编辑器


编辑文件：`sudo vim /etc/vim/vimrc`


在配置文件中可以看到有下面这个if判断,意思是语法高亮,如果是被注释掉状态（#/"）,可以将其放开（删掉#/"号）：



```
  if has("syntax")
    syntax on
  endif


```

然后在配置文件的最后一行，输入以下内容，可以让vim变得更漂亮、舒服，使用也更方便。



```
  set nu   " 设置左侧行号
 set tabstop=4 " 设置tab键长度为4(注意=和数字4之间不要有空格，否则打开vim会报错)
  set cursorline  " 突出显示当前行
 set ruler " 在右下角显示光标位置的状态行
  set autoindent  " 自动缩进
 set nobackup " 覆盖文件时不备份


```

编辑完成后使用命令 :wq 进行保存退出。


vim编辑器快捷指令：



```
a/i 进入编辑模式
ESC 返回命令模式
：wq 保存并退出
dd 删除整行
yy 复制整行
p 粘贴
u 撤销
/字符串 从上到下搜索
？字符串 从下到上搜索

```

### shell常用命令



```
echo $HOSTNAME
echo $HOME
date
ifconfig
uname -a
ubuntu-release
uptime
who
tar -xvzf  tar -cvzf
grep 
| 管道符是命令的进阶
> 输出重定向（>> 追加）
* 命令通配符
systemctl restart network 重启网卡
if、then、fi 、case条件语句
for、while-do 循环语句

```

以上。





