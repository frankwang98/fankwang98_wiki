







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Nodejs与Electron环境配置与示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Nodejs与Electron介绍](#smirk1_NodejsElectron_7)
	+ [:blush:2. 环境安装与配置](#blush2__29)
	+ [:satisfied:3. 应用示例](#satisfied3__42)
	+ [:satisfied:4. 关于技术选型](#satisfied4__80)




### 😏1. Nodejs与Electron介绍


官网：`https://nodejs.org/en/`


`Node.js`是一个用于在服务器端运行JavaScript的运行时环境，用于构建高性能的网络应用程序。



> 
> 1.Node.js是一个基于Chrome V8引擎的JavaScript运行时，用于在服务器端运行JavaScript代码。
> 
> 
> 



> 
> 2.Node.js允许使用JavaScript构建高性能、可扩展的网络应用程序，它提供了许多内置模块和库，简化了服务器端开发。
> 
> 
> 



> 
> 3.Node.js的主要特点是非阻塞I/O模型，使得它非常适合处理高并发的网络请求，同时也支持处理文件系统操作、数据库访问等任务。
> 
> 
> 



> 
> 4.Node.js使用npm（Node Package Manager）作为其包管理器，拥有大量的第三方模块和库，使得开发者可以轻松地扩展和重用代码。
> 
> 
> 


`Electron`是一个跨平台的桌面应用程序框架，使用Web技术构建原生级别的桌面应用程序，也就是将js工程打包成GUI界面程序的框架。Linux 操作系统的桌面平台 Skype 就是在 Electron 框架上创建的。



> 
> 1.Electron是一个开源的框架，用于构建跨平台的桌面应用程序，它使用Web技术（HTML、CSS和JavaScript）来构建应用程序界面。
> 
> 
> 



> 
> 2.Electron基于Chromium（用于Google Chrome的开源项目）和Node.js，使得开发者可以使用Web技术构建功能丰富、原生级别的桌面应用程序。
> 
> 
> 



> 
> 3.Electron提供了一个主进程（使用Node.js）和多个渲染进程（使用Chromium），使得开发者可以使用JavaScript控制整个应用程序的生命周期、访问底层系统资源，并在渲染进程中构建应用程序界面。
> 
> 
> 



> 
> 4.Electron被广泛应用于构建桌面应用程序，包括代码编辑器、聊天应用、音乐播放器等。
> 
> 
> 


### 😊2. 环境安装与配置


Nodejs安装：



```
# 从官网下载LTS稳定版
node -v # 查看版本

npm config set registry https://registry.npmmirror.com # 配置全局国内源
npm install -g cnpm --registry=https://registry.npmmirror.com # 或者用cnpm替换npm

node server.js # 执行程序

```

### 😆3. 应用示例


用Nodejs创建helloworld服务端示例，由三部分组成：


1. 引入 required 模块：可以使用 require 指令来载入 Node.js 模块。
2. 创建服务器：服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
3. 接收请求与响应请求：服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。


创建`server.js`：



```
var http = require('http');

http.createServer(function (request, response) {

    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');

```

Electron程序示例：



```
git clone https://github.com/electron/electron-quick-start
cd electron-quick-start
npm install
npm start

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/40dbd34f3e1d4e948ff00e6077b367ac.png)


### 😆4. 关于技术选型


源自：`https://juejin.cn/post/7102818131780845599`


关于聊天/团队协作软件的技术选择一般有几种：


1. Electron  
 就桌面端而言，常见的跨平台开发技术有现在比较火的Electron基于前端技术实现的，可以同时运行在Windows，Mac以及Linux。在如今2022年这个节点选择Electron作为桌面端开发的公司也越来越多。早期的飞书也是基于Electron开发的。而技术热度方面，从掘金上越来越多的Electron话题更新来看看，也可以看出，这项技术受到越来越多的欢迎。
2. Flutter  
 而之前版本Flutter在移动端方面有非常好的成绩，在Flutter3.0之后，已经可以稳定在Windows,Mac以及Linux上运行，而且也成为很多新项目很不错的技术选型。
3. Qt  
 Qt则属于比较老牌的跨平台开发技术，像国外的即时通讯软件Telegram就是使用Qt进行开发，我们在github上也可以看到其完整开源的代码。我曾经有段时间尝试去阅读相关源码，不过最后还是放弃了光先把代码拍起来，没有半个星期以上是很难搞定的。


国内大厂都是用哪个？


* 钉钉，在PC端没有选用跨端技术，至少在UI层面我看到的是这样的。钉钉在Windows下使用的duilib+cef的方案，而Mac则使用的是原生开发，在Linux上则是最近两年用Qt重新开发的。
* 飞书从一开始的Electron+Rust到后期也是使用Chromium+Rust的技术进行开发，很好实现Windows，Mac，Linux以及网页版的多端统一。
* 企业微信Windows和Mac上的技术选型和钉钉是一样的，不过Linux客户端貌似还没有。


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





