








#### 文章目录


* + [1. 环境安装](#1__1)
	+ [2. 一个WebGIS案例欣赏](#2_WebGIS_4)
	+ [3. GIS开发基础](#3_GIS_24)
	+ [4. 开发环境搭建](#4__54)
	+ [5. 调用API进行地图显示](#5_API_68)
	+ [6. 实时路况图层](#6__93)
	+ [7. 地图控件](#7__108)




### 1. 环境安装


安装nodejs和vue环境：`http://t.csdn.cn/er8B7`


### 2. 一个WebGIS案例欣赏


克隆大佬的项目并运行：



```
git clone https://github.com/zhengjie9510/webgis-demo.git
cd webgis-demo
npm install
npm run serve


```

打包项目：



```
npm run build
npm run lint

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/19c24d61769b4c358e09e436bb2ac89d.png)


### 3. GIS开发基础


学习地址： `https://www.bilibili.com/video/BV1Ui4y1U7c6/?p=26&share_source=copy_web&vd_source=c64d57391b4f01119d930e79fb0b819b`


GIS开发方向：


1. 桌面端开发（C/S）
2. web端开发（B/S，云GIS，跨平台）
3. 移动端开发（高德地图、美团外卖等）


学习方法：**整体性学习-建立联系-独立思考-强化学习**。


学习心态：**结果型心态、过程型心态**。


WebGIS的本质：`如何将地理信息通过web技术展现出来`。


学习路径：


1. WebGIS是web技术与gis技术的结合
2. 首先，了解web基础知识（HTML、CSS、JS）
3. 然后，学习前端工程化，了解常用的前端框架（vue、react）
4. 中间做几个练手小项目（熟悉开发流程）
5. 然后，进阶学习GIS相关的框架（二维openlayers、三维cesium）
6. 最后，深入学习，在实践中成长


地图的组成：


* 底图（Map）：信息的载体
* 图层（Layer）：不同地理信息的分类集合
* 要素（Feature）：不同的地理元素
* 几何（Geometry）：信息的数据模型和抽象


![在这里插入图片描述](https://img-blog.csdnimg.cn/79272b2aa8b24da8a803c6b84e21716b.png)


### 4. 开发环境搭建


安装：


* 开发软件：VSCode（live server插件实现网页热更新）
* 测试环境：chrome


![在这里插入图片描述](https://img-blog.csdnimg.cn/df521a96c52943e3bc2eff90728e6a98.png)


高德API：


* 注册个人开发者
* 创建应用


![在这里插入图片描述](https://img-blog.csdnimg.cn/b01102e9a7e4416b860ba60612037175.png)


### 5. 调用API进行地图显示


开发文档：`https://lbs.amap.com/api/jsapi-v2/summary/`


官方文档是最好的教程。


![在这里插入图片描述](https://img-blog.csdnimg.cn/08349a0eaac44245b978960b65767d09.png)


步骤如下：


1. 引入资源文件
2. 创建地图容器
3. 设置地图样式
4. 加载地图


地图显示效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c16291ffa5ad4bddb607305fa56b8aa0.png)


通过设置相关的地图参数如下：



```
https://lbs.amap.com/api/jsapi-v2/guide/map/lifecycle

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1982a1347553402cab24b1157d38ce36.png)


### 6. 实时路况图层



```
        var traffic = new AMap.TileLayer.Traffic({
            'autoRefresh': true,     //是否自动刷新，默认为false
            'interval': 180,         //刷新间隔，默认180s
        });

        map.add(traffic); //通过add方法添加图层

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c4fa76d5398844f2aeef72ec3c5cc14b.png)


### 7. 地图控件


…


以上。





