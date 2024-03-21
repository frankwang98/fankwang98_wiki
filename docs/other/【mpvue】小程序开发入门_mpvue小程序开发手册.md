







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍mpvue的使用。
> 
> 
> **学其所用，用其所学。——梁启超**
> 
> 
> 欢迎来到我的博客，一起学习知识，共同进步。  
>  🥞喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 小程序如何开发](#smirk1__9)
	+ [:blush:2. mpvue介绍](#blush2_mpvue_19)
	+ [:satisfied:3. mpvue入门案例](#satisfied3_mpvue_30)
	+ - [1.初始化mpvue项目](#1mpvue_35)
		- [2.搭建小程序开发环境](#2_59)
		- [3.调试开发mpvue](#3mpvue_64)




### 😏1. 小程序如何开发


小程序开发的基本步骤：


1. 注册微信公众平台账号并创建小程序
2. 下载安装微信开发者工具，并用微信公众平台账号登录
3. 在开发者工具中创建一个新的小程序项目，并填写相关信息
4. 编写小程序的前端界面代码和后端逻辑代码
5. 在开发者工具中进行调试和测试
6. 将小程序提交审核，并等待审核通过后发布


### 😊2. mpvue介绍


mpvue官网：`http://mpvue.com/`  
 github地址：`https://github.com/Meituan-Dianping/mpvue`


mpvue是一个基于Vue.js的小程序开发框架，由美团点评技术团队开发在2018年3月开源。它允许开发者使用Vue.js语法来开发微信小程序。mpvue框架能够将Vue.js代码编译成小程序原生的WXML模板语言和WXSS样式语言，并能够通过微信提供的API进行调用和处理。


mpvue具有以下特点：


1. 基于Vue.js：开发者可以使用熟悉的Vue.js语法进行开发，同时也能够利用Vue.js强大的组件化、数据绑定等功能。
2. 支持跨端开发：除了小程序之外，mpvue还支持Web和移动端H5的开发，可以极大地提高开发效率。
3. 小巧轻便：mpvue压缩后仅有20K左右，非常适合开发小型应用或在资源受限的环境下使用。
4. 易于上手：mpvue文档详尽、易懂，对于已经掌握Vue.js的开发者来说非常容易上手。


### 😆3. mpvue入门案例


使用mpvue开发小程序需要首先安装好`nodejs`和`vue`。


入门案例：


#### 1.初始化mpvue项目



```
# 1. 安装node并检查
$ node -v
v18.12.1

$ npm -v
5.6.0

# 2. 国内源
$ npm set registry https://registry.npm.taobao.org/

# 3. 全局安装 vue-cli
$ npm install --global vue-cli

# 4. 创建一个基于 mpvue-quickstart 模板的新项目
$ vue init mpvue/mpvue-quickstart my-project

# 5. 安装依赖
$ cd my-project
$ npm install
$ npm run dev

```

#### 2.搭建小程序开发环境


用微信专门的小程序开发者工具，支持Win和Mac。


下载链接：`https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html`


#### 3.调试开发mpvue


将项目目录导入开发者工具，会显示模拟器、编辑器和调试器等模块。


不过一般用开发者工具进行模拟器显示，编辑器可以用自己趁手的，比如`VS Code`。


如果新增了界面，需要重新执行`npm run dev`。


小程序调试界面如下：  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/c66aeb3c0c8343a2a4ef8b2c7529079a.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





