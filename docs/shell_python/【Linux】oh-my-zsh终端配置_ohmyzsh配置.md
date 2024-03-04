







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍oh-my-zsh终端配置。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. oh-my-zsh介绍](#smirk1_ohmyzsh_7)
	+ [:blush:2. 可用的shell终端](#blush2_shell_25)
	+ [:satisfied:3. oh-my-zsh安装与配置](#satisfied3_ohmyzsh_39)




### 😏1. oh-my-zsh介绍


Oh-My-Zsh是一个开源的命令行工具，它是基于Zsh shell的一个扩展框架。Zsh是一种强大的交互式shell，比默认的Bash shell功能更强大，并且提供了更多的定制选项和插件支持。


Oh-My-Zsh的目标是简化Zsh的配置过程，使其更易于使用和定制。它提供了一个预配置的设置，包括主题（用于美化终端外观）和插件（用于增强功能）。通过使用Oh-My-Zsh，用户可以快速设置和配置个性化的命令行环境。


以下是Oh-My-Zsh的一些特性：



> 
> 主题：Oh-My-Zsh提供了许多漂亮的主题选择，可以改变终端的外观和风格。用户可以根据自己的喜好选择合适的主题。
> 
> 
> 插件：Oh-My-Zsh具有丰富的插件生态系统，用户可以轻松地启用或禁用各种插件来增强命令行的功能。例如，插件可以提供自动完成、语法高亮、版本控制集成等功能。
> 
> 
> 自动补全：Oh-My-Zsh内置了强大的自动补全功能。当您输入命令时，它会自动提示可能的选项，并根据历史记录和当前上下文进行智能补全。
> 
> 
> 管理插件和主题：通过Oh-My-Zsh，您可以轻松管理已安装的插件和主题。添加新插件或切换主题只需编辑一个简单的配置文件。
> 
> 
> 社区支持：Oh-My-Zsh拥有活跃的社区，用户可以在社区中获得支持、分享配置和学习使用技巧。
> 
> 
> 


总而言之，Oh-My-Zsh是一个强大的工具，使得Zsh shell更加易于使用和定制。它提供了丰富的主题和插件选项，可以大大提升命令行环境的效率和舒适度。


### 😊2. 可用的shell终端


可以通过`cat /etc/shells`查看系统支持的shell终端列表，我的输出如下：



```
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh

```

可以通过`chsh -s /bin/zsh`切换默认的shell。


### 😆3. oh-my-zsh安装与配置


zsh比默认的bash功能更加强大，也更加美观，下面就来安装体验一下。


首先安装zsh：



```
sudo apt-get install zsh

```

安装oh-my-zsh：



```
wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
chmod a+x install.sh
./install.sh

```

![请添加图片描述](https://img-blog.csdnimg.cn/7e9b8b3faa05463eaefa0bb4c126f42b.png)


修改主题：



```
# 首先安装插件
git clone https://ghproxy.com/https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH\_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://ghproxy.com/https://github.com/zsh-users/zsh-autosuggestions ${ZSH\_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# 编辑zsh配置
gedit .zshrc
# 修改以下两行
ZSH\_THEME="ys"
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)	# 高亮和自动补全
# 刷新配置
source .zshrc

```

配置完成后终端如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6570d5fa67a746abb905a90403a44084.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





