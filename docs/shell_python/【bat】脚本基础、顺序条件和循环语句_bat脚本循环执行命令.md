






脚本（Script）语言是一种动态的、解释性的语言，依据一定的格式编写的可执行文件，又称作宏或批处理文件。脚本语言具有小巧便捷、快速开发的特点；常见的脚本语言有`Windows批处理脚本bat、Linux脚本语言shell以及python、matlab等`，脚本语言常用于安装或运行程序，执行重复操作等。



> 
> 用好脚本语言可以大大提高工作效率，已经成为运维人员的必备技能之一。
> 
> 
> 


顺序、条件和循环语句是程序中三种常用的结构语句。




#### 文章目录


* + [脚本基础](#_7)
	+ [顺序语句](#_16)
	+ [条件语句](#_28)
	+ [循环语句](#_66)




### 脚本基础


脚本（Script）在IT领域是舶来品，最早是从演艺界出现的。如果没有脚本，表演者只能即兴发挥，或者靠导演的口述来进行。无论是在演艺界还是IT领域，**脚本都有以下几个特点**：


1. 设定一个规程，可重复执行；
2. 需要具体的人/机器去做；
3. 能够方便的，快速的，经常的被修改。


脚本语言是实现运维和测试自动化的关键手段，否则同样的操作手工执行的话不仅效率低，人还会很累，**要把更多的经历放在创造性工作上，这就是我们要学好脚本语言的动力**。


### 顺序语句


顺序语句包含常见的赋值语句、文件处理语句、输出语句等，如：



```
set var = 1
cd /d c:\
md test
ping /n 10 baidu.com > test.txt
del test.txt
echo hello_world

```

### 条件语句


条件语句常用的是if-else，如：



```
## 选择语句
if 条件 (do...)
if 条件 (do...) else (do ...)

```

判断文件是否存在的脚本：



```
@echo off
if EXIST readme.txt (
  echo readme.txt exist.
) else (
  echo readme.txt not exist.
)
pause

```

根据用户输入执行对应语句：



```
@echo off
set /p var="Please input the number(1,2,3):"
if %var% == 1 (
  echo "the number equal to 1"
) else if %var% == 2 (
  echo "the number equal to 2"
) else if %var% == 3 (
  echo "the number equal to 3"
) else (
  echo "input wrong number,exit."
)
pause

```

### 循环语句


循环语句常用的是for循环，如：



```
## 循环语句
FOR /L %variable IN (start,step,end) do (command [command-parameters])

```

打印从1到10的数字：



```
@echo off
for /l %%i in (1,1,10) do (echo %%i)
pause

```

执行循环内所有操作：



```
@echo off
for %%a in (A,B,C,D) do (echo %%a)
pause

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/39c4e81957a44cffb063968da08690b9.png)


以上。





