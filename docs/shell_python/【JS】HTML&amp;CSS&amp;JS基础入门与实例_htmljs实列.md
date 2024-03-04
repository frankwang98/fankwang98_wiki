








#### 文章目录


* + [HTML](#HTML_1)
	+ [CSS](#CSS_27)
	+ [JS](#JS_62)




### HTML


HTML是一种用于创建网页的标准标记语言。


学习参考：`https://www.runoob.com/html/html-tutorial.html`


一个最基础的HTML实例：



```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Frank的个人博客(frank)</title>
</head>
<body>

<h1>我的第一个标题</h1>
<p>我的第一个段落。</p>

</body>
</html>

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/106c3db005bc41acbb2a20b7f4e45865.png)


### CSS


CSS是一种用来为结构化文档（如 HTML 文档或 XML 应用）添加样式（字体、间距和颜色等）的计算机语言，CSS 文件扩展名为 .css。


CSS有两种常用导入方式：导入式和链接式。


导入式实例：



```
body {
    background-color:#d0e4fe;
}
h1 {
    color:orange;
    text-align:center;
}
p {
    font-family:"Times New Roman";
    font-size:20px;
}

```

然后在HTML中加入：



```
<style type="text/css">
 
 @import"test.css"
 
</style>

```

最后效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6a6af1730ba74b8f89d2ce8a023a284e.png)


### JS


JavaScript 是 Web 的编程语言。所有现代的 HTML 页面都可以使用 JavaScript。


1. HTML 定义了网页的内容
2. CSS 描述了网页的布局
3. JavaScript 控制了网页的行为


百分比进度条实例：


HTML定义：



```
<!DOCTYPE html>
<html>
<!-- 头定义 -->
<head>
<meta charset="utf-8">
<title>Frank的个人博客(frank)</title>
</head>

<style type="text/css">
 
 @import"test.css"
 
</style>

<script src="test.js"></script>

<body>
<!-- 实例 -->
<h1>我的第一个标题</h1>
<p>我的第一个段落。</p>

<!-- 百分比进度条 -->
<h1>JavaScript 百分比进度条</h1>
<div id="myProgress">
  <div id="myBar">10%</div>
</div>
<br>
<button onclick="move()">点我</button>

</body>
</html>


```

CSS定义：



```
body {
    background-color:#d0e4fe;
}
h1 {
    color:orange;
    text-align:center;
}
p {
    font-family:"Times New Roman";
    font-size:20px;
}

#myProgress {
    width: 100%;
    background-color: #ddd;
  }
  
#myBar {
    width: 10%;
    height: 30px;
    background-color: #4CAF50;
    text-align: center;
    line-height: 30px;
    color: white;
  }

```

JS定义：



```
function move() {
  var elem = document.getElementById("myBar");   
  var width = 10;
  var id = setInterval(frame, 10);
  function frame() {
    if (width >= 100) {
      clearInterval(id);
    } else {
      width++; 
      elem.style.width = width + '%'; 
      elem.innerHTML = width \* 1  + '%';
    }
  }
}

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d4461beb8ee44f8a9b79d6fb6b55b912.png)


以上。





