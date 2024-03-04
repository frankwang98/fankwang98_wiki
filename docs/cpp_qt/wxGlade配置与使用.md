> 学习wxGlade是因为Autoware.ai的交互界面是用它做的。

wxGlade是基于wxPython的一款跨平台GUI开发工具，以下是在Ubuntu系统下的环境配置。


### 1.安装Gnome/GTK


wxGlade需要有GTK的前置环境，否则下一步pip install wxpython会出错。安装命令如下：



```
sudo apt-get install gnome-devel

```

这里我安装了Gnome，应该是包含了gtk，另外还会自动安装一些小工具，如果想简洁安装，可以试试其他单独安装gtk的方法。  
 如： `sudo apt-get install libgtk2.0-dev` （未测试）


### 2.pip安装wxpython


这一步比较简单，但耗时较长。安装命令如下：（这一步有错了）



```
pip install wxpython

```

2022.8.8 记录  
 wxpython安装版本有错的话会导致autoware打不开，错误如下：


![请添加图片描述](https://img-blog.csdnimg.cn/6b7b8e955e36452387b68061ab3deabd.png)


`pip install wxpython`安装得到的版本是wxpython 4.1.1并不是4.0.7版本，因此可以查询自己的版本，重新在官网上下载对应版本的wxpython包：  
 [wxpython 4.0.7下载](https://extras.wxpython.org/wxPython4/extras/linux/gtk2/ubuntu-18.04/)


下载wxPython-4.0.7-cp27-cp27mu-linux\_x86\_64.whl，然后在下载目录下打开终端：



```
python2.7 -m pip install wxPython-4.0.7-cp27-cp27mu-linux_x86_64.whl

```

然后修改runtime\_manager\_dialog.py 脚本文件：  
 找到 autoware.ai/src/autoware/utilities/runtime\_manager/scripts 中的 runtime\_manager\_dialog.py 文件


* 在文件中添加 import wx.adv
* 把文件中所有的 wx.HyperlinkCtrl 替换成 wx.adv.HyperlinkCtrl
* 把文件中所有的 wx.EVT\_HYPERLINK 替换成 wx.adv.EVT\_HYPERLINK


改完后重新编译即可。


### 3.下载wxglade源码


github:`https://github.com/wxGlade/wxGlade`  
 gitee:`https://gitee.com/mirrors/wxGlade/`


### 4.打开wxGlade GUI开发环境


上面的源码下载号之后，进入 wxglade 目录，找到以下两个脚本，用python xxx打开即可（两个都可以用）。



```
python wxglade

```

![请添加图片描述](https://img-blog.csdnimg.cn/a078234019a44990a893a06562b54ff1.png)


打开后，软件界面如下：


![请添加图片描述](https://img-blog.csdnimg.cn/78eacd13434640d0aaee377cbf1239e5.png)


### 5.帮助文档


在该目录下 `wxGlade/docs/html/index.html`，有教程，直接用浏览器打开即可。


以上。





