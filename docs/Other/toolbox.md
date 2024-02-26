## github代理下载
git clone： `git clone https://ghproxy.com/` 

wget & curl下载：

`wget https://ghproxy.com/`

`curl -O https://ghproxy.com/`

## pip镜像下载
临时：`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple xxx`  

永久：

windows系统，在user目录中创建一个pip目录，如C:\Users\xxx\pip，新建文件pip.ini,内容如下：

```shell
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

内容如下：

```shell
[global]
index-url = http://pypi.douban.com/simple/
[install]
trusted-host = pypi.douban.com
```

## conda镜像下载
可以直接修改配置文件.condarc的方式来完成下载源的修改。
Linux系统中.condarc文件的位置在/home/用户名/下，Windows系统中.condarc文件的位置在C:\users\用户名下。
内容如下：
```shell
channels:
  - https://mirrors.aliyun.com/anaconda/pkgs/free/
  - https://mirrors.aliyun.com/anaconda/pkgs/main/
  - defaults
ssl_verify: true
show_channel_urls: true
```
查看修改是否成功，打开Anaconda Prompt，输入`conda config --show channels`查看。

## ubuntu主题更改
在Ubuntu和Linux Mint中使用以下PPA安装[Flat Remix Gnome](https://www.gnome-look.org/p/1013030/)主题。
```
sudo add-apt-repository ppa:daniruiz/flat-remix
sudo apt-get update
sudo apt-get install flat-remix-gnome
```
安装其他自定义主题到`~/.themes`，然后用tweak-tool来自定义设置。

自定义主题有：[Matcha](https://github.com/vinceliuice/Matcha-gtk-theme)

安装tweak工具和user theme来设置主题：
```
sudo apt install gnome-tweak-tool
sudo apt install gnome-shell-extensions
```
不过后来发现，还是原生的主题耐看，就没再折腾了。

## git环境配置
1. 打开终端，配置用户名和邮箱
```
git config --global user.name “username”
git config --global user.email “useremail@qq.com“
git config -l
```

2. 生成ssh公钥（ssh访问大文件时更稳定）
```
ssh-keygen -t ed25519 -C "xxxxx@xxxxx.com"  
cat ~/.ssh/id_ed25519.pub
复制生成后的 ssh key，通过仓库主页 「管理」->「部署公钥管理」->「添加部署公钥」 ，添加生成的 public key 添加到仓库中。
终端验证：ssh -T git@gitee.com
```

3. 创建项目
```
mkdir wiki
cd wiki
git init
```

4. 上传项目
```
git add .
git commit -m “消息”
git status
git remote add origin https://gitee.com/frankwang98/wiki.git  ( git@gitee.com:frankwang98/wiki.git ) 
git push -u origin master (下次可以直接git push)
git check (切换分支)
```

## 基于Mkdocs的Wiki静态博客搭建
1. Mkdocs安装
```
sudo apt install mkdocs
mkdocs --version
pip3 install mkdocs-awesome-pages-plugin pymdown-extensions pygments python-markdown-math
```

2. 创建项目
```
mkdocs new wiki
cd wiki
mkdocs serve
http://127.0.0.1:8000/
```

3. 修改配置文件
```
gedit mkdocs.yml,修改theme为readthedocs(不要用gedit)
nav导航文件
```

4. 站点生成
```
mkdocs build
ls site
echo "site/" >> .gitignore 如果你使用 git 等版本控制系统, 你可能不希望提交构建之后的文档到版本库. 在 .gitignore 中添加 site/ 即可忽略该目录.
mkdocs build --clean  一段时间后, 可能有文件被从源码中移除了, 但是相关的文档仍残留在 site 目录中. 在构建命令中添加 --clean 参数即可移除这些文档.
```

5. 站点部署
```
mkdocs gh-deploy  (这个分支放生成的站点)
master  （这个分支放md）
```

6. 帮助
```
mkdocs --help
mkdocs build --help
```

## 基于hexo的gitee静态博客搭建
1. hexo下载
```
hexo下载地址：国外站-https://nodejs.org/en/  国内源-https://mirrors.huaweicloud.com/nodejs/v16.14.2/
下载好后，tar -xf XXX.tar.xz解压缩
进入node文件夹的bin目录下，建立索引
	sudo ln -s /home/XXX/node/bin/npm /usr/local/bin/
	sudo ln -s /home/XXX/node/bin/node /usr/local/bin/
设置成功后，检查版本号 node -v ; npm -v
设置国内源：npm config set registry https://registry.npm.taobao.org
```

2. hexo安装
```
安装hexo客户端：npm install -g hexo-cli

设置索引：sudo ln -s /home/XXX/node/lib/node_modules/hexo-cli/bin/hexo /usr/local/bin/

设置成功后，检查版本号：hexo -v
```

3. 添加github访问代理
```
sudo gedit /etc/hosts
185.199.108.133 raw.githubusercontent.com
```

4. 初始化hexo
```
mkdir blog				创建一个文件夹用来放博客文件
cd blog && hexo init .	进入blog并初始化
npm install				初始化组件
hexo s					本地测试，进入http://localhost:4000
```

5. 写一篇自己的博客 
```
hexo n "XXX"	写一篇自己的博客

cd source/_posts/	创建的新博客存放在这里了，支持Markdown语法写作 

hexo cl && hexo g && hexo s	清除缓存，生成博客，本地查看
```

6. 将博客挂到gitee Pages服务上
```
安装git：sudo apt-get install git

初始化blog文件夹：git init

在blog目录下安装部署gitee的插件：npm install --save hexo-deployer-git

对blog下的_config.yml文件进行配置修改：gedit _config.yml，修改最后的deploy为git地址

hexo d	远程推送

在gitee repo中的pages服务下进行部署更新
```

## Windows设置IP与DNS（交互界面和CMD命令行）
1. 打开命令行
Windows+R，输入cmd进入命令行，常用的设置网络命令如下：

2. 设置IP
```
设置自动获取IP地址（DHCP）——netsh interface ip set address name="本地连接" source=dhcp

设置固定IP——netsh interface ip set address name="本地连接" source=static addr=192.168.0.3 mask=255.255.255.0 gateway=192.168.0.1 gwmetric=auto（gateway和gwmetric默认不设置也可）
```

3. 设置DNS
```
自动获取DNS——netsh interface ip set dns name="本地连接" source=dhcp

手动设置单个DNS 例218.85.157.99——netsh interface ip set dns name="本地连接" source=static addr=218.85.157.99 register=primary

需要多加个备用DNS 例202.101.98.55——netsh interface ip add dns name="本地连接" source=static addr=202.101.98.55 index=2
```

## Windows办公+开发工具链整理
### 办公
- TIM：简约版QQ
- 微信：国民级通讯工具
- 钉钉：阿里系办公软件
- 腾讯会议：单纯的开会用软件，也有微信小程序，毕竟是自家生态
- WPS：国产Word、Excel、PPT全家桶
- 幕布：思维导图
- Typora：Markdown文档工具
### 工具
- Bandzip：强大的压缩与解压缩工具
- Potplayer：强大的音视频播放工具
- Snipaste：截图与贴图
- OBS Studio：屏幕录制
- Windows Terminal：微软的终端工具
### 开发
- WSL2：适用于 Linux 的 Windows 子系统，现在普遍是2版本，可在此基础上安装ubuntu、kali等linux系统
- Docker：依赖Linux内核的通用容器工具
- Vmware Workstation：虚拟机工具
- Matlab：科学开发语言工具
- STM32：嵌入式电控开发工具
- CANoe：CAN通讯仿真与测试
- Visual Studio：微软软件开发工具
- Qt：基于C++的界面开发
- ROS：机器人操作系统
- Pycharm：Python集成开发工具

## VScode自动生成Markdown目录
安装Markdown ALL插件,Ctrl+shift+P打开命令面板,Generate TOC即可.
记得写Markdown时不要加序号了,不然会重复.
也可以修改配置来取消自动编号!