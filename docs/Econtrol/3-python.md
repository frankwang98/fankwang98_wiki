# Python学习笔记
## 一、Python介绍
### 简介
Python 是一种**解释型、交互型、面向对象、动态数据类型**的、适合初学者的高级程序设计语言。		
Python 2.7 被确定为最后一个 Python 2.x 版本。	
Python 3.x 不向下兼容。	
以下都是基于Python 3 。
### 过往
	Python 是由 Guido van Rossum 在八十年代末和九十年代初，在荷兰国家数学和计算机科学研究所设计出来的。	
	Python 本身也是由诸多其他语言发展而来的,这包括 ABC、Modula-3、C、C++、Algol-68、SmallTalk、Unix shell 和其他的脚本语言等等。	
	像 Perl 语言一样，Python 源代码同样遵循 GPL(GNU General Public License)协议。	
### 特点
    1.易于学习：Python有相对较少的关键字，结构简单，和一个明确定义的语法，学习起来更加简单。

    2.易于阅读：Python代码定义的更清晰。

    3.易于维护：Python的成功在于它的源代码是相当容易维护的。

    4.一个广泛的标准库：Python的最大的优势之一是丰富的库，跨平台的，在UNIX，Windows和Macintosh兼容很好。

    5.互动模式：互动模式的支持，您可以从终端输入执行代码并获得结果的语言，互动的测试和调试代码片断。

    6.可移植：基于其开放源代码的特性，Python已经被移植（也就是使其工作）到许多平台。

    7.可扩展：如果你需要一段运行很快的关键代码，或者是想要编写一些不愿开放的算法，你可以使用C或C++完成那部分程序，然后从你的Python程序中调用。

    8.数据库：Python提供所有主要的商业数据库的接口。

    9.GUI编程：Python支持GUI可以创建和移植到许多系统调用。

    10.可嵌入: 你可以将Python嵌入到C/C++程序，让你的程序的用户获得"脚本化"的能力。

### Python应用
- 豆瓣网 - 图书、唱片、电影等文化产品的资料数据库网站
- 知乎 - 一个问答网站
- Blender - 使用Python作为建模工具与GUI语言的开源3D绘图软件
## 二、环境搭建
以下基于Linux平台，其他平台请自查：	
Python3安装：`sudo apt-get install python3`	
查看版本：	
```
python3 -V
或
python3 --version
```
## 三、第一个Python程序
编辑程序：
```
#!/usr/bin/python3
print("Hello, World!")

```
执行程序：	
`python3 hello.py`
## 四、Python常用语法实践
### 编码
默认情况下，Python 3 源码文件以 UTF-8 编码，所有字符串都是 unicode 字符串。	
`# -*- coding: utf-8 -*-`	
### 标识符
    第一个字符必须是字母表中字母或下划线 _ 。
    标识符的其他的部分由字母、数字和下划线组成。
    标识符对大小写敏感。
在 Python 3 中，可以用中文作为变量名，非 ASCII 标识符也是允许的了。
### 保留字
保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字： 
```
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```
### 注释
	单行注释以 # 开头
	多行注释可以用多个 # 号，还有 ''' 和 """
### 行与缩进 
	python最具特色的就是使用缩进来表示代码块，不需要使用大括号 {} ，但若缩进错误会导致程序出错。
```
if True:
    print ("True")
else:
    print ("False")
```
### 多行语句
Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠 \ 来实现多行语句。	
```
total = item_one + \
        item_two + \
        item_three
```
在 [], {}, 或 () 中的多行语句，不需要使用反斜杠 \。
```
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```
### 数字类型（Number）
python中数字有四种类型：整数、布尔型、浮点数和复数。		

    int (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
    bool (布尔), 如 True。
    float (浮点数), 如 1.23、3E-2
    complex (复数), 如 1 + 2j、 1.1 + 2.2j
### 字符串（String）
	Python 中单引号 ' 和双引号 " 使用完全相同。
    使用三引号(''' 或 """)可以指定一个多行字符串。
    转义符 \。
    反斜杠可以用来转义，使用 r 可以让反斜杠不发生转义。 如 r"this is a line with \n" 则 \n 会显示，并不是换行。
    按字面意义级联字符串，如 "this " "is " "string" 会被自动转换为 this is string。
    字符串可以用 + 运算符连接在一起，用 * 运算符重复。
    Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。
    Python 中的字符串不能改变。
    Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。
    字符串的截取的语法格式如下：变量[头下标:尾下标:步长]
```
#!/usr/bin/python3
word = '字符串'
sentence = "这是一个句子。"
paragraph = """这是一个段落，
可以由多行组成"""

str='123456789'
 
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符
print(str[0])              # 输出字符串第一个字符
print(str[2:5])            # 输出从第三个开始到第五个的字符
print(str[2:])             # 输出从第三个开始后的所有字符
print(str[1:5:2])          # 输出从第二个开始到第五个且每隔一个的字符（步长为2）
print(str * 2)             # 输出字符串两次
print(str + '你好')         # 连接字符串
print('------------------------------')
print('hello\nrunoob')      # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob')     # 在字符串前面添加一个 r，表示原始字符串，不会发生转义

```
### 空行
函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

空行与代码缩进不同，空行并不是 Python 语法的一部分。书写时不插入空行，Python 解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

记住：空行也是程序代码的一部分。
### import 与 from...import
在 python 用 import 或者 from...import 来导入相应的模块。	
将整个模块(somemodule)导入，格式为： `import somemodule	`	
从某个模块中导入某个函数,格式为： `from somemodule import somefunction`		
从某个模块中导入多个函数,格式为： `from somemodule import firstfunc, secondfunc, thirdfunc`		
将某个模块中的全部函数导入，格式为： `from somemodule import *`		
```
#!/usr/bin/python3
# 查询命令行参数和路径
import sys
print('================Python import mode==========================')
print ('命令行参数为:')
for i in sys.argv:
    print (i)
print ('\n python 路径为',sys.path)
```
### Python运算符
#### 算术运算符
```
#!/usr/bin/python3
 
a = 21
b = 10
c = 0
 
c = a + b
print ("1 - c 的值为：", c)
 
c = a - b
print ("2 - c 的值为：", c)
 
c = a * b
print ("3 - c 的值为：", c)
 
c = a / b
print ("4 - c 的值为：", c)
 
c = a % b
print ("5 - c 的值为：", c)
 
# 修改变量 a 、b 、c
a = 2
b = 3
c = a**b 
print ("6 - c 的值为：", c)
 
a = 10
b = 5
c = a//b 
print ("7 - c 的值为：", c)

```
#### 比较运算符
```
#!/usr/bin/python3
 
a = 21
b = 10
c = 0
 
if ( a == b ):
   print ("1 - a 等于 b")
else:
   print ("1 - a 不等于 b")
 
if ( a != b ):
   print ("2 - a 不等于 b")
else:
   print ("2 - a 等于 b")
 
if ( a < b ):
   print ("3 - a 小于 b")
else:
   print ("3 - a 大于等于 b")
 
if ( a > b ):
   print ("4 - a 大于 b")
else:
   print ("4 - a 小于等于 b")
 
# 修改变量 a 和 b 的值
a = 5
b = 20
if ( a <= b ):
   print ("5 - a 小于等于 b")
else:
   print ("5 - a 大于  b")
 
if ( b >= a ):
   print ("6 - b 大于等于 a")
else:
   print ("6 - b 小于 a")

```
#### 赋值运算符
```
#!/usr/bin/python3
 
a = 21
b = 10
c = 0
 
c = a + b
print ("1 - c 的值为：", c)
 
c += a
print ("2 - c 的值为：", c)
 
c *= a
print ("3 - c 的值为：", c)
 
c /= a 
print ("4 - c 的值为：", c)
 
c = 2
c %= a
print ("5 - c 的值为：", c)
 
c **= a
print ("6 - c 的值为：", c)
 
c //= a
print ("7 - c 的值为：", c)

```
#### 位运算
```
#!/usr/bin/python3
 
a = 60            # 60 = 0011 1100 
b = 13            # 13 = 0000 1101 
c = 0
 
c = a & b        # 12 = 0000 1100
print ("1 - c 的值为：", c)
 
c = a | b        # 61 = 0011 1101 
print ("2 - c 的值为：", c)
 
c = a ^ b        # 49 = 0011 0001
print ("3 - c 的值为：", c)
 
c = ~a           # -61 = 1100 0011
print ("4 - c 的值为：", c)
 
c = a << 2       # 240 = 1111 0000
print ("5 - c 的值为：", c)
 
c = a >> 2       # 15 = 0000 1111
print ("6 - c 的值为：", c)
```
#### 逻辑运算符
```
#!/usr/bin/python3
 
a = 10
b = 20
 
if ( a and b ):
   print ("1 - 变量 a 和 b 都为 true")
else:
   print ("1 - 变量 a 和 b 有一个不为 true")
 
if ( a or b ):
   print ("2 - 变量 a 和 b 都为 true，或其中一个变量为 true")
else:
   print ("2 - 变量 a 和 b 都不为 true")
 
# 修改变量 a 的值
a = 0
if ( a and b ):
   print ("3 - 变量 a 和 b 都为 true")
else:
   print ("3 - 变量 a 和 b 有一个不为 true")
 
if ( a or b ):
   print ("4 - 变量 a 和 b 都为 true，或其中一个变量为 true")
else:
   print ("4 - 变量 a 和 b 都不为 true")
 
if not( a and b ):
   print ("5 - 变量 a 和 b 都为 false，或其中一个变量为 false")
else:
   print ("5 - 变量 a 和 b 都为 true")
```
#### 成员运算符
```
#!/usr/bin/python3
 
a = 10
b = 20
list = [1, 2, 3, 4, 5 ]
 
if ( a in list ):
   print ("1 - 变量 a 在给定的列表中 list 中")
else:
   print ("1 - 变量 a 不在给定的列表中 list 中")
 
if ( b not in list ):
   print ("2 - 变量 b 不在给定的列表中 list 中")
else:
   print ("2 - 变量 b 在给定的列表中 list 中")
 
# 修改变量 a 的值
a = 2
if ( a in list ):
   print ("3 - 变量 a 在给定的列表中 list 中")
else:
   print ("3 - 变量 a 不在给定的列表中 list 中")
```
#### 身份运算符
```
#!/usr/bin/python3
# 注： id() 函数用于获取对象内存地址。
a = 20
b = 20
 
if ( a is b ):
   print ("1 - a 和 b 有相同的标识")
else:
   print ("1 - a 和 b 没有相同的标识")
 
if ( id(a) == id(b) ):
   print ("2 - a 和 b 有相同的标识")
else:
   print ("2 - a 和 b 没有相同的标识")
 
# 修改变量 b 的值
b = 30
if ( a is b ):
   print ("3 - a 和 b 有相同的标识")
else:
   print ("3 - a 和 b 没有相同的标识")
 
if ( a is not b ):
   print ("4 - a 和 b 没有相同的标识")
else:
   print ("4 - a 和 b 有相同的标识")
# is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。
``` 
### Python流程控制
#### if
```
#!/usr/bin/python3
var1 = 100
if var1:
    print ("1 - if 表达式条件为 true")
    print (var1)
 
var2 = 0
if var2:
    print ("2 - if 表达式条件为 true")
    print (var2)
print ("Good bye!")
```
#### if-elif
```
#!/usr/bin/python3
age = int(input("请输入你家狗狗的年龄: "))
print("")
if age <= 0:
    print("你是在逗我吧!")
elif age == 1:
    print("相当于 14 岁的人。")
elif age == 2:
    print("相当于 22 岁的人。")
elif age > 2:
    human = 22 + (age -2)*5
    print("对应人类年龄: ", human)
 
### 退出提示
input("点击 enter 键退出")
```
#### if嵌套
```
#!/usr/bin/python3
num=int(input("输入一个数字："))
if num%2==0:
    if num%3==0:
        print ("你输入的数字可以整除 2 和 3")
    else:
        print ("你输入的数字可以整除 2，但不能整除 3")
else:
    if num%3==0:
        print ("你输入的数字可以整除 3，但不能整除 2")
    else:
        print  ("你输入的数字不能整除 2 和 3")
```
#### while
```
#!/usr/bin/python3
# 计算0-100的和
n = 100
 
sum = 0
counter = 1
while counter <= n:
    sum = sum + counter
    counter += 1
 
print("1 到 %d 之和为: %d" % (n,sum))
```
#### while-else
```
#!/usr/bin/python3
 
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```
#### while无限循环
```
#!/usr/bin/python3
# 当要循环的语句很简单时，可以写在同一行
flag = 1
 
while (flag): print ('欢迎访问菜鸟教程!')
 
print ("Good bye!")
```
#### for
```
#!/usr/bin/python3

sites = ["Baidu", "Google","Runoob","Taobao"]
for site in sites:
    if site == "Runoob":
        print("菜鸟教程!")
        break
    print("循环数据 " + site)
else:
    print("没有循环数据!")
print("完成循环!")
# break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。
# continue 语句被用来告诉 Python 跳过当前循环块中的剩余语句，然后继续进行下一轮循环。 
```
### Python函数
函数是**组织好的，可重复使用的，用来实现单一，或相关联功能**的代码段。

函数能提高应用的模块性，和代码的重复利用率。你已经知道Python提供了许多内建函数，比如print()。但你也可以自己创建函数，这被叫做用户自定义函数。
#### 函数定义
```
#!/usr/bin/python3
'''
def 函数名（参数列表）:
    函数体
'''
def hello() :
    print("Hello World!")

hello()
```
#### return语句
```
#!/usr/bin/python3
# 可写函数说明
def sum( arg1, arg2 ):
   # 返回2个参数的和."
   total = arg1 + arg2
   print ("函数内 : ", total)
   return total
 
# 调用sum函数
total = sum( 10, 20 )
print ("函数外 : ", total)
```
### Python面向对象
Python从设计之初就已经是一门面向对象的语言，正因为如此，在Python中创建一个类和对象是很容易的。	

    类(Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
    方法：类中定义的函数。
    类变量：类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
    数据成员：类变量或者实例变量用于处理类及其实例对象的相关的数据。
    方法重写：如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
    局部变量：定义在方法中的变量，只作用于当前实例的类。
    实例变量：在类的声明中，属性是用变量来表示的，这种变量就称为实例变量，实例变量就是一个用 self 修饰的变量。
    继承：即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
    实例化：创建一个类的实例，类的具体对象。
    对象：通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。
#### 类定义
```
#!/usr/bin/python3

class MyClass:
    """一个简单的类实例"""
    i = 12345
    def f(self):
        return 'hello world'
 
# 实例化类
x = MyClass()
 
# 访问类的属性和方法
print("MyClass 类的属性 i 为：", x.i)
print("MyClass 类的方法 f 输出为：", x.f())
# 类有一个名为 __init__() 的特殊方法（构造方法），该方法在类实例化时会自动调用
# self代表类的实例，而非类
```
#### 类的方法
```
#!/usr/bin/python3

#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
# 实例化类
p = people('runoob',10,30)
p.speak()
```
#### 继承
Python 同样支持类的继承，如果一种语言不支持继承，类就没有什么意义。	
```
#!/usr/bin/python3

#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 
 
s = student('ken',10,60,3)
s.speak()
```
### 命名空间/作用域
一般有三种命名空间：

    内置名称（built-in names）， Python 语言内置的名称，比如函数名 abs、char 和异常名称 BaseException、Exception 等等。
    全局名称（global names），模块中定义的名称，记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。
    局部名称（local names），函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）
```
#!/usr/bin/python3

total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```
## 五、Python标准库
### 操作系统接口
os模块提供了不少与操作系统相关联的函数，因此能和shell配合做一些自动化测试程序！	
```
import os
os.getcwd()      # 返回当前的工作目录
os.chdir('/server/accesslogs')   # 修改当前的工作目录
os.system('mkdir today')   # 执行系统命令 mkdir 
```
### 文件通配符
glob模块提供了一个函数用于从目录通配符搜索中生成文件列表
```
import glob
glob.glob('*.py')
```
### 命令行参数
通用工具脚本经常调用命令行参数。这些命令行参数以链表形式存储于 sys 模块的 argv 变量。
```
import sys
print(sys.argv)
```
### 错误输出重定向和程序终止
```
sys.stderr.write('Warning, log file not found starting a new one\n')
```
sys 还有 stdin，stdout 和 stderr 属性，即使在 stdout 被重定向时，后者也可以用于显示警告和错误信息。	
大多脚本的定向终止都使用 "sys.exit()"。	
### 数学
math模块为浮点运算提供了对底层C函数库的访问: 	
```
import math
math.cos(math.pi / 4)
```
random提供了生成随机数的工具。
```
import random
random.choice(['apple', 'pear', 'banana'])
```
### 日期和时间
datetime模块为日期和时间处理同时提供了简单和复杂的方法。

支持日期和时间算法的同时，实现的重点放在更有效的处理和格式化输出。

该模块还支持时区处理: 
```
#!/usr/bin/python3

from datetime import date
now = date.today()
now
print(now)
```
### 2to3工具-自动将 Python 2 代码转为 Python 3 代码
工具安装：`sudo apt install 2to3`

测试：这里有一个 Python 2.x 的源码文件，example.py	
```
def greet(name):
    print "Hello, {0}!".format(name)
print "What's your name?"
name = raw_input()
greet(name)
```
最常用的命令是加上-w参数，`2to3 -w example.py`，最后生成：	
```
def greet(name):
    print("Hello, {0}!".format(name))
print("What's your name?")
name = input()
greet(name)
```
注释和缩进都会在转换过程中保持不变。

官方参考文档：[2to3](https://docs.python.org/zh-cn/3/library/2to3.html)

## 六、Python应用案例
### 网络编程
Python 提供了两个级别访问的网络服务。：

    低级别的网络服务支持基本的 Socket，它提供了标准的 BSD Sockets API，可以访问底层操作系统Socket接口的全部方法。
    高级别的网络服务模块 SocketServer， 它提供了服务器中心类，可以简化网络服务器的开发。
#### 什么是 Socket?

Socket又称"套接字"，应用程序通常通过"套接字"向网络发出请求或者应答网络请求，使主机间或者一台计算机上的进程间可以通讯。
#### socket()函数

Python 中，我们用 socket() 函数来创建套接字，语法格式如下：	
`socket.socket([family[, type[, proto]]])`

    family: 套接字家族可以是 AF_UNIX 或者 AF_INET
    type: 套接字类型可以根据是面向连接的还是非连接分为SOCK_STREAM或SOCK_DGRAM
    protocol: 一般不填默认为0.
#### 服务端实例
```
#!/usr/bin/python3
# 我们使用 socket 模块的 socket 函数来创建一个 socket 对象。socket 对象可以通过调用其他函数来设置一个 socket 服务。

# 现在我们可以通过调用 bind(hostname, port) 函数来指定服务的 port(端口)。

# 接着，我们调用 socket 对象的 accept 方法。该方法等待客户端的连接，并返回 connection 对象，表示已连接到客户端。
# 导入 socket、sys 模块
import socket
import sys

# 创建 socket 对象
serversocket = socket.socket(
            socket.AF_INET, socket.SOCK_STREAM)

# 获取本地主机名
host = socket.gethostname()
print(host)

port = 9999

# 绑定端口号
serversocket.bind((host, port))

# 设置最大连接数，超过后排队
serversocket.listen(5)

while True:
    # 建立客户端连接
    clientsocket,addr = serversocket.accept()      

    print("连接地址: %s" % str(addr))
   
    msg='欢迎访问菜鸟教程！'+ "\r\n"
    clientsocket.send(msg.encode('utf-8'))
    clientsocket.close()
```
#### 客户端实例
```
#!/usr/bin/python3

# 接下来我们写一个简单的客户端实例连接到以上创建的服务。端口号为 9999。

# socket.connect(hostname, port ) 方法打开一个 TCP 连接到主机为 hostname 端口为 port 的服务商。连接后我们就可以从服务端获取数据，记住，操作完成后需要关闭连接。

# 导入 socket、sys 模块
import socket
import sys

# 创建 socket 对象
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 获取本地主机名
host = socket.gethostname()

# 设置端口号
port = 9999

# 连接服务，指定主机和端口
s.connect((host, port))

# 接收小于 1024 字节的数据
msg = s.recv(1024)

s.close()

print (msg.decode('utf-8'))
```
### Python多线程
### Python日期和时间
Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间。	

每个时间戳都以自从 1970 年 1 月 1 日午夜（历元）经过了多长时间来表示。

Python 的 time 模块下有很多函数可以转换常见日期格式。如函数 time.time() 用于获取当前时间戳, 如下实例:	
```
import time  # 引入time模块

ticks = time.time()
print ("当前时间戳为:", ticks)
```
### Python内置函数
[参考](https://www.runoob.com/python3/python3-built-in-functions.html)
### Python pip
pip 是 Python 包管理工具，该工具提供了对 Python 包的查找、下载、安装、卸载的功能。

软件包也可以在 https://pypi.org/ 中找到。

目前最新的 Python 版本已经预装了 pip。
```
pip --version	# 版本
pip install some-package-name	# 安装包
pip uninstall some-package-name	# 移除包
pip list	# 查看包
```
## 七、数据结构与算法(Python)
### 二分查找（递归）
```
#!/usr/bin/python3

# 返回 x 在 arr 中的索引，如果不存在返回 -1
def binarySearch (arr, l, r, x): 
  
    # 基本判断
    if r >= l: 
  
        mid = int(l + (r - l)/2)
  
        # 元素整好的中间位置
        if arr[mid] == x: 
            return mid 
          
        # 元素小于中间位置的元素，只需要再比较左边的元素
        elif arr[mid] > x: 
            return binarySearch(arr, l, mid-1, x) 
  
        # 元素大于中间位置的元素，只需要再比较右边的元素
        else: 
            return binarySearch(arr, mid+1, r, x) 
  
    else: 
        # 不存在
        return -1
  
# 测试数组
arr = [ 2, 3, 4, 10, 40 ] 
x = 10
  
# 函数调用
result = binarySearch(arr, 0, len(arr)-1, x) 
  
if result != -1: 
    print ("元素在数组中的索引为 %d" % result )
else: 
    print ("元素不在数组中")
```
