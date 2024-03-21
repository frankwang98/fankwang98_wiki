







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍标准库常用模块。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 标准库介绍](#smirk1__7)
	+ [:blush:2. 环境安装与配置](#blush2__27)
	+ [:satisfied:3. 应用示例](#satisfied3__33)
	+ - [os库示例](#os_34)
		- [datetime库示例](#datetime_55)
		- [random库示例](#random_75)
		- [math库示例](#math_94)
		- [sys库示例](#sys_123)
		- [json库示例](#json_166)
		- [re库示例](#re_200)
		- [csv库示例](#csv_240)
		- [urllib库示例](#urllib_275)
		- [socket库示例](#socket_306)
		- [logging库示例](#logging_340)




### 😏1. 标准库介绍


Python标准库是Python编程语言的内置模块集合，它提供了广泛的功能和工具，用于开发各种类型的应用程序。下面是一些常用的Python标准库以及它们的简要介绍：



> 
> os：提供与操作系统交互的功能，如文件和目录操作、环境变量访问等。  
>  sys：提供对Python解释器和运行时环境的访问和控制。  
>  datetime：包含处理日期和时间的类和函数，包括日期计算、格式化、解析等。  
>  math：提供常用的数学函数和常量，如三角函数、指数函数、对数函数等。  
>  random：用于生成伪随机数的功能，包括随机数生成器、随机样本选择等。  
>  json：用于处理JSON（JavaScript Object Notation）数据的编码、解码和操作。  
>  re：提供正则表达式匹配和操作的功能。  
>  csv：用于读写CSV（Comma-Separated Values）格式的文件。  
>  urllib：用于进行URL请求和操作，包括HTTP、FTP等。  
>  sqlite3：提供与SQLite数据库进行交互的功能，包括连接、查询、执行事务等。  
>  collections：提供额外的数据类型，如defaultdict、Counter等，用于数据集合的高效操作。  
>  itertools：提供用于迭代操作的函数，如排列组合、循环迭代等。  
>  logging：用于记录日志信息和调试信息的功能，支持多种日志级别和输出方式。  
>  time：提供与时间相关的功能，如获取当前时间、暂停程序执行等。  
>  socket：用于进行网络通信，包括建立TCP/IP连接、发送和接收数据等。
> 
> 
> 


这些只是Python标准库中的一小部分，此外还包含很多其他模块和包，每个模块都提供特定领域的功能和工具。**用好标准库，将大大加快我们的开发速度。**


### 😊2. 环境安装与配置


Python标准库是Python的一部分，不需要单独安装。提前安装好python环境即可。


python环境安装参考：`http://t.csdn.cn/9rV2a`


python导入模块是用`import`，如`import os`


### 😆3. 应用示例


#### os库示例



```
import os

# 获取当前工作目录
current_dir = os.getcwd()
print("当前工作目录：", current_dir)

# 创建新目录
new_dir = "my\_directory"
os.mkdir(new_dir)
print("创建目录：", new_dir)

# 遍历目录中的文件和子目录
for root, dirs, files in os.walk(current_dir):
    print("当前目录：", root)
    print("子目录：", dirs)
    print("文件：", files)


```

#### datetime库示例



```
from datetime import datetime, timedelta

# 获取当前时间
current_time = datetime.now()
print("当前时间：", current_time)

# 格式化日期时间字符串
formatted_time = current_time.strftime("%Y-%m-%d %H:%M:%S")
print("格式化时间：", formatted_time)

# 计算日期差值
future_date = current_time + timedelta(days=7)
days_diff = (future_date - current_time).days
print("未来日期：", future_date)
print("日期差值（天数）：", days_diff)


```

#### random库示例



```
import random

# 生成随机整数
random_number = random.randint(1, 10)
print("1-10的随机整数：", random_number)

# 从列表中随机选择一个元素
my_list = ["apple", "banana", "cherry"]
random_element = random.choice(my_list)
print("随机选择元素：", random_element)

# 打乱列表顺序
random.shuffle(my_list)
print("打乱顺序后的列表：", my_list)


```

#### math库示例



```
import math

# 数学常量
print(math.pi) # 输出圆周率π的值

# 数学函数
print(math.sqrt(16)) # 平方根函数，输出4.0

print(math.ceil(3.7)) # 向上取整，输出4
print(math.floor(3.7)) # 向下取整，输出3

print(math.sin(math.pi/2)) # 正弦函数，输出1.0
print(math.cos(math.pi)) # 余弦函数，输出-1.0
print(math.tan(0)) # 正切函数，输出0.0

print(math.log(10)) # 自然对数函数，默认底数为e，输出2.302585092994046
print(math.log10(100)) # 以10为底的对数函数，输出2.0

print(math.degrees(math.pi/2)) # 弧度转角度，输出90.0
print(math.radians(180)) # 角度转弧度，输出3.141592653589793

print(math.factorial(5)) # 阶乘函数，输出120

# 其他函数和常量，请参考官方文档查看更多用法


```

#### sys库示例



```
import sys

# 获取命令行参数列表
print(sys.argv)  # 输出当前脚本的命令行参数列表

# 获取命令行参数个数
print(len(sys.argv))  # 输出当前脚本的命令行参数个数

# 强制退出程序
# sys.exit() # 程序立即退出

# 获取Python解释器版本信息
print(sys.version)  # 输出Python解释器的版本信息字符串
print(sys.version_info)  # 输出Python解释器的版本信息元组

# 获取操作系统平台信息
print(sys.platform)  # 输出当前操作系统的平台标识符

# 获取模块搜索路径
print(sys.path)  # 输出Python解释器搜索模块的路径列表

# 获取模块的引用计数
import math
print(sys.getrefcount(math))  # 输出math模块的引用计数

# 标准输入输出
sys.stdout.write('Hello, World!\n')  # 使用标准输出打印文本
sys.stdin.readline()  # 从标准输入读取一行文本

# 执行程序时的警告设置
sys.warnoptions.append('ignore')  # 忽略所有警告信息

# 运行时异常处理
try:
    1 / 0
except:
    exc_type, exc_value, exc_traceback = sys.exc_info()  # 获取当前异常信息
    print(exc_type, exc_value)


```

#### json库示例



```
import json

# 将Python对象编码为JSON字符串
data = {
    'name': 'John',
    'age': 30,
    'city': 'New York'
}
json_str = json.dumps(data)
print(json_str)  # 输出JSON字符串: {"name": "John", "age": 30, "city": "New York"}

# 将JSON字符串解码为Python对象
json_str = '{"name": "John", "age": 30, "city": "New York"}'
data = json.loads(json_str)
print(data)  # 输出Python对象: {'name': 'John', 'age': 30, 'city': 'New York'}

# 将Python对象写入JSON文件
data = {
    'name': 'John',
    'age': 30,
    'city': 'New York'
}
with open('data.json', 'w') as f:
    json.dump(data, f)

# 从JSON文件中读取Python对象
with open('data.json', 'r') as f:
    data = json.load(f)
print(data)  # 输出Python对象: {'name': 'John', 'age': 30, 'city': 'New York'}


```

#### re库示例



```
import re

# 检查字符串是否匹配正则表达式
pattern = r"ab.\*cd"  # 正则表达式模式
text = "abcdefghi"  # 待匹配的字符串
match = re.match(pattern, text)
if match:
    print("匹配成功")
else:
    print("匹配失败")

# 在字符串中搜索匹配正则表达式的部分
pattern = r"\d+"  # 正则表达式模式，匹配一个或多个数字
text = "Hello 123 World 456"
result = re.search(pattern, text)
if result:
    print("匹配到的内容:", result.group())  # 输出: 123

# 在字符串中进行全局搜索和替换
pattern = r"apple"  # 要搜索和替换的正则表达式模式
text = "I have an apple, I like apple pie."
new_text = re.sub(pattern, "orange", text)
print(new_text)  # 输出: I have an orange, I like orange pie.

# 切分字符串
pattern = r"\s+"  # 正则表达式模式，匹配一个或多个连续的空白字符
text = "Hello World"
parts = re.split(pattern, text)
print(parts)  # 输出: ['Hello', 'World']

# 查找所有匹配的字符串
pattern = r"\w+"  # 正则表达式模式，匹配一个或多个连续的字母或数字
text = "Hello 123 World 456"
matches = re.findall(pattern, text)
print(matches)  # 输出: ['Hello', '123', 'World', '456']


```

#### csv库示例



```
import csv

# 读取CSV文件
with open('data.csv', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)

# 写入CSV文件
data = [
    ['Name', 'Age', 'City'],
    ['Alice', 25, 'New York'],
    ['Bob', 30, 'London'],
    ['Charlie', 35, 'Tokyo']
]
with open('data.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerows(data)

# 指定分隔符和引号字符
data = [
    ['Name', 'Age', 'City'],
    ['Alice, Jr.', 25, 'New York'],
    ['Bob', 30, 'London'],
    ['"Charlie, Sr."', 35, 'Tokyo']
]
# 使用with语句来确保正确关闭文件
with open('data.csv', 'w', newline='') as file:
    writer = csv.writer(file, delimiter=';', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    writer.writerows(data)


```

#### urllib库示例



```
import urllib.request

# 发送GET请求，并获取响应内容
response = urllib.request.urlopen('http://example.com')
html = response.read().decode('utf-8')
print(html)

# 发送POST请求，并传递表单数据
data = {'name': 'Alice', 'age': 25}
encoded_data = urllib.parse.urlencode(data).encode('utf-8')
response = urllib.request.urlopen('http://example.com', data=encoded_data)
html = response.read().decode('utf-8')
print(html)

# 下载文件到本地
url = 'http://example.com/images/example.jpg'
urllib.request.urlretrieve(url, 'example.jpg')

# 发送带有Headers的请求
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'
}
request = urllib.request.Request('http://example.com', headers=headers)
response = urllib.request.urlopen(request)
html = response.read().decode('utf-8')
print(html)


```

#### socket库示例



```
import socket

# 创建TCP套接字并连接到服务器
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('127.0.0.1', 8080))

# 发送数据到服务器
message = 'Hello, server!'
client_socket.send(message.encode('utf-8'))

# 接收服务器返回的数据
response = client_socket.recv(1024).decode('utf-8')
print(response)

# 关闭套接字连接
client_socket.close()

# 创建UDP套接字并发送数据
server_address = ('127.0.0.1', 8080)
message = 'Hello, server!'
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
client_socket.sendto(message.encode('utf-8'), server_address)

# 接收服务器返回的数据
response, server_address = client_socket.recvfrom(1024)
print(response.decode('utf-8'))

# 关闭套接字
client_socket.close()


```

#### logging库示例



```
import logging

# 配置日志输出的格式和级别
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

# 记录不同级别的日志信息
logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





