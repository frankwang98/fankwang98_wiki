






谷歌chrome浏览器有时会遇到下载链接自动被拦截的问题，在Win系统中可以用“迅雷”来下载某链接，但linux系统中没有迅雷，这里记录一种类似迅雷的下载器——uget和aria2。


一. 安装uget



```
sudo add-apt-repository ppa:plushuang-tw/uget-stable 
sudo apt-get update 
sudo apt-get install uget

```

二. 安装aria2



```
sudo add-apt-repository ppa:t-tujikawa/ppa 
sudo apt-get update 
sudo apt-get install aria2

```

三. 设置uget和aria2


在应用软件中打开`uget`，选择`edit->setting->plugin->plug-in matching order`，选择`aria2`；


参数默认即可：`--enable-rpc=true -D --disable-ipv6 --check-certificate=false`


然后在终端执行：



```
sudo add-apt-repository --remove ppa:t-tujikawa/ppa

```

可解决下载速度慢的问题；


接下来就可以用uget来下载链接了，尽情使用吧。


以上。





