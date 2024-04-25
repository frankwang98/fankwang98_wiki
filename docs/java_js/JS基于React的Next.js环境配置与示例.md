







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍基于React的Next.js环境配置与示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Next.js介绍](#smirk1_Nextjs_7)
	+ [:blush:2. 环境安装与配置](#blush2__31)
	+ [:satisfied:3. 应用示例](#satisfied3__50)
	+ - [添加主页](#_51)
		- [添加页面和导航栏](#_64)




### 😏1. Next.js介绍


官网：`https://nextjs.org/`


`Next.js` 是一个基于 React 的轻量级框架，用于构建现代化的、可扩展的 Web 应用程序。它提供了一种简单而强大的方式来开发服务器渲染 (Server-side Rendering, SSR) 和静态网站生成 (Static Site Generation, SSG) 的 React 应用程序。


下面是一些 `Next.js` 的主要特点和功能：



> 
> 1.服务器渲染 (SSR) 和静态网站生成 (SSG)：Next.js 提供了内置的 SSR 和 SSG 功能，使你可以在服务器端预渲染页面，从而提供更快的加载速度和更好的 SEO。
> 
> 
> 



> 
> 2.基于页面的路由：Next.js 采用基于页面的路由系统，通过文件系统来自动生成路由，使得创建和管理页面变得简单直观。
> 
> 
> 



> 
> 3.热模块替换 (Hot Module Replacement, HMR)：Next.js 支持热模块替换，在开发过程中实时更新代码，无需刷新页面，提高开发效率。
> 
> 
> 



> 
> 4.集成开发环境 (IDE) 支持：Next.js 提供了与 Visual Studio Code (VS Code) 和 JetBrains WebStorm 等常见 IDE 的集成，包括自动完成、调试和代码质量工具等。
> 
> 
> 



> 
> 5.CSS 模块和样式支持：Next.js 内置了对 CSS 模块的支持，可以轻松管理组件的样式，并且支持 Sass、Less 和 CSS-in-JS 等不同的样式解决方案。
> 
> 
> 



> 
> 6.自动代码拆分：Next.js 可以自动将页面和组件拆分成小块，按需加载，从而提高页面加载性能和用户体验。
> 
> 
> 



> 
> 7.强大的插件系统：Next.js 提供了丰富的插件系统，使你可以轻松扩展和定制项目的功能和配置。
> 
> 
> 



> 
> 8.完整的生命周期和数据获取控制：Next.js 提供了完整的生命周期方法和数据获取钩子，以便在服务器端和客户端渲染之间管理数据的获取和处理。
> 
> 
> 


`Next.js` 的目标是简化 `React` 应用程序的开发过程，并提供最佳实践和现代化的开发体验。它适用于从小型应用程序到大型企业级应用程序的各种项目。


### 😊2. 环境安装与配置



```
npm init -y # 初始化
npm install next react react-dom # 安装模块

```

在package.json添加字段：



```
"scripts": {
   "dev": "next",
   "build": "next build",
   "start": "next start"
 }

```


```
npm run dev # 本地运行 3000端口

```

### 😆3. 应用示例


#### 添加主页


`pages`是Next.js默认的网页路径，其中的`index.js`就代表整个网站的主页。创建`pages/index.js`组件：



```
function Home() {
  return <h1>Hello, Next.js!</h1>;
}

export default Home;

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e3fe86103b8f4eb282156347b4df6c92.png)


#### 添加页面和导航栏


创建`pages/about.js`：



```
function About() {
    return <h1>About Page</h1>;
}

export default About;

```

修改`index.js`如下：



```
import Link from 'next/link';

function Navigation() {
  return (
    <nav>
      <ul>
        <li>
          <Link href="/">Home</Link>
        </li>
        <li>
          <Link href="/about">About</Link>
        </li>
      </ul>
    </nav>
  );
}

export default function Home() {
  return (
    <div>
      <Navigation />
      <h1>Hello, Next.js!</h1>
    </div>
  );
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/7fd28e613d5d466c9e1732e352a3a5a7.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





