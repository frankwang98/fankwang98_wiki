







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Express.js环境配置与示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 知识介绍](#smirk1__7)
	+ [:blush:2. 环境安装与配置](#blush2__23)
	+ [:satisfied:3. 应用示例](#satisfied3__39)




### 😏1. 知识介绍


官网：`https://expressjs.com/`


`Express.js`是一个简洁而灵活的Node.js Web应用程序框架，它提供了一组简单、易于使用的工具和中间件，用于帮助构建Web应用程序和API。Express.js是目前最受欢迎的Node.js框架之一，被广泛用于构建各种类型的Web应用程序，包括单页应用、多页应用、RESTful API和后端服务等。


以下是`Express.js`的一些主要特点和优势：



> 
> 1.简单易用：Express.js采用了简洁的API设计，使得构建Web应用程序变得非常简单。它提供了一组核心功能，例如路由、中间件、请求处理和响应处理等，使开发人员能够轻松地构建路由和处理HTTP请求。
> 
> 
> 



> 
> 2.中间件支持：Express.js的核心特性是中间件机制，它允许开发人员在请求和响应之间插入功能模块。你可以使用内置的中间件或编写自定义的中间件来处理身份验证、日志记录、错误处理、静态文件服务等。这种灵活的中间件机制使得构建复杂的应用程序变得更加容易。
> 
> 
> 



> 
> 3.路由功能：Express.js提供了简单而灵活的路由功能，可以根据URL路径和HTTP方法将请求映射到相应的处理函数。这使得创建和管理多个路由变得非常简单，可以轻松处理各种请求和路由规则。
> 
> 
> 



> 
> 4.快速而高效：Express.js是一个轻量级框架，它在性能和响应速度方面表现出色。由于它是构建在Node.js的事件驱动、非阻塞I/O模型上，因此能够处理大量并发请求，提供高效的性能。
> 
> 
> 



> 
> 5.强大的扩展性：Express.js拥有庞大的生态系统和活跃的社区支持，提供了许多插件和中间件，可以轻松扩展和定制应用程序的功能。从身份验证、数据库集成到模板引擎和API工具，你可以从丰富的第三方扩展中选择适合你的需求。
> 
> 
> 


### 😊2. 环境安装与配置



```
node -v
npm -v
npm init # 初始化项目
npm install express
node app.js # 运行程序

```

此外，也可用`express-generator`直接生成express的模板。



```
npx express-generator # 安装
npm install
npm start

```

### 😆3. 应用示例


创建app.js，程序示例：



```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, Express!');
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





