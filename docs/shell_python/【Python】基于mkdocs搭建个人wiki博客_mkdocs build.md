






基于Python的mkdocs也可以搭建个人博客，且自带搜索和ico。


贴一下我的博客：`https://frankwang98.gitee.io/wiki`


![在这里插入图片描述](https://img-blog.csdnimg.cn/d8d15d713d26486998d254dc1ff56279.png)




#### 文章目录


* + [1. Mkdocs介绍](#1_Mkdocs_7)
	+ [2. 环境搭建](#2__10)
	+ - [Windows](#Windows_13)
		- [Linux](#Linux_42)
	+ [3. mkdocs创建博客示例](#3_mkdocs_51)
	+ [4. 博客发布到gitee/github](#4_giteegithub_88)
	+ [5. 主题与其他](#5__95)




### 1. Mkdocs介绍


MkDocs是一个快速、简单的静态网站生成器，适用于构建项目文档。源文件以 Markdown 格式编写，并使用单个 YAML 配置文件进行配置。


### 2. 环境搭建


环境包括python、pip，在此基础上安装mkdocs。


#### Windows


python下载地址：`https://mirrors.huaweicloud.com/python/3.6.8/`


pip下载地址：`https://pypi.org/project/pip/#downloads`


pip解压后安装：



```
python setup.py install
pip -V	检查版本号

```

python国内源地址：



```
[global]
index-url = http://pypi.douban.com/simple/
[install]
trusted-host = pypi.douban.com

```

mkdocs安装：



```
pip install mkdocs
mkdocs --version

```

#### Linux


安装：



```
sudo apt install mkdocs
mkdocs --version
pip install mkdocs-awesome-pages-plugin pymdown-extensions pygments python-markdown-math

```

### 3. mkdocs创建博客示例


新建博客：



```
mkdocs new blog
cd blog
mkdocs serve	启动服务
http://127.0.0.1:8000	本地生成

```

修改配置：



```
gedit mkdocs.yml,修改theme为readthedocs
nav导航文件

```

站点生成：



```
mkdocs build	博客编译后，生成site文件夹
ls site
echo site/ >> .gitignore 如果你使用 git 等版本控制系统, 你可能不希望提交构建之后的文档到版本库. 在 .gitignore 中添加 site/ 即可忽略该目录.
mkdocs build --clean  一段时间后, 可能有文件被从源码中移除了, 但是相关的文档仍残留在 site 目录中. 在构建命令中添加 --clean 参数即可移除这些文档.

```

站点部署：



```
mkdocs gh-deploy  (这个分支放生成的站点)
master  （这个分支放md）

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e6686aaaf588433a9dbdaf5b05a61f92.png)


### 4. 博客发布到gitee/github


注册gitee或github账号，创建仓库如wiki。


点击服务，开启Gitee Pages。


![在这里插入图片描述](https://img-blog.csdnimg.cn/11600e41571b45628d1f68810787b665.png)


### 5. 主题与其他


配置教程：`http://t.csdn.cn/DZ4Cy`


中文教程：`https://markdown-docs-zh.readthedocs.io/zh_CN/latest/`


第三方主题：`https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes`


![在这里插入图片描述](https://img-blog.csdnimg.cn/25ce5f16a5524b87afc12a97fdcd0f8b.png)


如配置materials主题：



```
pip install mkdocs-material
在配置文件中修改：
theme:
  name: material

```

以上。





