> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Qt环境配置与入门。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

<iframe src="//player.bilibili.com/player.html?aid=920485946&bvid=BV1Wu4y1776o&cid=1320557821&p=1" width="600" height="400" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

### 😏1. Qt介绍

`Qt`是一个跨平台的C++应用程序开发框架，被广泛用于开发图形界面和嵌入式系统应用程序。它最初由奥斯陆的一家挪威公司Trolltech（现在被Digia收购）开发，并于1995年首次发布。

`Qt Creator`是Qt官方的IDE，此外也兼容第三方扩展，如`Visual Studio、Python`。Qt可以使用纯C++开发界面和逻辑；也可以用`QML`做界面，C++做逻辑，QML效果会好一些。`Qt Quick`技术是指用QML快速开发图形界面。

Qt具有许多特性，使得其成为一个流行的开发框架：

> 1.跨平台支持：Qt可以在各种操作系统上运行，包括Windows、MacOS、Linux、Android和iOS等。并且Qt应用程序在不同平台运行时的外观和行为都相同，这大大提高了开发效率和用户体验。

> 2.应用程序开发：Qt提供了丰富的库和工具，用于开发各种应用程序，从简单的命令行工具到复杂的图形用户界面应用程序。

> 3.图形用户界面设计：Qt拥有强大的界面设计工具Qt Designer，可用于创建漂亮的用户界面。它还支持自定义样式表和主题，以及无缝集成SVG图形等。

> 4.数据库访问：Qt提供了名为Qt SQL的模块，用于访问各种关系型数据库。它可轻松地连接到多个数据库，如MySQL、Oracle和SQLite等。

> 5.网络编程：Qt网络模块提供了一组高级API，用于开发基于TCP、UDP和HTTP协议的网络应用程序。

> 6.多语言支持：Qt提供了强大的多语言支持，包括Unicode和本地化字符串等。这使得开发者可以轻松地编写跨国界面并支持多种语言。

Qt是一个功能强大，易于使用且具有跨平台特性的应用程序开发框架。它被视为开发图形用户界面和嵌入式系统应用程序的首选框架之一。主要用于嵌入式和桌面的图形界面开发，具体有`基本界面、二维绘图、三维绘图、音视频、网络通信、文件管理`等。

### 😊2. Qt环境配置

国内下载Qt可以从镜像网站下载，常用的几个网站是：

```bash
中国科学技术大学：http://mirrors.ustc.edu.cn/qtproject/
清华大学：https://mirrors.tuna.tsinghua.edu.cn/qt/
北京理工大学：http://mirror.bit.edu.cn/qtproject/
中国互联网络信息中心：http://mirror.bit.edu.cn/qtproject/

```

可以下载多平台的版本，例如Windows的`exe`、Ubuntu的`run`，运行安装即可，选择需要的模块。目前更新到Qt6，Qt5.15之后就只能在线安装了。

另外，在Ubuntu也可以直接命令行安装：`sudo apt install qt5-default`

对应版本是5.9.5稳定版，可通过`qmake -v`查看。

### 😆3. Qt入门示例

Qt安装完成后，自带丰富的`example`，可以学习。

可以自己新建一个项目，了解Qt的基本开发流程。

新建Qt工程时，Qt的Application有多个应用程序的创建模板，我们先了解以下两种：

1. Qt Widgets Application，支持桌面平台的有图形用户界面的应用程序。GUI 的设计完全基于 C++ 语言，采用 Qt 提供的一套 C++ 类库。
2. Qt Console Application，控制台应用程序，无 GUI 界面，一般用于学习 C/C++ 语言，只需要简单的输入输出操作时可创建此类项目。

Qt有3种基类：

1. QMainWindow 是主窗口类，主窗口具有主菜单栏、工具栏和状态栏，类似于一般的应用程序的主窗口；
2. QWidget 是所有具有可视界面类的基类，选择 QWidget 创建的界面对各种界面组件都可以支持；
3. QDialog 是对话框类，可建立一个基于对话框的界面。

可以直接打开mainwindow.ui来到`Designer`设计模式，同VB这类图形化编程语言类似。然后添加一个Label控件，可以添加文字，更改大小，基本上和VB的操作模式一样。也可以在代码中直接生成图形控件。

程序生成控件的代码示例如下：

```cpp
#include "mainwindow.h"
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QLabel *label = new QLabel("Hello Qt!");
    label->show();
    return app.exec();
}

```

Qt的编译工具默认是`qmake`，Qt6之后编译工具改为了`cmake`，两者各有优势，需要学会去使用。

Ubuntu可以在命令行直接生成pro工程文件，较为方便：

```sh
qmake -project
qmake xxx.pro
make	# 编译

```

Windows Qt程序的构建快捷键`Ctrl+B`，运行快捷键`Ctrl+R`，可以在构建和运行设置中`自定义构建的目录`。

### 😆4. Qt信号槽机制

信号槽机制是一种用于在对象之间进行通信的机制。它是Qt框架的核心特性之一，使得在事件发生时能够自动触发相应的操作，从而实现对象之间的解耦和灵活的交互。底层基于libuv库，以实现高性能的事件驱动和非阻塞I/O操作。

信号槽的连接有多种方式：

1. SIGNAL/SLOT
2. 函数地址
3. UI界面-转到槽
4. UI界面-信号槽编辑器
5. lambda表达式

此外，还有多种定义和使用信号槽的方式，如连接重载的信号和槽，可以用函数指针的方式，让编译器自动推断有参和无参函数。

```cpp
    // 定义函数指针，编译器自动推断，有参和无参
    void (Commander::*pGo)() = &Commander::go;
    void (Soldier::*pFight)() = &Soldier::fight;
    connect(&commander, pGo, &soldier, pFight);
    commander.go();
    void (Commander::*pGoForFreedom)(QString) = &Commander::go;
    void (Soldier::*pFightForFreedom)(QString) = &Soldier::fight;
    connect(&commander, pGoForFreedom, &soldier, pFightForFreedom);
    commander.go("for freedom!");

```

还有一个信号连接多个槽，多个信号连接一个槽，信号连接信号，断开连接等其他应用，可以在项目中合理使用。

### 😆5. Qt控件示例

示例代码：
```cpp
QPushButton按钮
//创建按钮
button = new QPushButton("按钮 A",this);
//定义按钮 X 轴,Y 轴,W 宽,H 高
button->setGeometry(QRect(100,100,100,25));
//给按钮添加插槽事件(点击事件)
connect(button,SIGNAL(released()),this,SLOT(txtButton()));

QLabel标签
//实例 QLabel 控件
label = new QLabel("我是 QLabel",this);
//QLabel 位置
//label->move(100,100);
label->setGeometry(QRect(100,100,200,30));
//label 样式(CSS 样式表)
//font-size 字号
//color 字体颜色
//font-weight 字宽
//font-style 字体样式
label->setStyleSheet("font-size:20px;color:red;font-weight:bold;font-style:italic");

QLineEdit 单行文本
//创建 QLineEdit 控件
lineEdit = new QLineEdit(this);
//位置大小
lineEdit->setGeometry(QRect(100,100,200,25));
//样式
//border 边框线大小
//border-style 边框样式 solid 实线
//border-color:blue red 上下蓝色 左右红色 
lineEdit->setStyleSheet("border:1px;border-style:solid;color:red;border-color: blue red;");
//限制最长输入 12 位
lineEdit->setMaxLength(12);
//不可写入
//lineEdit->setEchoMode(QLineEdit::NoEcho);
//密码*号输入
lineEdit->setEchoMode(QLineEdit::Password);

QTextEdit 多行文本
//实例 QTextEdit 控件
textEdit = new QTextEdit(this);
//控件位置大小
textEdit->setGeometry(QRect(50,50,200,150));
//内容
textEdit->setText("我是第一行<br/>我是第二行");

QPlainTextEdit 多行文本
//实例
plainTextEdit = new QPlainTextEdit(this);
//位置
plainTextEdit->setGeometry(QRect(50,50,200,100));
//添加内容
plainTextEdit->setPlainText("第一行");

QComboBox 下拉列表框
首先在头文件引用控件类#include <QComboBox>
//实例 QComboBox
QComboBox *comboBox = new QComboBox(this);
//控件显示位置大小
comboBox->setGeometry(QRect(50,50,120,25));
//定义字符串列表
QStringList str;
str << "数学" << "语文" << "地理";
//将字符串列表绑定 QComboBox 控件
comboBox->addItems(str);

QFontComboBox 字体下拉列表框
//实例 QFontComboBox
fontComboBox = new QFontComboBox(this);
//实例 QPushButton
button = new QPushButton(this);
//实例 QLabel
label = new QLabel(this);
label->setGeometry(QRect(50,150,300,25));
//按钮名
button->setText("按钮");
//位置
button->move(180,50);
//事件
connect(button,SIGNAL(released()),this,SLOT(txtButton()));
//QFontComboBox 控件位置
fontComboBox->setGeometry(QRect(50,50,120,25));

QSpinBox 范围控件
//实例 QSpinBox
spinBox = new QSpinBox(this);
//位置
spinBox->setGeometry(QRect(50,50,100,25));
//值范围
spinBox->setRange(0,200);
//初始值
spinBox->setValue(10);
//后缀
spinBox->setSuffix("元");
//前缀
spinBox->setPrefix("$");

QTimeEdit 时间控件
#include <QDateTime>
//实例
timeEdit = new QTimeEdit(this);
//位置
timeEdit->setGeometry(QRect(50,50,120,25));
//获取系统时间
QDateTime sysTime = QDateTime::currentDateTime();
//获取时分秒以“：”号拆分赋予 list 数组
QStringList list = sysTime.toString("hh:mm:ss").split(':');
//将时分秒绑定控件 
timeEdit->setTime(QTime(list[0].toInt(),list[1].toInt(),list[2].toInt()));

QDateEdit 日期控件
#include <QDateTime>
//实例
dateEdit = new QDateEdit(this);
//位置
dateEdit->setGeometry(QRect(50,50,120,25));
//获取系统时间
QDateTime sysTime = QDateTime::currentDateTime();
//获取时分秒以“-”号拆分赋予 list 数组
QStringList list = sysTime.toString("yyyy-MM-dd").split('-');
//将年月日绑定控件 
dateEdit->setDate(QDate(list[0].toInt(),list[1].toInt(),list[2].toInt()));

QScrollBar 进度条控件
//实例
scrollBar = new QScrollBar(this);
spinBox = new QSpinBox(this);
//横显/竖显
scrollBar->setOrientation(Qt::Horizontal);
//位置
scrollBar->setGeometry(QRect(50,50,180,20));
spinBox->setGeometry(QRect(50,90,100,25));
//控制条宽度
scrollBar->setPageStep(10);
//scrollBar 事件
connect(scrollBar,SIGNAL(valueChanged(int)),spinBox,SLOT(setValue(int)));
//spinBox 事件
connect(spinBox,SIGNAL(valueChanged(int)),scrollBar,SLOT(setValue(int)));
//初始值
scrollBar->setValue(50);

QRadioButton 单选按钮
//实例
radioM = new QRadioButton(this);
radioW = new QRadioButton(this);
label = new QLabel(this);
//位置
radioM->setGeometry(QRect(50,50,50,50));
radioW->setGeometry(QRect(100,50,50,50));
label->setGeometry(QRect(50,100,100,25));
radioM->setText("男");
radioW->setText("女");
//默认选择
radioM->setChecked(true);
label->setText("男");
//事件
connect(radioM,SIGNAL(clicked()),this,SLOT(radioChange()));
connect(radioW,SIGNAL(clicked()),this,SLOT(radioChange()));

QCheckBox 复选框
//实例化 checkBox 控件
checkBox01 = new QCheckBox(this);
checkBox02 = new QCheckBox(this);
checkBox03 = new QCheckBox(this);
//实例 label 控件显示结果
label = new QLabel(this);
//控件位置
checkBox01->setGeometry(QRect(50,50,50,50));
checkBox02->setGeometry(QRect(100,50,50,50));
checkBox03->setGeometry(QRect(150,50,50,50));
label->setGeometry(QRect(50,100,200,30));
//控件值
checkBox01->setText("数学");
checkBox02->setText("语文");
checkBox03->setText("地理");
//checkbox 控件点击事件
connect(checkBox01, SIGNAL(clicked(bool)), this, SLOT(checkChange()));
connect(checkBox02, SIGNAL(clicked(bool)), this, SLOT(checkChange()));
connect(checkBox03, SIGNAL(clicked(bool)), this, SLOT(checkChange()));

QListView 列表控件(数据模型类)
//实例 listView 控件
listView = new QListView(this);
//定义位置宽高
listView->setGeometry(QRect(50,50,100,100));
//StringList 数组
QStringList string;
string << "数学" << "语文" << "外语" <<"地理";
//添加数据
model = new QStringListModel(string);
//将数据绑定 listView 控件
listView-> setModel(model);

QTreeView 树控件
//实例 QTreeView 控件
treeView = new QTreeView(this);
//位置
treeView->setGeometry(QRect(50,50,150,200));
//实例数据类型 4 个节点，2 列
model = new QStandardItemModel(3,2);
//列名称
model-> setHeaderData(0,Qt::Horizontal,"第一列");
model-> setHeaderData(1,Qt::Horizontal,"第二列");
//定义节点
QStandardItem *item1 = new QStandardItem("数学");
item1->setIcon(QIcon(":/new/prefix1/folder"));
QStandardItem *item2 = new QStandardItem("语文");
item2->setIcon(QIcon(":/new/prefix1/folder"));
QStandardItem *item3 = new QStandardItem("外语");
item3->setIcon(QIcon(":/new/prefix1/folder"));
//外语子项
QStandardItem *item4 = new QStandardItem("外语 A");
item4->setIcon(QIcon(":/new/prefix1/file"));
item3->appendRow(item4);
//将节点添加至 QStandardItemModel
model->setItem(0,0,item1);
model->setItem(1,0,item2);
model->setItem(2,0,item3);
//将 QStandardItemModel 数据绑定 QTreeView 控件
treeView-> setModel(model);

QTableView 表格控件(数据模型定义)
//实例
tableView = new QTableView(this);
//位置
tableView->setGeometry(QRect(50,50,310,200));
//实例数据模型
model = new QStandardItemModel();
//定义列
model->setHorizontalHeaderItem(0,new QStandardItem("数学"));
model->setHorizontalHeaderItem(1,new QStandardItem("语文"));
model->setHorizontalHeaderItem(2,new QStandardItem("外语"));
//行数据 0 行,0 列
model->setItem(0,0,new QStandardItem("数学 A"));
model->setItem(0,1,new QStandardItem("语文 A"));
model->setItem(0,2,new QStandardItem("外语 A"));
model->setItem(1,0,new QStandardItem("数学 B"));
model->setItem(1,1,new QStandardItem("语文 B"));
model->setItem(1,2,new QStandardItem("外语 B"));
//将数据模型绑定控件
tableView-> setModel(model);

QMenu、QToolBar 控件
#include <QMessageBox>
//实例菜单
fileMenu = new QMenu(this);
editMenu = new QMenu(this);
helpMenu = new QMenu(this);
//填充菜单子节点
newAct = new QAction(QIcon( ":/images/new" ), tr( "新建" ), this );
newAct->setShortcut(tr("Ctrl+N" ));
newAct->setStatusTip(tr("新建文件" ));
connect(newAct, SIGNAL(triggered()), this, SLOT(newFile()));
cutAct = new QAction(QIcon( ":/images/cut" ), tr( "剪切" ), this );
cutAct->setShortcut(tr("Ctrl+X" ));
cutAct->setStatusTip(tr("剪切内容" ));
copyAct = new QAction(QIcon( ":/images/copy" ), tr( "复制" ), this );
copyAct->setShortcut(tr("Ctrl+C" ));
copyAct->setStatusTip(tr("复制内容" ));
pasteAct = new QAction(QIcon( ":/images/paste" ), tr( "粘贴" ), this );
pasteAct->setShortcut(tr("Ctrl+V" ));
pasteAct->setStatusTip(tr("粘贴内容" ));
aboutQtAct = new QAction(tr( "关于 Qt" ), this );
aboutQtAct->setStatusTip(tr("关于 Qt 信息" ));
connect(aboutQtAct, SIGNAL(triggered()), qApp, SLOT(aboutQt()));
//填充菜单
fileMenu = menuBar()->addMenu(tr( "文件" ));
fileMenu->addAction(newAct);
fileMenu->addSeparator();
editMenu = menuBar()->addMenu(tr("编辑" ));
editMenu->addAction(cutAct);
editMenu->addAction(copyAct);
editMenu->addAction(pasteAct);
menuBar()->addSeparator();
helpMenu = menuBar()->addMenu(tr("帮助" ));
helpMenu->addAction(aboutQtAct);
//toolBar 工具条
fileToolBar = addToolBar(tr("新建"));
fileToolBar->addAction(newAct);
editToolBar = addToolBar(tr("修改"));
editToolBar->addAction(cutAct);
editToolBar->addAction(copyAct);
editToolBar->addAction(pasteAct);
//子菜单事件
void MainWindow::newFile(){
QMessageBox::warning(this,tr("Warning"),tr("创建新文件？"),
QMessageBox::Yes | QMessageBox::Default,
QMessageBox::No);
}
```

组件可以看作是前面控件的集成，用来实现模块化功能。

日历组件

登陆窗口（widget）

文件浏览对话框

颜色选择对话框

进度条实例

Timer 实时更新时间
```cpp
#include <QDateTime>
//实例 QLabel 控件
label = new QLabel(this);
//位置
label->setGeometry(QRect(50,50,200,25));
//实例 QTimer 控件
timer = new QTimer;
//timer 时间
connect(timer,SIGNAL(timeout()),this,SLOT(timerTime()));
//执行
timer->start(1000);
void MainWindow::timerTime(){
	//获取系统时间
	QDateTime sysTime = QDateTime::currentDateTime();
	label->setText(sysTime.toString());
}
```

还有图像、图表、多媒体、网络、数据库等操作。

### 6.Qt开发经验摘抄
来自gitee飞扬青云的Qt开发经验学习摘抄！！！

1. 编译出现错误时，从第一个开始解决，后面的错误可能是前面引起的。

2. 定时器是个好东西，如果你的启动界面很卡，可以给初始就加载的东西分分流，比如异步执行或延时执行。

	```cpp
	//异步执行load函数
	QMetaObject::invokeMethod(this, "load", Qt::QueuedConnection);
	//延时10毫秒执行load函数
	QTimer::singleShot(10, this, SLOT(load()));
	```

3. QtCreator默认是单线程编译，类似的，可以在设置kit时-j后面接的是电脑的核心数。

4. 很多时候找到Qt对应封装的方法后，记得多看看该函数的重载，多个参数的，你会发现不一样的世界，有时候会恍然大悟，原来Qt已经帮我们封装好了，比如QString、QColor的重载参数极其丰富，很多你做梦都想要的功能就在里面。

5. 可以在pro文件中写上版本号、程序图标、产品名称、版权所有、文件说明等信息（Qt5才支持），其实在windows上就是qmake的时候会自动将此信息转换成rc文件。对于早期的Qt4版本你可以手动写rc文件实现。

	```cpp
	程序版本
	VERSION  = 2025.10.01
	程序图标
	RC_ICONS = main.ico
	产品名称
	QMAKE_TARGET_PRODUCT = quc
	版权所有
	QMAKE_TARGET_COPYRIGHT = feiyangqingyun
	文件说明
	QMAKE_TARGET_DESCRIPTION = QQ: 517216493  WX: feiyangqingyun
	```

6. 绘制平铺背景QPainter::drawTiledPixmap，绘制圆角矩形QPainter::drawRoundedRect()，而不是QPainter::drawRoundRect()，这两个函数非常容易搞混。

7. Qt内置图标封装在QStyle中，大概七十多个图标，可以直接拿来用。

	```cpp
	SP_TitleBarMenuButton,
	SP_TitleBarMinButton,
	SP_TitleBarMaxButton,
	SP_TitleBarCloseButton,
	SP_MessageBoxInformation,
	SP_MessageBoxWarning,
	SP_MessageBoxCritical,
	SP_MessageBoxQuestion,
	...
	//下面这样取出来使用就行
	QPixmap pixmap = this->style()->standardPixmap(QStyle::SP_TitleBarMenuButton);
	ui->label->setPixmap(pixmap);
	```

8. 对QLCDNumber控件设置样式，需要将QLCDNumber的segmentstyle设置为flat，不然你会发现没效果。

9. 为了美化界面，Qt每个控件都可以设置样式！在开发时, 无论是出于维护的便捷性, 还是节省内存资源的考虑, 都应该有一个 qss 文件来存放所有的样式表, 而不应该将 setStyleSheet 写的到处都是。如果是初学阶段或者测试阶段可以直接UI上右键设置样式表，正式项目还是建议统一到一个qss样式表文件比较好，统一管理。

10. 如果用了webengine模块，发布程序的时候带上QtWebEngineProcess.exe、translations文件夹、resources文件夹，不然无法正常运行。

11. 可以指定位置设置背景图片。

	```cpp
	QMainWindow > .QWidget {
	    background-color: gainsboro;
	    background-image: url(:/images/xxoo.png);
	    background-position: top right;
	    background-repeat: no-repeat
	}
	```

12. 如果发现QtCreator中的构建套件不正常了或者坏了（比如不能正确识别环境中的qmake或者编译器、打开项目不能正常生成影子构建目录），请找到两个目录（C:\Users\Administrator\AppData\Local\QtProject、C:\Users\Administrator\AppData\Roaming\QtProject）删除即可，删除后重新打开QtCreator进行构建套件的配置就行。

13. qml播放视频在linux需要安装 sudo apt-get install libpulse-dev。

14. QtChart模块从Qt5.7开始自带，最低编译要求Qt5.4。在安装的时候记得勾选，默认不勾选。使用该模块需要引入命名空间。
	
	```cpp
	#include <QChartView>
	QT_CHARTS_USE_NAMESPACE
	class CustomChart : public QChartView
	```

15. 非常不建议tr中包含中文，尽管现在的新版Qt支持中文到其他语言的翻译，但是很不规范，也不知道TMD是谁教的（后面发现我在刚学Qt的时候也发布了一些demo到网上也是tr包含中文的，当时就狠狠的打了自己一巴掌），tr的本意是包含英文，然后翻译到其他语言比如中文，现在大量的初学者滥用tr，如果没有翻译的需求，禁用tr，tr需要开销的，Qt默认会认为他需要翻译，会额外进行特殊处理。

16. ！！！超过两处相同处理的代码，建议单独写成函数。代码尽量规范精简，比如 if(a == 123) 要写成 if (123 == a)，值在前面，再比如 if (ok == true) 要写成 if (ok)，if (ok == false) 要写成 if (!ok)等。

17. Qt打包发布，有很多办法，Qt5以后提供了打包工具windeployqt（linux上为linuxdeployqt，mac上为macdeployqt）可以很方便的将应用程序打包，使用下来发现也不是万能的，有时候会多打包一些没有依赖的文件，有时候又会忘记打包一些插件尤其是用了qml的情况下，而且不能识别第三方库，比如程序依赖ffmpeg，则对应的库需要自行拷贝，终极大法就是将你的可执行文件复制到Qt安装目录下的bin目录，然后整个一起打包，挨个删除不大可能依赖的组件，直到删到正常运行为止。

18. Qt封装的QDateTime日期时间类非常强大，可以字符串和日期时间相互转换，也可以毫秒数和日期时间相互转换，还可以1970经过的秒数和日期时间相互转换等。

	```cpp
	QDateTime dateTime;
	QString dateTime_str = dateTime.currentDateTime().toString("yyyy-MM-dd hh:mm:ss");
	//从字符串转换为毫秒（需完整的年月日时分秒）
	datetime.fromString("2011-09-10 12:07:50:541", "yyyy-MM-dd hh:mm:ss:zzz").toMSecsSinceEpoch();
	//从字符串转换为秒（需完整的年月日时分秒）
	datetime.fromString("2011-09-10 12:07:50:541", "yyyy-MM-dd hh:mm:ss:zzz").toTime_t();
	//从毫秒转换到年月日时分秒
	datetime.fromMSecsSinceEpoch(1315193829218).toString("yyyy-MM-dd hh:mm:ss:zzz");
	//从秒转换到年月日时分秒（若有zzz，则为000）
	datetime.fromTime_t(1315193829).toString("yyyy-MM-dd hh:mm:ss[:zzz]");
	```

19. 在我们使用QList、QStringList、QByteArray等链表或者数组的过程中，如果只需要取值，而不是赋值，强烈建议使用 at() 取值而不是 [] 操作符，在官方书籍《C++ GUI Qt 4编程（第二版）》的书中有特别的强调说明，此教材的原作者据说是Qt开发的核心人员编写的，所以还是比较权威，至于使用 at() 与使用 [] 操作符速度效率的比较，网上也有网友做过此类对比。原文在书的212页，这样描述的：Qt对所有的容器和许多其他类都使用隐含共享，隐含共享是Qt对不希望修改的数据决不进行复制的保证，为了使隐含共享的作用发挥得最好，可以采用两个新的编程习惯。第一种习惯是对于一个（非常量的）向量或者列表进行只读存取时，使用 at() 函数而不用 [] 操作符，因为Qt的容器类不能辨别 [] 操作符是否将出现在一个赋值的左边还是右边，他假设最坏的情况出现并且强制执行深层赋值，而 at() 函数则不被允许出现在一个赋值的左边。

20. 心中有坐标，万物皆painter，强烈建议在学习自定义控件绘制的时候，将qpainter.h头文件中的函数全部看一遍、试一遍、理解一遍，这里边包含了所有Qt内置的绘制的接口，对应的参数都试一遍，你会发现很多新大陆，会一定程度上激发你的绘制的兴趣，犹如神笔马良一般，策马崩腾遨游代码绘制的世界。

21. 在使用setItemWidget或者setCellWidget的过程中，有时候会发现设置的控件没有居中显示而是默认的左对齐，而且不会自动拉伸填充，对于追求完美的程序员来说，这个可不大好看，有个终极通用办法就是，将这个控件放到一个widget的布局中，然后将widget添加到item中，这样就完美解决了，而且这样可以组合多个控件产生复杂的控件。

22. QTextEdit右键菜单默认英文的，如果想要中文显示，加载widgets.qm文件即可，一个Qt程序中可以安装多个翻译文件，不冲突。

23. 理论上串口和网络收发数据都是默认异步的，操作系统自动调度，完全不会卡住界面，网上那些说收发数据卡住界面主线程的都是扯几把蛋，真正的耗时是在运算以及运算后的处理，而不是收发数据，在一些小数据量运算处理的项目中，一般不建议动用线程去处理，线程需要调度开销的，不要什么东西都往线程里边扔，线程不是万能的。只有当真正需要将一些很耗时的操作比如编码解码等，才需要移到线程处理。

24. 用QFile读写文件的时候，推荐用QTextStream文件流的方式来读写文件，速度快很多，基本上会有30%的提升，文件越大性能区别越大。

25. 很多初学者甚至几年工作经验的人，对多线程有很深的误解和滥用，尤其是在串口和网络通信这块，什么都往多线程里面丢，一旦遇到界面卡，就把数据收发啥的都搞到多线程里面去，殊不知绝大部分时候那根本没啥用，因为没找到出问题的根源。
	- 如果你没有使用wait***函数的话，大部分的界面卡都出在数据处理和展示中，比如传过来的是一张图片的数据，你需要将这些数据转成图片，这个肯定是耗时的；
	- 还有就是就收到的数据曲线绘制出来，如果过于频繁或者间隔过短，肯定会给UI造成很大的压力的，最好的办法是解决如何不要频繁绘制UI比如合并数据一起绘制等；
	- 如果是因为绘制UI造成的卡，那多线程也是没啥用的，因为UI只能在主线程；
	- 串口和网络的数据收发默认都是异步的，由操作系统调度的，如果数据处理复杂而且数据量大，你要做的是将数据处理放到多线程中；
	- 如果没有严格的数据同步需求，根本不需要调用wait***之类的函数来立即发送和接收数据，实际需求中大部分的应用场景其实异步收发数据就足够了；
	- 有严格数据同步需求的场景还是放到多线程会好一些，不然你wait***就卡在那边了；
	- 多线程是需要占用系统资源的，理论上来说，如果线程数量超过了CPU的核心数量，其实多线程调度可能花费的时间更多，各位在使用过程中要权衡利弊；
	- 再次强调，不要指望Qt的网络通信支持高并发，最多到1000个能正常工作就万事大吉，一般建议500以内的连接数。有大量高并发的需求请用第三方库比如swoole等。

26. Qt5.15版本开始官方不再提供安装包，只提供源码，可以自行编译或者在线安装，估计每次编译各种版本太麻烦，更多的是为了统计收集用户使用信息比如通过在线安装，后期可能会逐步加大商业化力度。

27. 关于Qt中文乱码的问题，个人也稍微总结了一点，应该可以解决99%以上的Qt版本的乱码问题。
	- 第一步：代码文件选择用utf8编码带bom。
	- 第二步：在有中文汉字的代码文件顶部加一行（一般是cpp文件） #pragma execution_character_set("utf-8") 可以考虑放在head.h中，然后需要的地方就引入head头文件就行，而不是这行代码写的到处都是；这行代码是为了告诉msvc编译器当前代码文件用utf8去编译。
	- 第三步：main函数中加入设置编码的代码，以便兼容Qt4，如果没有Qt4的场景可以不用，从Qt5开始默认就是utf8编码。
	
28. 关于Qt众多版本（至少几百个）都不兼容的问题，在经过和Qt中国的林斌大神和其他大神（Qt非官方技术交流群）头脑风暴以后，最终得出以下的结论。
	- Qt在二进制兼容这块，已经做了最大的努力，通过将各种代码细节隐藏，Q指针+D指针技巧，尽量保持了接口的统一；
	- 是否兼容最主要考虑编译器的因素，毕竟任何Qt版本都是需要通过编译器编译成对应的二进制文件，由他说了算。如果两个Qt版本采用的编译器版本一样，极大概率可执行文件是兼容的，比如 Qt5.10+msvc2015 32 位 和 Qt5.11+msvc2015 32位 编译出来的可执行文件，都用Qt5.11的库是可行的；
	- mingw编译器的Qt版本也是如此，就是因为Qt官方安装包集成的mingw编译器一直在更新（极少附近版本没有更新mingw编译器版本除外），比如5.7用的mingw53，5.12用的mingw73，5.15用的mingw81，因为带的Qt库也是这个编译器编译出来的，所以导致看起来全部不兼容；
	- 如果想要完全兼容，还有一个注意要素，那就是对应代码使用的类的头文件接口是否变了，按道理原有的接口极少会变，一般都是新增加，或者大版本才会改变，比如Qt4-Qt5-Qt6这种肯定没法兼容的，接口和模块都变了；
	- 大胆的猜测：如果Qt5.6到Qt5.15你全部用一种编译器比如mingw73或者msvc2015重新编译生成对应的Qt运行库，然后在此基础上开发程序，最后生成的可执行文件用Qt5.15的库是都可以的，这样就轻松跨越了多个版本兼容；
	- 大胆的建议：在附近的几个版本统一编译器，比如5.6-5.12之间就统一用mingw53或者msvc2015,5.12-5.15统一用msvc2017，要尝鲜其他编译器的可以自行源码编译其他版本，这样最起码附近的一大段版本（大概2-3年的版本周期）默认就兼容了。
	- 本人测试的是widget部分，qml未做测试，不清楚是否机制一样；

29. Qt中自带的很多控件，其实都是由一堆基础控件（QLabel、QPushButton等）组成的，比如日历面板 QCalendarWidget 就是 QToolButton+QSpinBox+QTableView 等组成，妙用 findChildren 可以拿到父类对应的子控件集合，可以直接对封装的控件中的子控件进行样式的设置，其他参数的设置比如设置中文文本（默认可能是英文）等。

30. 关于网络通信，tcp和udp是两种不同的底层的网络通信协议，两者监听和通信的端口互不相干的，不同的协议或者不同的网卡IP地址可以用相同的端口。之前有个人说他的电脑居然可以监听一样的端口进行通信，颠覆了他以前的认知，书上说的明明是不可以相同端口的，后面远程一看原来选择的不同的网卡IP地址，当然可以的咯。
	- tcp对网卡1监听了端口6000，还可以对网卡2监听端口6000。
	- tcp对网卡1监听了端口6000，udp对网卡1还可以继续监听端口6000。
	- tcp对网卡1监听了端口6000，在网卡1上其他tcp只能监听6000以外的端口。
	- udp协议也是上面的逻辑。

31. 开源的图表控件QCustomPlot很经典，在曲线数据展示这块性能彪悍，总结了一些容易忽略的经验要点。
	- 可以将XY轴对调，然后形成横向的效果，无论是曲线图还是柱状图，分组图、堆积图等，都支持这个特性。
	- 不需要的提示图例可以调用 legend->removeItem 进行移除。
	- 两条曲线可以调用 setChannelFillGraph 设置合并为一个面积区域。
	- 可以关闭抗锯齿 setAntialiased 加快绘制速度。
	- 可以设置不同的线条样式（setLineStyle）、数据样式（setScatterStyle）。
	- 坐标轴的箭头样式可更换 setUpperEnding。
	- 可以用 QCPBarsGroup 实现柱状分组图，这个类在官方demo中没有，所以非常容易忽略。

	```cpp
	//对调XY轴，在最前面设置
	QCPAxis *yAxis = customPlot->yAxis;
	QCPAxis *xAxis = customPlot->xAxis;
	customPlot->xAxis = yAxis;
	customPlot->yAxis = xAxis;
	
	//移除图例
	customPlot->legend->removeItem(1);
	
	//合并两个曲线画布形成封闭区域
	customPlot->graph(0)->setChannelFillGraph(customPlot->graph(1));
	
	//关闭抗锯齿以及设置拖动的时候不启用抗锯齿
	customPlot->graph()->setAntialiased(false);
	customPlot->setNoAntialiasingOnDrag(true);
	
	//多种设置数据的方法
	customPlot->graph(0)->setData();
	customPlot->graph(0)->data()->set();
	
	//设置不同的线条样式、数据样式
	customPlot->graph()->setLineStyle(QCPGraph::lsLine);
	customPlot->graph()->setScatterStyle(QCPScatterStyle::ssDot);
	customPlot->graph()->setScatterStyle(QCPScatterStyle(shapes.at(i), 10));
	
	//还可以设置为图片或者自定义形状
	customPlot->graph()->setScatterStyle(QCPScatterStyle(QPixmap("./sun.png")));
	QPainterPath customScatterPath;
	for (int i = 0; i < 3; ++i) {
	    customScatterPath.cubicTo(qCos(2 * M_PI * i / 3.0) * 9, qSin(2 * M_PI * i / 3.0) * 9, qCos(2 * M_PI * (i + 0.9) / 3.0) * 9, qSin(2 * M_PI * (i + 0.9) / 3.0) * 9, 0, 0);
	}
	customPlot->graph()->setScatterStyle(QCPScatterStyle(customScatterPath, QPen(Qt::black, 0), QColor(40, 70, 255, 50), 10));
	
	//更换坐标轴的箭头样式
	customPlot->xAxis->setUpperEnding(QCPLineEnding::esSpikeArrow);
	customPlot->yAxis->setUpperEnding(QCPLineEnding::esSpikeArrow);
	
	//设置背景图片
	customPlot->axisRect()->setBackground(QPixmap("./solarpanels.jpg"));
	//画布也可以设置背景图片
	customPlot->graph(0)->setBrush(QBrush(QPixmap("./balboa.jpg")));
	//整体可以设置填充颜色或者图片
	customPlot->setBackground(QBrush(gradient));
	//设置零点线条颜色
	customPlot->xAxis->grid()->setZeroLinePen(Qt::NoPen);
	//控制是否鼠标滚轮缩放拖动等交互形式
	customPlot->setInteractions(QCP::iRangeDrag | QCP::iRangeZoom | QCP::iSelectPlottables);
	
	//柱状分组图
	QCPBarsGroup *group = new QCPBarsGroup(customPlot);
	QList<QCPBars*> bars;
	bars << fossil << nuclear << regen;
	foreach (QCPBars *bar, bars) {
	    //设置柱状图的宽度大小
	    bar->setWidth(bar->width() / bars.size());
	    group->append(bar);
	}
	//设置分组之间的间隔
	group->setSpacing(2);
	```

32. Qt天生就是linux的，从linux开始发展起来的，所以不少Qt程序员经常的开发环境是linux，比如常用的ubuntu等系统，整理了一点常用的linux命令。

	|命令|功能|
	|:------|:------|
	|sudo -s|切换到管理员，如果是 sudo -i 切换后会改变当前目录。|
	|apt install g++| 安装软件包（要管理员权限），另一个派系的是 yum install。|
	|cd /home|进入home目录。|
	|ls|罗列当前所在目录所有目录和文件。|
	|ifconfig|查看网卡信息包括IP地址，windows上是 ipconfig。|
	|tar -zxvf bin.tar.gz|解压文件到当前目录。|
	|tar -jxvf bin.tar.xz|解压文件到当前目录。|
	|tar -zxvf bin.tar.gz -C /home|解压文件到/home目录，记住是大写的C。|
	|tar -zcvf bin.tar.gz bin|将bin目录压缩成tar.gz格式文件（压缩比一般）。|
	|tar -jcvf bin.tar.xz bin|将bin目录压缩成tar.xz格式文件（压缩比高，推荐）。|
	|tar -... |j z 表示不同的压缩方法，x表示解压，c表示压缩。|
	|gedit 1.txt|用记事本打开文本文件。|
	|vim 1.txt |用vim打开文件，很多时候可以缩写用vi。|
	|./configure  make -j4  make install|通用编译源码命令，第一步./configure执行配置脚本，第二步make -j4启用多线程编译，第三步make install安装编译好的文件。|
	|./configure -prefix /home/liu/Qt-5.9.3-static -static -sql-sqlite -qt-zlib -qt-xcb -qt-libpng -qt-libjpeg -fontconfig -system-freetype -iconv -nomake tests -nomake examples -skip qt3d -skip qtdoc |Qt通用编译命令。|
	|./configure -static -release -fontconfig -system-freetype -qt-xcb -qt-sql-sqlite -qt-zlib -qt-libpng -qt-libjpeg -nomake tests -nomake examples -prefix /home/liu/qt/Qt5.6.3| Qt静态带中文。|
	|./configure -prefix /home/liu/Qt-5.9.3-static -static -release -nomake examples -nomake tests -skip qt3d|精简编译命令。|
	|./configure --prefix=host --enable-static --disable-shared --disable-doc|ffmpeg编译命令。|

33. 随着国产化的兴起，各种国产系统和国产数据库等逐渐进入开发者的世界，罗列几个要点。
	- 中标麒麟neokylin基于centos。
	- 银河麒麟kylin早期版本比如V2基于freebsd，新版本V4、V10基于ubuntu。
	- 优麒麟ubuntukylin就是ubuntu的汉化版本，加了点农历控件啥的。
	- deepin基于debian。
	- uos基于deepin或者说是deepin的商业分支。
	- ubuntu基于debian。
	- linux界主要分两种发行版本，debian（ubuntu、deepin、uos、银河麒麟kylin等）和redhat（fedora、centos、中标麒麟neokylin、中兴新支点newstart等），分别对应apt-get和yum安装命令。绝大部分的linux系统都基于或者衍生自这两种发行版本。
	- 理论上基于同一种系统内核的，在其上编译的程序可以换到另外的系统运行，前提是编译器版本一致，比如都是gcc4.9，在ubuntu14.04 64位用gcc4.9编译的Qt程序，是能够在uos 64位上运行的。
	- 高版本编译器的系统一般能够兼容低版本的，比如你用gcc4.9编译的程序是能够在gcc7.0上运行，反过来不行。
	- 意味着如果你想尽可能兼容更多的系统，尽量用低版本的编译器编译你的程序，当然要你的程序代码语法支持，比如c++11就要从gcc4.7开始才支持，如果你的代码用了c++11则必须至少选择gcc4.7版本及以上。
	- 用Qt编写linux程序为了发布后的可执行文件可以兼容各种linux系统，只要在这两种内核（debian、redhat）的系统上用低版本的编译器比如gcc4.7编译qt程序发布即可。
	- 2022-01-27补充：根据Qt官方安装包，发现基于redhat的gcc4.9编译器发布的，通用各种linux系统（亲测ubuntu各个版本、fedora、centos、deepin、uos、银河麒麟kylin、中标麒麟neokylin、中兴新支点newstart等），自己按照这个版本也亲测打包发布了亲测可用，我擦，redhat系统的也可以在debian系统跑。
	- 2022-02-10补充：debian上静态编译的程序也可以在redhat系统跑，可能静态编译去掉了很多依赖吧。
	- 2022-03-01补充：低调大佬补充，如果没有特定的依赖关系，高版本的编译器编译的程序也可以在低版本编译器的系统运行，比如alpine Linux下用gcc11/clang13编译生成的可执行二进制，依然可以在cenos5/ubuntu10上运行。并不是编译器版本的问题，也不是C++11特性的问题，这个问题涉及到太多，内核版本、gnu libc、ABI兼容等等，两句话说不清。
	- 按照QtCreator软件采用的编译器环境规则，一般来说就是低版本的可以在高版本运行，比如Qt5可以在ubuntu14/16/18/20运行，但是高版本编译器编译的就无法在低版本编译器系统运行，会提示缺少GLBC、LIBCXX、symbol xxxxxx等，比如Qt6可以在ubuntu20运行而无法在ubuntu18/16/14等运行。
	- 在uos上做开发，建议采用系统自带的Qt库环境开发，以及命令行安装开发环境，不建议使用Qt官方的安装包搭建环境，因为uos的Qt是魔改过的，用Qt官方的标准安装包的环境开发出来的程序，打包发布很可能会有依赖问题而无法运行，而用系统自带的就不存在这个问题。
	- 国产人大金仓数据库用的是postgresql数据库改的，意味着你在Qt中用postgresql数据库插件也是能够连接到人大金仓数据库的。
	- 以上未必完全正确，欢迎各位指正。

34. 纵观Qt的发展历史，也几乎经历着合久必分、分久必合的逻辑，比如最开始QPushButton等UI控件类都是在QtGui模块中，后面越发臃肿不方便管理和升级迭代，又分离出一个QtWidgets模块；到Qt6又将QList和QVector合并了成了一个类，搞得像分久必合；而且一些数学函数以及封装的c++标准函数库的方法，逐渐放弃了Qt自己的封装改用c++标准函数库，从开始的分到现在的合统一。

35. QtCreator中pro项目文件格式说明。

	|名称|说明|
	|:------|:------|
	|QT += core gui|添加本项目中需要的模块，影响后面代码文件include的时候自动弹出下拉选择，如果pro文件没有引入该模块则无法自动语法提示，一般打包发布的时候对应动态库文件比如 Qt5Core.dll。|
	|TARGET = xxx|生成最后目标文件的名字，可以是可执行文件或者库文件。|
	|TEMPLATE = app|项目程序的生成模式，默认是app表示生成可执行文件程序，如果是动态库项目就是 TEMPLATE = lib。|
	|CONFIG += qaxcontainer|引入一些配置，在Qt4的时候还用来引入一些模块，其中有部分改成了QT += 方式引入，比如Qt5引入本地activex控件支持改成了QT += qaxcontainer。|
	|DEFINES += xxx|项目中自定义的一些定义，可以在代码文件中识别，通常用来定义一些不同平台的处理，根据项目需要自己定义任何标识。| 
	|HEADERS += head.h|项目中用到的头文件，一般拓展名是.h，可以写在一行也可以分行写，分行要用 \ 斜杠结束。|
	|SOURCES += main.cpp|项目中用到的实现文件，一般拓展名是.cpp，可以写在一行也可以分行写，分行要用 \ 斜杠结束。|
	|FORMS += Form.ui|项目中用到的UI文件，一般拓展名是.ui，可以写在一行也可以分行写，分行要用 \ 斜杠结束。|
	|RESOURCES += main.qrc|项目中用到的资源文件，可以多个，写代码使用对应资源文件中的文件时候务必记得资源文件中的前缀。|
	|LIBS += -L$$PWD/ -lavformat -lavcodec|项目中编译时候链接依赖的库，一般是 .lib .a .dylib 文件，可以写在一行，省略文件名的lib打头部分，也可以分多行绝对路径和全名称。|
	|DESTDIR += $$PWD/bin|目标生成路径，$$PWD表示当前目录，一般建议生成的最终文件重定向到另外目录存放，好找，不然一堆临时文件在里面有时候文件太多好难找。|
	|INCLUDEPATH += $$PWD/include|工程需要的头文件，指定整个目录，写代码的时候找到的话会自动下拉。|
	|DEPENDPATH += |工程的依赖路径，用的比较少，一般涉及到引入链接库的时候可能需要。|
	|include($$PWD/3rd.pri)|引入pri模块文件，pri最大的好处就是分目录管理文件，通用的轮子模块可以放到一个目录下，然后用pri统一管理，可以给多个项目公用。|

	**官方详细地址[https://doc.qt.io/qt-5/qmake-variable-reference.html](https://doc.qt.io/qt-5/qmake-variable-reference.html)**

36. 无论你是学Qt，Java，Python或其它，都需要明白一个道理：摒弃掉你的好奇心，千万不要去追求第三方类或工具是怎么实现的，这往往会让你收效甚微，其实，你只需要熟练掌握它的接口，知道类的目的即可，不可犯面向过程的毛病，刨根问底。记住，你的目标是让其它工具为你服务，你要踩在巨人的肩膀上创造世界。

37. Qt真正的核心：**元对象系统、属性系统、对象模型、对象树、信号槽。**往死里啃这五大特性，在你的项目中，逐渐的设法加入这些特性，多多练习使用它们，长此以往你会收获意想不到的效果。

38. 一边请教别人，一边多多重构，其实编码这条路虽然有人给你指路，但真正走下去的是你自己，当你真正走完时，你的编码水平一定会有非常大的提升。也许别人1000行的代码，在你这里几十行就搞定了，这也正事Qt的魅力。

39. 在阅读Qt的帮助文档时，要静下心来，不要放过每一句，记住在文档中没有废话，尤其是每段的开头。

40. Qt界的中文乱码问题，版本众多导致的如何选择安装包问题，如何打包发布程序的问题，堪称Qt界的三座大山！

41. Qt安装目录下的Examples目录下的例子，看完学完，月薪20K起步；Qt常用类的头文件的函数看完学完使用一遍并加以融会贯通，月薪30K起步。

42. 如果出现崩溃和段错误，80%都是因为要么越界，要么未初始化，死扣这两点，80%的问题解决了。

43. widget主要集中在金融、军工、安防、航天、船舶、教育等领域，qml主要集中在汽车仪表、车机、直播等领域。

	总之，无论qml还是widget，和找老婆一样，适合自己的就是最好的，自己擅长哪个就用哪个。

	如果还不知道擅长哪个，有空就两个都学，学习过程中自己就会有切身感受和对比，能者多劳多多益善。能够顺利的最快的完成老板的任务给老板赚钱才是王道。

	网友补充：如果你的软件最终是手指操作的多，就用qml，如果是鼠标操作的多，就选择widget。

44. 最后一条：**珍爱生命，远离编程。**祝大家头发浓密，睡眠良好，情绪稳定，财富自由！

45. 除官方例子外，推荐例程：

	|QtWidget开源demo集合[https://gitee.com/feiyangqingyun/QWidgetDemo](https://gitee.com/feiyangqingyun/QWidgetDemo)

	|QtQuick/Qml开源demo集合[https://gitee.com/jaredtao/TaoQuick](https://gitee.com/jaredtao/TaoQuick)

	|QtQuick/Qml开源demo集合[https://gitee.com/zhengtianzuo/QtQuickExamples](https://gitee.com/zhengtianzuo/QtQuickExamples)

### 7.Qt学习资源
#### 推荐网站主页
|名称|网址|
|:------|:------|
|qtcn|[http://www.qtcn.org](http://www.qtcn.org)|
|豆子的空间|[https://www.devbean.net](https://www.devbean.net)|
|yafeilinux|[http://www.qter.org](http://www.qter.org)|
|feiyangqingyun|[https://blog.csdn.net/feiyangqingyun](https://blog.csdn.net/feiyangqingyun)|
|**Qt作品大全**|[https://qtchina.blog.csdn.net/article/details/97565652](https://qtchina.blog.csdn.net/article/details/97565652)|
|Qt系列文章|[https://blog.csdn.net/feiyangqingyun/category_11460485.html](https://blog.csdn.net/feiyangqingyun/category_11460485.html)|
|一去二三里|[http://blog.csdn.net/liang19890820](http://blog.csdn.net/liang19890820)|
|乌托邦2号|[http://blog.csdn.net/taiyang1987912](http://blog.csdn.net/taiyang1987912)|
|foruok|[http://blog.csdn.net/foruok](http://blog.csdn.net/foruok)|
|jason|[http://blog.csdn.net/wsj18808050](http://blog.csdn.net/wsj18808050)|
|朝十晚八|[http://www.cnblogs.com/swarmbees](http://www.cnblogs.com/swarmbees)|
|BIG_C_GOD|[http://blog.csdn.net/big_c_god](http://blog.csdn.net/big_c_god)|
|公孙二狗|[https://qtdebug.com/qtbook](https://qtdebug.com/qtbook)|
|雨田哥|[https://blog.csdn.net/ly305750665](https://blog.csdn.net/ly305750665)|
|郑天佐|[https://blog.csdn.net/zhengtianzuo06](https://blog.csdn.net/zhengtianzuo06)|
|寒山-居士|[https://blog.csdn.net/esonpo](https://blog.csdn.net/esonpo)|
|前行中小猪|[http://blog.csdn.net/goforwardtostep](http://blog.csdn.net/goforwardtostep)|
|涛哥的知乎专栏|[https://zhuanlan.zhihu.com/TaoQt](https://zhuanlan.zhihu.com/TaoQt)|
|Qt君|[https://blog.csdn.net/nicai_xiaoqinxi](https://blog.csdn.net/nicai_xiaoqinxi)|

#### 推荐学习网站
|名称|网址|
|:------|:------|
|Qt老外视频教程|[http://space.bilibili.com/2592237/#!/index](http://space.bilibili.com/2592237/#!/index)|
|Qt维基补充文档|[https://wiki.qt.io/Main](https://wiki.qt.io/Main)|
|Qt源码查看网站|[https://code.woboq.org/qt5](https://code.woboq.org/qt5)|
|Qt官方下载地址|[https://download.qt.io](https://download.qt.io)|
|Qt官方下载新地址|[https://download.qt.io/new_archive/qt/](https://download.qt.io/new_archive/qt/)|
|Qt国内镜像下载地址|[https://mirrors.cloud.tencent.com/qt](https://mirrors.cloud.tencent.com/qt)|
|Qt安装包下载地址|[http://qthub.com/download/](http://qthub.com/download/)|
|Qt最新版二进制包|[https://build-qt.fsu0413.me/](https://build-qt.fsu0413.me/)|
|Qt版本更新内容|[https://doc-snapshots.qt.io/qt6-6.2/whatsnew62.html](https://doc-snapshots.qt.io/qt6-6.2/whatsnew62.html)|
|Qt中qmake变量说明|[https://doc.qt.io/qt-5/qmake-variable-reference.html](https://doc.qt.io/qt-5/qmake-variable-reference.html)|
|Qt入门最简单教程|[http://c.biancheng.net/qt/](http://c.biancheng.net/qt/)|
|qss学习地址1|[http://47.100.39.100/qtwidgets/stylesheet-reference.html](http://47.100.39.100/qtwidgets/stylesheet-reference.html)|
|qss学习地址2|[http://47.100.39.100/qtwidgets/stylesheet-examples.html](http://47.100.39.100/qtwidgets/stylesheet-examples.html)|
|精美图表控件QWT|[http://qwt.sourceforge.net/](http://qwt.sourceforge.net/)|
|精美图表控件QCustomPlot|[https://www.qcustomplot.com/](https://www.qcustomplot.com/)|
|免费图标下载|[http://www.easyicon.net/](http://www.easyicon.net/)|
|图形字体下载|[https://www.iconfont.cn/](https://www.iconfont.cn/)|
|漂亮界面网站|[https://www.ui.cn/](https://www.ui.cn/)|
|微信公众号|**官方公众号：Qt软件** &nbsp; **亮哥公众号：高效程序员**|

#### 书籍推荐
1. C++入门书籍推荐《C++ primer plus》，进阶书籍推荐《C++ primer》。
2. Qt入门书籍推荐霍亚飞的《Qt Creator快速入门》，Qt进阶书籍推荐官方的《C++ GUI Qt4编程》，qml书籍推荐《Qt5编程入门》，Qt电子书强烈推荐《Qt5.10 GUI完全参考手册》。
3. 强烈推荐程序员自我提升、修养、规划系列书《走出软件作坊》《大话程序员》《程序员的成长课》《解忧程序员》，受益匪浅，受益终生！

以上。





