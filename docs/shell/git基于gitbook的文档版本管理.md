






在我们的学习工作中，常常会遇到文档不好管理，修改不好回溯等问题，基于gitbook的文档版本管理就能解决这一问题，gitbook可以很方便的为我们管理方案或知识文档，类似云笔记。


![在这里插入图片描述](https://img-blog.csdnimg.cn/87e2ebcaef5c4821970c78067ba9cb4c.png)




#### 文章目录


* + [1. 环境配置](#1__6)
	+ [2. 初始化book](#2_book_24)
	+ [3. 本地预览](#3__35)
	+ [4. 输出](#4__44)
	+ [5. 发布](#5__58)




### 1. 环境配置


gitbook可在线或离线编辑，在线即在网页端，离线的话需要配置环境。


gitbook地址：`https://www.gitbook.com/`


安装Nodejs：`https://nodejs.org/en`


安装完成后，输入`node -v`查看版本号：


![在这里插入图片描述](https://img-blog.csdnimg.cn/0165fe79207c4568a4ad555641fd3268.png)


安装gitbook-cli脚手架：`npm install -g gitbook-cli`


安装完成后，输入`gitbook -V`查看版本号。


如果出现错误，可能是node版本的问题：`http://www.ushinian.cn/archives/54`


gitbook类似我们的云笔记，也是基于Markdown语法编辑的，编辑器的话，大家可以选择`Typora、VSCode、SublimeText`等，自行下载即可。


### 2. 初始化book


创建文件夹如：mybook


初始化：`gitbook init`


初始化完成后，默认会生成：`SUMMARY.md、README.md`


![在这里插入图片描述](https://img-blog.csdnimg.cn/acd743a488db4360b94e0357c6bfbce3.png)


README类似于mybook的简介部分，而SUMMARY则相当于mybook的目录。


### 3. 本地预览


本地目录初始化完成后，先本地生成预览：



```
每次修改文章目录后，通过执行 gitbook init 自动生成对应的文件
gitbook serve
http://localhost:4000	//浏览器输入

```

### 4. 输出


gitbook支持导出如下格式：



```
HTML格式：本地生成的_book目录
PDF 格式：安装相关包

```

还可用这条命令打包html到指定目录：`gitbook build ./{book_name} --output=./{outputFolde}`


要打包pdf需要安装包：`npm install gitbook-pdf -g`


然后执行命令打包pdf：`gitbook pdf .`


### 5. 发布


可以将gitbook通过`github pages`发布出去，这样所有人都可以看到这个在线书籍/笔记了。


创建github仓库如mybook，并创建`gh-pages`分支，有了这个分支，会自动创建`http://USERNAME.github.io/mybook`。


然后同步本地仓库到这个分支即可。


以上。





