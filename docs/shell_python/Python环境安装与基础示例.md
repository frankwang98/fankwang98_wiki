







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Python环境配置与基础语法。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. python介绍](#smirk1_python_7)
	+ [:blush:2. 环境安装与配置](#blush2__25)
	+ - [Windows安装python](#Windowspython_26)
		- [Ubuntu安装python](#Ubuntupython_47)
		- [miniconda安装](#miniconda_77)
	+ [:satisfied:3. python基础语法](#satisfied3_python_101)
	+ - [第一个python程序](#python_108)
		- [语法基础](#_115)
		- [数据类型](#_161)
		- [控制流语句](#_194)
		- [函数](#_228)
		- [文件操作](#_242)
		- [模块](#_278)
		- [类](#_289)




### 😏1. python介绍


`Python`是一种高级编程语言，由Guido van Rossum于1991年创建。它被设计成易读、简洁、可扩展的语言，具有强大的功能和广泛的应用领域。


以下是Python的一些重要特点：



> 
> 1.简单易学：Python使用清晰简洁的语法，易于阅读和理解，适合初学者入门。它的设计哲学强调代码的可读性和明确性。
> 
> 
> 



> 
> 2.开源和跨平台：Python是开源的，可以免费使用和分发。它支持在多个操作系统上运行，包括Windows、macOS和各种Linux发行版。
> 
> 
> 



> 
> 3.大量的库和框架：Python拥有庞大而活跃的社区，提供了丰富的第三方库和框架，使开发人员能够快速构建各种应用程序，例如科学计算、数据分析、Web开发、人工智能等。
> 
> 
> 



> 
> 4.面向对象编程：Python支持面向对象编程（OOP），允许开发人员使用类、对象、继承和多态等概念来组织和管理代码。
> 
> 
> 



> 
> 5.动态类型和自动内存管理：Python是一种动态类型语言，不需要显式地声明变量类型，这使得代码编写更加灵活。此外，Python还具有自动内存管理机制，开发人员不需要手动处理内存分配和释放。
> 
> 
> 



> 
> 6.多用途：Python可用于各种任务，包括脚本编写、快速原型开发、图像处理、网络编程、游戏开发等。它还是许多流行软件工具和框架的首选语言，如Django、Flask、NumPy和Pandas等。
> 
> 
> 


综上，Python因其简洁性、可读性和功能强大而受到广泛的欢迎。无论你是初学者还是经验丰富的开发人员，Python都是一个值得学习和掌握的优秀编程语言。


### 😊2. 环境安装与配置


#### Windows安装python


首先在官网下载安装包：`https://www.python.org/downloads/windows/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/1342ddcb7f434adda402e84406522b64.png)


这里我选择3.7.1下载安装（默认或自定义安装）。


然后打开`cmd`命令行输入`python`，即可进入交互环境，并查看版本`python -V`。


更换国内源：



```
# 在用户下创建pip文件夹，并创建pip.ini文件
[global]
index-url = http://pypi.douban.com/simple/
# index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = pypi.douban.com
# trusted-host = https://pypi.tuna.tsinghua.edu.cn

```

#### Ubuntu安装python


ubuntu默认安装了python，ubuntu18默认是`python2`，ubuntu20之后默认是`python3`。


如果需要安装指定的python3版本，以下：



```
# 安装指定版本python
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar -zxvf Python-3.7.1.tgz
cd Python-3.7.1
./configure
# 或者
./configure --prefix=/usr/local/python3.7.1
make
make test
sudo make install

# 更换默认python版本
sudo mv /usr/bin/python /usr/bin/python.bak
sudo ln -s /usr/local/python3.7.1/bin/python3.7 /usr/bin/python
sudo mv /usr/bin/pip /usr/bin/pip.bak
sudo ln -s /usr/local/python3.7.1/bin/pip3 /usr/bin/pip

```

python安装扩展包非常方便，可以通过`pip install xxx`指令安装，如：



```
pip install numpy

```

#### miniconda安装


为了便于对python环境及pip包做管理，可以安装`anaconda`，但它也有缺点（运行卡顿等），因此可选择`miniconda`，可方便创建多个版本的python环境。


`miniconda`安装：



```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sudo chmod +x Miniconda3-latest-Linux-x86_64.sh
sudo ./Miniconda3-latest-Linux-x86_64.sh
/opt/miniconda3	# 选择安装目录
# 打开~/.bashrc文件，在文件末尾加入如下内容
export PATH="/opt/miniconda3/bin:$PATH"

```

`conda`使用：



```
conda install python=3.7
conda create -n <my_environment> python=3.7	# 创建指定名字的环境
# 若出错，是因为权限问题 sudo chmod a+w .conda
conda activate <my_environment>	# 生效环境
conda env list	# 查看环境列表
conda env remove --name <my_environment> --all	# 删除环境

```

Python作为一种语言，与英语一样，要多用才能更熟练。在做项目、解决问题的过程中会自然而然地熟悉和进步。


### 😆3. python基础语法


python是解释型语言，因此程序不需要编译可以直接运行，在终端运行python程序可以直接`python xxx.py`，如：



```
python main.py

```

为了编写和运行方便，可以安装`VSCode`或`Pycharm`这种IDE，有了插件的加持编写代码更加方便。


#### 第一个python程序



```
#!/usr/bin/python3

print("Hello, World!")

```

#### 语法基础


Python标识符也是要以字母或下划线开头，且对大小写敏感。如`value setValue ClassExample`。


关键字（保留字）不能用于标识符的名称。查看python中的所有关键字，可以用标准库的`keyword`模块：



```
import keyword
print(keyword.kwlist)

```

Python中单行注释以`#`开头，多行注释可以用多个`#`号，或者三引号`'''`和`"""`。



```
# 单行注释

'''
多行注释1
多行注释2
多行注释3
'''

"""
多行注释4
多行注释5
多行注释6
"""

```

Python与C++一个大的差异是，python用缩进来控制代码块，而不是`{}`。



```
if True:
    print ("now is True")
else:
    print ("now is False")

```

Python语句末尾不需要分号，但需要在一行显示多条语句时，用`分号;`隔开。


用 `import` 或者 `from...import` 来导入相应的模块。如



```
import xxx # 导入整个模块
from xxx import xxx # 导入模块的某个函数
from xxx import xxx, xxx # 导入模块的多个函数
from xxx import \* # 导入模块的所有函数

```

#### 数据类型


Python中数字有3种类型，即`整数int、浮点数float和复数complex`。


`字符串string`可以用单引号或双引号表示，且没有单独的字符类型，一个字符就是长度为1的字符串。转义符`\`可以用来转义，如`\n`，但在string前加上`r`可以使其不转义，输出本身的值。



```
str = '123456789'
print(str + r"hello\nworld")

```

此外，还有`布尔型bool、列表list、元组tuple（不可变）、集合set、字典dictionary`。



```
# list
my_list = [1, 2, 3, "apple", "banana"]

print(my_list[0])  # 输出：1
print(my_list[3])  # 输出："apple"

my_list.append("orange")  # 添加元素
print(my_list)  # 输出：[1, 2, 3, "apple", "banana", "orange"]

my_list.remove(2)  # 移除元素
print(my_list)  # 输出：[1, 3, "apple", "banana", "orange"]

# tuple（不可变）
my_tuple = (1, 2, 3, "apple", "banana")

print(my_tuple[0])  # 输出：1
print(my_tuple[3])  # 输出："apple"

```

Python的数据类型间的转换，可以直接将数据类型作为函数名即可。如`float(x)`。


#### 控制流语句



```
number = 7
guess = -1
print("猜数字游戏!")

# while循环
while guess != number:
    guess = int(input("请输入你猜的数字：")) # input输入
	# if else条件
    if guess == number:
        print("恭喜，你猜对了！")
    elif guess < number:
        print("猜的数字小了...")
    elif guess > number:
        print("猜的数字大了...")

```


```
# for循环
# list
sites = ["Baidu", "Google", "Yandex"]
for site in sites:
    print(site)
# string
word = 'python'
for letter in word:
    print(letter)
# number
for number in range(1, 8):
    print(number)

```

#### 函数



```
# 最大值函数
def max(a, b):
    if a > b:
        return a
    else:
        return b

a = 9
b = 5
print(max(a, b))

```

#### 文件操作



```
import os

def writeFile():
    # 创建文件
    file = open("example.txt", "w")
    # 写入文件内容
    file.write("This is a new file.")
    # 追加写入文件内容
    file.write("\nThis is a new line 2.")
    # 关闭文件
    file.close()

def readFile():
    # 创建文件
    file = open("example.txt", "r")
    # 读取文件内容
    content = file.read()
    print(content)
    # 关闭文件
    file.close()

def removeFile():
    # 删除文件
    if os.path.exists("example.txt"):
        os.remove("example.txt")
    else:
        print("The file does not exist.")

removeFile()
writeFile()
readFile()

```

#### 模块



```
import sys

print('命令行参数如下:')
for i in sys.argv:
    print(i)

print('Python 路径为：', sys.path, '\n')

```

#### 类



```
# 类定义
class people:
    # 定义基本属性
    name = ''
    age = 0
    # 定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0

    # 定义构造方法（初始化）
    def \_\_init\_\_(self, n, a, w):
        self.name = n
        self.age = a
        self.__weight = w

    def speak(self):
        print("%s 说: 我 %d 岁， %d kg。" % (self.name, self.age, self.__weight))


# 实例化类
p = people('XiaoMing', 18, 60)
p.speak()

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4dad9a61ecf847938b2fb5156fc50872.png#pic_center)


以上。





