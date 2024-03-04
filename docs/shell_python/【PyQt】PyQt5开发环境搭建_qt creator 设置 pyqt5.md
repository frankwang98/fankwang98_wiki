







> 
> PyQt是基于python来开发Qt可视化窗口的简称，Qt本身是基于C++开发，性能较好，Qt与Python结合后，在Python的支持下可以快速地开发桌面应用程序。
> 
> 
> 




#### 文章目录


* + [1. PyQt5介绍](#1_PyQt5_3)
	+ [2. 环境安装](#2__16)
	+ [3. 开发第一个PyQt5应用](#3_PyQt5_62)
	+ [4. 配置QtDesigner](#4_QtDesigner_96)




### 1. PyQt5介绍


PyQt5的开发主要包括：



```
Qt Designer
PyQt5基本窗口控件（QMainWindow、QWidget、QLabel、QLineEdit、菜单、工具栏等）
PyQt5高级组件（QTableView、QListView、容器、多线程等）
PyQt5布局管理（QBoxLayout、QGridLayout、QFormLayout、嵌套布局等）
PyQt5信号与槽（事件处理、传递数据等）
PyQt5图形与特效（定制窗口风格、绘图、qss与UI美化、不规则窗口、设置样式等）
PyQt5扩展应用（制作安装程序、数据处理、第三方绘图库、UI自动化测试等）

```

### 2. 环境安装


环境安装包含以下部分：



```
1. Python
2. Pycharm
3. PyQt5模块

```

python的安装不用多说，在下面地址下载即可，这里我的是3.6.8。


下载地址：`https://www.python.org/downloads/windows/`


在cmd中输入python查看版本。


![在这里插入图片描述](https://img-blog.csdnimg.cn/bef83aceafa243848e87e9d35317b248.png)


不过python默认的pip安装源是国外的，为了下载更快速（用国内源），在windows下，需要在用户目录下新建pip目录，并创建pip.ini：



```
[global]
index-url = http://pypi.douban.com/simple/
[install]
trusted-host = pypi.douban.com

```

然后安装Pycharm社区版编辑器：`https://www.jetbrains.com/pycharm/download/#section=windows`


![在这里插入图片描述](https://img-blog.csdnimg.cn/18b300fa8e654eaa9d6f34284cc3bead.png)


最后安装PyQt5模块及常用工具：



```
pip install PyQt5
pip install PyQt5-tools

```

并配置环境变量：



```
D:\Python\Lib\site-packages\pyqt5_tools
D:\Python\Lib\site-packages\PyQt5\Qt5\plugins

```

在命令行中输入`import PyQt5`测试。


### 3. 开发第一个PyQt5应用


需要用到两个类：QApplication和QWidget，都在PyQt5.QtWidgets模块中。


代码如下：



```
import sys

from PyQt5.QtWidgets import QApplication,QWidget

if __name__ == '\_\_main\_\_':
    # 创建QApplication类的实例
    app = QApplication(sys.argv)
    # 创建一个窗口
    w = QWidget()
    # 设置窗口的尺寸
    w.resize(400,200)
    # 移动窗口
    w.move(300,300)

    # 设置窗口的标题
    w.setWindowTitle('第一个基于PyQt5的桌面应用')
    # 显示窗口
    w.show()

    # 进入程序的主循环、并通过exit函数确保主循环安全结束
    sys.exit(app.exec_())


```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/eb7eb1bade8d4dc9a251a7ba326ad1eb.png)


### 4. 配置QtDesigner


如果装了Qt的话，可以使用QtDesigner来创建ui界面文件，通过在python中设置外部工具引用，使得在pyqt5工程中可以打开QtDesigner，且可以通过pyuic5来将ui文件转为py文件，进行调用。


![在这里插入图片描述](https://img-blog.csdnimg.cn/d38950ca5eed4c2abb60a6cb492fe674.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/506aca4d2ded428582ecd3b49a37b494.png)


pyuic5的参数调用：`-m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py`


然后就可以在工程中使用这两个工具了。


![在这里插入图片描述](https://img-blog.csdnimg.cn/7ca76e51f91b4b4ca0c90c44daa0bd06.png)


以上。





