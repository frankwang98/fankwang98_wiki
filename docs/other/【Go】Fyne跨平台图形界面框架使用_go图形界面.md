







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Fyne跨平台图形界面框架使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Fyne介绍](#smirk1_Fyne_7)
	+ [:blush:2. 环境安装与配置](#blush2__27)
	+ [:satisfied:3. 应用示例](#satisfied3__58)




### 😏1. Fyne介绍


Fyne是一个用于创建跨平台应用程序的Go语言框架。它提供了简单易用的API和工具，使开发者能够快速构建漂亮、高性能的图形界面应用程序。


官网：`https://fyne.io/`


Github地址：`https://github.com/fyne-io/fyne`


以下是Fyne的一些主要特点和优势：



> 
> 1.跨平台支持：Fyne支持多个操作系统和平台，包括Windows、macOS、Linux以及基于Raspberry Pi等设备。这意味着您可以使用相同的代码库构建适用于不同平台的应用程序。
> 
> 
> 



> 
> 2.简单易用的API：Fyne提供了简洁而直观的API，使得构建用户界面变得非常容易。它采用了声明式布局，您可以使用自定义控件或内置控件来创建界面，并使用现代化的UI风格。
> 
> 
> 



> 
> 3.原生外观和性能：Fyne使用操作系统的本地GUI组件，以确保应用程序在外观和行为上与目标平台保持一致。同时，它还优化了绘制和渲染引擎，以提供快速且流畅的用户体验。
> 
> 
> 



> 
> 4.支持多种输入方式：Fyne支持鼠标、键盘和触摸屏等多种输入方式，使您能够轻松处理各种用户交互。
> 
> 
> 



> 
> 5.适用于嵌入式设备：Fyne也可以用于嵌入式设备，包括基于树莓派的应用程序开发。它的轻量级设计和高性能使其成为在资源受限的环境下构建应用程序的理想选择。
> 
> 
> 


Fyne是一个强大而灵活的跨平台GUI框架，适用于使用Go语言开发图形界面应用程序的开发者。无论您是要构建桌面应用、移动应用还是嵌入式应用，Fyne都可以提供简单、高效和可靠的解决方案。


### 😊2. 环境安装与配置


上一节已经安装好了`go`和`gcc`，下面就安装`fyne`这个跨平台GUI框架，go安装包类似python语言的`pip`。



```
# 设置go get代理
go env -w GO111MODULE=on
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct

# 永久设置
sudo gedit /etc/profile
export GOPROXY=https://goproxy.cn  #代理

# go get安装和更新包（Fyne这里是v1，需要v2记得加v2）
go get fyne.io/fyne
go get -u fyne.io/fyne
go get fyne.io/fyne/v2

# 卸载包（需要手动删除源码和二进制文件，示例如下）
rm -rf $GOPATH/src/example.com/package
rm $GOPATH/bin/package
# 如果没有设置GOPATH，安装的pkg默认是在home下
# 建议go安装时不用用apt装，因为后面许多框架都需要go版本1.17以上

# 设置项目
mkdir hello_go
go mod init hello_go # 初始化
go mod tidy # 安装依赖
touch main.go
go run main.go

```

### 😆3. 应用示例


go最简示例：



```
package main

import (
   "fyne.io/fyne/widget"
   "fyne.io/fyne/app"
)

func main() {
   app := app.New()

   w := app.NewWindow("Hello")
   w.SetContent(widget.NewVBox(
        widget.NewLabel("Hello Fyne!"),
        widget.NewButton("Quit", func() {
            app.Quit()
         }),
    ))

    w.ShowAndRun()
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e2b977913ae444597f523ff76353a55.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





