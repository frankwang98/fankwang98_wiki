








#### 文章目录


* + [功能](#_1)
	+ [用法](#_4)
	+ [示例](#_23)




### 功能


input——请求用户输入，即允许程序和人之间进行交互。


### 用法



```
prompt = 'What is the original value? ';
x = input(prompt) 

```

prompt 是指向用户展示的文本。


显示 prompt 中的文本并等待用户输入值后按 回车键。用户可以输入 pi/4 或 rand(3) 之类的表达式，并可以使用工作区中的变量。


若输入空，则会返回空矩阵；若输入无效的表达式，则显示错误信息并重新提示输入。



```
txt = input(prompt,"s") 

```

返回输入的文本"s"，不会将输入作为表达式来计算。


### 示例


1.请求数值输入或表达式输入。



```
% 请求数值输入
prompt = "What is the original value? ";
x = input(prompt)
y = x\*10

```

输入数值（如12），返回120。



```
% 请求表达式输入
prompt = "What is the original value? ";
x = input(prompt)
y = x\*10

```

输入magic(3)，返回如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/294c09234a0d4a50a886b135609b0822.png)


2.请求未处理的文本输入


请求文本响应（如条件语句Y/N）



```
prompt = "Do you want more? Y/N [Y]: ";
txt = input(prompt,"s");
if isempty(txt)
    txt = 'Y';
end
%disp('txt')

```

input 函数返回与键入内容完全相同的文本。如果输入为空，此代码将为 txt 指定默认值 ‘Y’。


以上。





