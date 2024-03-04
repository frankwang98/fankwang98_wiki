







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍gitlab本地服务器的搭建。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. GitLab介绍](#smirk1_GitLab_7)
	+ [:blush:2. 环境配置](#blush2__13)
	+ [:satisfied:3. GitLab使用](#satisfied3_GitLab_61)




### 😏1. GitLab介绍


`GitLab`是一个基于Git仓库管理的Web平台，提供了一些用于软件开发的工具。它包含从项目计划到代码审查、测试和部署的所有功能。


`GitLab`可以是自托管的，也可以在GitLab公司的服务器上进行托管。它提供了许多功能，如源代码管理、问题跟踪、持续集成、Wiki和代码审查等。这些功能使得GitLab成为一个非常强大的工具，特别是对于团队协作开发。


`GitLab`还提供了丰富的API，使得它可以与其他工具集成，例如`JIRA`、`Slack`和`CI/CD`工具等。此外，GitLab还支持`Docker`镜像管理和`Kubernetes`集群管理等最新技术。


### 😊2. 环境配置


以ubuntu18安装为例：


安装依赖包：`sudo apt-get install curl openssh-server ca-certificates postfix`（postfix配置选择Internet不带Smarthost的）


添加公钥：`curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null`


编辑配置文件：



```
sudo nano /etc/apt/sources.list.d/gitlab-ce.list
deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu bionic main

```

环境配置脚本：



```
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
sudo bash script.deb.sh

```

然后安装gitlab-ce：



```
sudo apt-get update
sudo apt-get install gitlab-ce

```

安装完成后如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/952263dea0f848a4aeb550d2499a9a81.png)


启动各项服务：



```
service sshd start
service postfix start
sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT	# 配置防火墙
sudo gitlab-ctl reconfigure
sudo gitlab-ctl status	# 检查gitlab是否运行，下面则表示正常

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/2a4890421b044f20bca3b6079c6fa9f6.png)


打开浏览器本地界面进行相关配置即可：`http://localhost/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/0ed2ae2e8236499cabf85311ca67f585.png)


### 😆3. GitLab使用


本地搭建后，就可以使用自己的代码托管私服了。


也可以修改相关参数：



```
sudo gedit /etc/gitlab/gitlab.rb
external_url 'http://192.168.1.100:9092'  # 本机局域网ip地址为192.168.1.100，设置端口为9092
sudo gitlab-ctl reconfigure # 重新配置
sudo gitlab-ctl status	# 查看 GitLab 状态

sudo systemctl enable gitlab-runsvdir.service # 开机自启
sudo systemctl disable gitlab-runsvdir.service # 取消自启

```

设置管理员密码：



```
cd /opt/gitlab/bin
sudo gitlab-rails console
u=User.where(id:1).first
user.password='086530Qwe'
user.save!
exit # 设置完就可以登陆了

```

使用方面，跟`github`和`gitee`类似，不过功能更加强大，适合团队或公司内部搭建使用。


参考：



```
https://www.cnblogs.com/zzuuoo666/p/12597498.html#1.%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96%E5%8C%85
http://t.csdnimg.cn/PW7et
https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/
https://www.cnblogs.com/Mr-Ding/p/17602014.html

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





