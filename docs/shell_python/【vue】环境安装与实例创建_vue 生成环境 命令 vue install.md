







> 
> vue是一套用于构建前端界面的框架。
> 
> 
> 




#### 文章目录


* + [1. vue环境安装](#1_vue_3)
	+ [2. 创建项目](#2__18)
	+ - [vue init创建项目](#vue_init_19)
		- [Vite创建项目](#Vite_39)
		- [vue create创建项目](#vue_create_57)
		- [vue ui创建项目](#vue_ui_74)
	+ [3. 打包项目](#3__83)




### 1. vue环境安装


首先安装nodejs并配置npm国内镜像：`https://zhuanlan.zhihu.com/p/442215189`


升级或安装cnpm并查看版本：



```
npm install cnpm -g
cnpm -v

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0c549f111b3474e82f506ef3daa7637.png)


安装vue命令行工具：`cnpm install -g @vue/cli`  
 查看版本：`vue -V`


### 2. 创建项目


#### vue init创建项目


创建项目：`vue init webpack vue3-1`


![在这里插入图片描述](https://img-blog.csdnimg.cn/d593b6ffd3ec415e8edc6f5a038bbcee.png)


创建完成后，进入项目并运行：



```
cd vue3-1
cnpm run dev

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/64599f86acc0456bbc603b22868b1668.png)


然后在浏览器中打开网址：`http://localhost:8080/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/8511c146dae245e3a3cd19b0674e5a36.png)  
 可以使用`vue ui`来打开图形界面创建和管理项目：


![在这里插入图片描述](https://img-blog.csdnimg.cn/033f98cb105246e7bb05980dbec25dcd.png)


#### Vite创建项目


Vite 是一个 web 开发构建工具，由于其原生 ES 模块导入方式，可以实现闪电般的冷服务器启动。


创建项目：`cnpm init vite-app vue3-2`


运行项目：



```
cd vue3-2
cnpm install
cnpm run dev

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8aa704ce67f84a93a7fbb6f4b9d6ce56.png)  
 浏览器中打开`http://localhost:3000/`如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/29bbf13922e64a52a2dccef686dedd67.png)


#### vue create创建项目


创建项目：`vue create vue3-3`


启动项目：



```
cd vue3-3
cnpm run serve

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7687b9b32a9c4048a49239d63a8ff353.png)


浏览器打开`http://localhost:8080/`效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/18a04067d88c4612a26834ead524ddce.png)


#### vue ui创建项目


创建ui：`vue ui`


![在这里插入图片描述](https://img-blog.csdnimg.cn/b50e9bdd3968466b8186a93b311c8d68.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/57cadfa3aae747619e687bf8f95930a3.png)


### 3. 打包项目


打包命令：`cnpm run build`


![在这里插入图片描述](https://img-blog.csdnimg.cn/a8af96ba6f054e90a5b2d36db874e1ed.png)


执行完成后，会在项目目录中生成一个 dist 目录，该目录一般包含 index.html 文件及 static 目录，static 目录包含了静态文件 js、css 以及图片目录 images。（实际部署时只要放入dist即可）


![在这里插入图片描述](https://img-blog.csdnimg.cn/50311322b49444ce9f10f76285877612.png)


在dist中打开index.html可能是空白，这里是因为导入css和js的路径有误，只需把绝对路径改为相对路径即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/1220c177b4ba4801b5288fbe4826c93e.png)


改后的效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/07c6a460bd6b427bb6945e63fe4a89a5.png)


以上。





