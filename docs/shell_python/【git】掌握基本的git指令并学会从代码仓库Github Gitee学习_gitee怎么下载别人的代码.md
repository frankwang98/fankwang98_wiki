








#### 文章目录


* + [git常用命令](#git_1)
	+ [从代码仓库学习](#_81)
	+ [代理下载/同步Github资源](#Github_110)
	+ [其他学习资源](#_133)




### git常用命令


这里总结了一些我经常使用的git命令：


1. 配置全局名称和邮箱



```
git config user.name "xxx"
git config user.email "xxx@qq.com"

```

注：也可单独配置工程的用户信息


2. 克隆和切换分支



```
git clone https://gitcode.net/qq_40344790/test.git（克隆工程）
git branch -a（显示所有分支）
git checkout develop（切换develop分支）
git checkout -b main（若分支不存在，则自动创建main分支）
git branch -d main（删除main分支）

```

3. 上传和推送代码



```
git add .（添加代码修改到本地）
git commit -m "code:update src"（给要推送的代码命名）
git remote add origin https://gitcode.net/qq_xxx/test.git（本地和远程间建立联系）
git remote rm origin / git remote rename origin old-origin（若提示origin已存在，先删除再重新执行上条）
git push -u origin develop（推送到develop分支）
git push（若前述已全部设置好，直接push上传即可）

```

4. 添加ssh密钥（RSA加密算法的应用）



```
cd .ssh（mkdir .ssh如果没有先创建.ssh文件夹）
ssh-keygen -t rsa -C "xxx@qq.com"（生成密钥）
id_rsa和id_rsa.pub（生成这两个文件）
cat id_rsa.pub（查看公钥并添加到gitlab/github/gitee）

```

5. 其他操作



```
git status 查看仓库变更状态
git diff 比较暂存区和工作区差异
git reset 回退版本
git rm 将文件从暂存区和工作区中删除
git mv 移除或重命名工作区文件
git log 查看历史提交记录（git reflog）
git fetch 从远程获取代码库
git pull 下载远程代码并合并（=fetch+merge）
git push 上传远程代码并合并

```

另外，如果想清空仓库重新开始一段提交，github没有清空仓库的选项（gitee有），可以用以下命令来实现：



```
# 删除主分支main的提交记录
# 切换到一个脱离主分支的另外一条全新主分支，随便一个名字，后面还会改
git checkout --orphan latest_branch
# 暂存所有改动过的文件，内容为当前旧分支的所有文件
git add -A
# 提交更改
git commit -am "init"
# 删除原始主分支
git branch -D main
# 将当前分支重命名为 main（或master）
git branch -m main
# 最后，强制更新存储库
git push -f origin main

```

此外，还有一个常见的换行符问题，默认情况下，在Windows换行符为`CRLF`，也就是\r\n，在Linux是`LF`，也就是\n，所以一般在Linux开发不会有这个问题，但如果在Windows下， 需要设置一下用户配置默认换行符为LF，而且也要设置git不论上传还是下载的都是LF，如下：



```
git config --global core.autocrlf input
git config --global core.eol lf

```

### 从代码仓库学习


GitHub的访问时好时不好，如果有需要的资源或许可以在Gitee上找到。


github官网：`https://github.com/`  
 gitee官网：`https://gitee.com/`


在GitHub/Gitee上下载代码库有两种方式，即**Download zip / Clone。**


第一种方式需要在网页上到达那个界面，假如我们不想打开浏览器就想直接下载到本地计算机上，就需要用第二种方式了。*（已知代码库地址，比如我们要下载某本书的配套资源，一般作者会把Url贴在书的前言。）*


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430163106361.png#pic_center)


**操作如下：**


在本地计算机的某个文件夹中（会clone到这里），点击鼠标右键选择 “GIt Bash Here”


命令行窗口，输入命令 ：`git clone URL`（把URL换成上图复制的地址）


例：`git clone https://github.com/PacktPublishing/Tkinter-GUI-Programming-by-Example`


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430163058289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 等一会，出现100%，done，则代码克隆完成（`gitee`）。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430163228694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 在本地会默认生成存储该代码库的文件夹。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430163257933.png#pic_center)  
 以上就是用git工具快速下载代码库的操作了。


### 代理下载/同步Github资源


我们是否有一种苦恼，Windows电脑端上不去Github，圈圈一直转，git clone工具也失了色彩，那么是否有一种方法能让我们纵览github不受限制呢，今天，福音来了。


第一种方式：可以用代理来下载github资源，参考`https://mirror.ghproxy.com/`


第二种方式：可以将github的仓库同步到gitee。


1. 首先，你要有一个gitee账号，点击新建仓库；  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623152038999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)
2. 很贴心，在其他网站的仓库，可以在此导入；


![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062315212446.png)


3. 输入要导入的github仓库地址，点击导入；  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623152812955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)
4. 等待拉取完成，根据项目大小，时间有所差异；


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210623152857485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)


5. 然后就可以在gitee上看自己的github仓库了，git clone工具也可以使用了（速度棒棒哒）。


### 其他学习资源


廖雪峰的官方网站学git真是太好了：  
 [贴个搭建git服务器的链接，不过现在一个人用不起来，先码住！](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)  
 [菜鸟的git教程](https://www.runoob.com/git/git-tutorial.html)


以上。





