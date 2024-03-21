







> 
> 命令行操作确实很方便快捷，但图形化工具看起来更直观，在git仓库管理中也是这样。
> 
> 
> 


这一节来介绍使用git图形化管理工具`SourceTree`。


### 1. 安装


地址：`https://www.sourcetreeapp.com/`


目前还只支持Windows和Mac OS。


![在这里插入图片描述](https://img-blog.csdnimg.cn/68496ccb8019490988bc33221fe91bef.png)


### 2. 使用


在我们掌握git命令行的提交和分支管理后，再使用GUI工具，会发现更高效。


#### 添加仓库



```
git config user.name
git config user.email

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/cc13c5d57e41420788a2e58eac009bbb.png)


#### 提交



```
git add .
git commit -m "update"

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b274cc1f23b4e89a81c7cc24feec698.png)


#### 分支



```
git branch -a
git checkout xxx
git merge

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/062f919e0ffd4e09a45c03c0b7d93a3d.png)


#### 推送



```
git pull
git push

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e92ceb2a78a4aedba5e5340b38aa144.png)


### 3. 小结


SourceTree GUI工具对于常用的提交、分支、推送等操作来说非常方便，但也保留了终端工具，是一款git管理利器。


参考：



```
https://www.liaoxuefeng.com/wiki/896043488029600/1317161920364578
https://www.runoob.com/git/source-tree-intro.html

```

以上。





