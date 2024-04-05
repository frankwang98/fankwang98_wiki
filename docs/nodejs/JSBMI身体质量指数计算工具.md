







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍BMI身体质量指数计算工具的JS实现。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 知识介绍](#smirk1__7)
	+ [:blush:2. BMI计算工具的JS实现](#blush2_BMIJS_25)
	+ [:satisfied:3. 黑白主题切换](#satisfied3__86)




### 😏1. 知识介绍


`BMI`（Body Mass Index，身体质量指数），也称为体重指数，是一种常用的衡量成人人体肥胖程度的指标。它通过身高和体重之间的数值关系来评估一个人的体重是否适中。


BMI的计算公式如下：



```
BMI = 体重（kg）/ (身高（m） * 身高（m）)

```

根据计算得到的BMI值，可以将人体的体重状况分为以下几个范围：



```
BMI < 18.5：体重过轻
18.5 <= BMI < 24：体重正常
24 <= BMI < 28：超重
BMI >= 28：肥胖

```

### 😊2. BMI计算工具的JS实现


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/63deadf5d0524cf9995a95eec5763c83.png)


代码示例：



```
<!DOCTYPE html>
<html>
<head>
  <title>BMI Calculator</title>
  <style>
    /\* create style container \*/
    .container {
      width: 300px;
      margin: 0 auto;
      padding: 20px;
      border: 1px solid #ccc;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- container include header + label + input + button + result -->
    <h2>BMI Calculator</h2>
    <label for="weight">Weight (kg):</label>
    <input type="number" id="weight" min="0" step="0.01" required><br><br>
    <label for="height">Height (cm):</label>
    <input type="number" id="height" min="0" step="0.01" required><br><br>
    <button onclick="calculateBMI()">Calculate BMI</button><br><br>
    <div id="result"></div>
  </div>

  <script>
    // calculateBMI
    function calculateBMI() {
      var weightInput = document.getElementById("weight");
      var heightInput = document.getElementById("height");
      var resultDiv = document.getElementById("result");

      var weight = parseFloat(weightInput.value);
      var height = parseFloat(heightInput.value);

      // 检查输入是否有效
      if (isNaN(weight) || isNaN(height) || weight <= 0 || height <= 0) {
        resultDiv.innerHTML = "Please enter valid weight and height.";
      } else {
        // 将身高转换为米
        height = height / 100;
        // 计算BMI
        var bmi = weight / (height \* height);
        // 显示结果
        resultDiv.innerHTML = "Your BMI is: " + bmi.toFixed(2);
      }
    }
  </script>
</body>
</html>

```

### 😆3. 黑白主题切换


晚上使用电脑时，如果还是这种纯白的页面，看起来很不舒服，因此有必要让网页根据时间来切换黑白主题，或者手动切换。


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/17ca44c699634e6c81f592a10510be43.png)


代码示例如下：



```
<!DOCTYPE html>
<html>
<head>
  <title>BMI Calculator</title>
  <style>
    /\* create style container \*/
    .container {
      width: 300px;
      margin: 0 auto;
      padding: 20px;
      border: 1px solid #ccc;
      text-align: center;
    }
    body {
      transition: background-color 0.5s ease;
      background-color: #f5f5f5; /\* 默认白天背景色 \*/
      color: #333; /\* 默认文本颜色 \*/
      text-align: center;
    }
    .container {
      width: 300px;
      margin: 0 auto;
      padding: 20px;
      border: 1px solid #ccc;
    }
    .toggle-btn {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- container include header + label + input + button + result -->
    <h2>BMI Calculator</h2>
    <label for="weight">Weight (kg):</label>
    <input type="number" id="weight" min="0" step="0.01" required><br><br>
    <label for="height">Height (cm):</label>
    <input type="number" id="height" min="0" step="0.01" required><br><br>
    <button onclick="calculateBMI()">Calculate BMI</button><br><br>
    <div id="result"></div>
  </div>

  <div class="container">
    <h2>Day/Night Background</h2>
    <p>Current Time: <span id="time"></span></p>
    <button class="toggle-btn" onclick="toggleBackground()">Toggle Background</button>
  </div>

  <script>
    // calculateBMI
    function calculateBMI() {
      var weightInput = document.getElementById("weight");
      var heightInput = document.getElementById("height");
      var resultDiv = document.getElementById("result");

      var weight = parseFloat(weightInput.value);
      var height = parseFloat(heightInput.value);

      // 检查输入是否有效
      if (isNaN(weight) || isNaN(height) || weight <= 0 || height <= 0) {
        resultDiv.innerHTML = "Please enter valid weight and height.";
      } else {
        // 将身高转换为米
        height = height / 100;
        // 计算BMI
        var bmi = weight / (height \* height);
        // 显示结果
        resultDiv.innerHTML = "Your BMI is: " + bmi.toFixed(2);
      }
    }

    // toggleBackground
    var timeDisplay = document.getElementById("time");
    var toggleButton = document.getElementsByClassName("toggle-btn")[0];
    
    function updateTime() {
      var date = new Date();
      var hours = date.getHours();
      var minutes = date.getMinutes();
      var seconds = date.getSeconds();
      
      var time = hours + ":" + formatTimeComponent(minutes) + ":" + formatTimeComponent(seconds);
      timeDisplay.textContent = time;
      
      // 根据时间设置背景样式，按需要设置
      if (hours >= 18 || hours < 6) { // 如果是晚上（18:00 - 05:59）
        setNightBackground();
      } else { // 如果是白天（06:00 - 17:59）
        setDayBackground();
      }
    }
    
    function formatTimeComponent(component) {
      return component < 10 ? "0" + component : component;
    }
    
    function setDayBackground() {
      document.body.style.backgroundColor = "#f5f5f5";
      document.body.style.color = "#333";
    }
    
    function setNightBackground() {
      document.body.style.backgroundColor = "#333";
      document.body.style.color = "#f5f5f5";
    }
    
    function toggleBackground() {
      var currentColor = document.body.style.backgroundColor;
      if (currentColor === "rgb(245, 245, 245)") {
        setNightBackground();
      } else {
        setDayBackground();
      }
    }
    
    // 更新时间
    updateTime();
    setInterval(updateTime, 1000);
  </script>
</body>
</html>

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





