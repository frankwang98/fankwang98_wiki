> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍QWidget介绍与注册登陆界面示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. QWidget介绍

`QWidget` 是 Qt 框架中的一个基类，用于创建用户界面的可视化组件。它是所有用户界面组件的基础，包括窗口、对话框、按钮、文本框等。`QWidget` 提供了一组通用的功能和属性，以及与用户交互的事件处理机制。

下面是一些 `QWidget` 的主要特点和功能：

> 1.绘制和布局：QWidget 提供了用于绘制和布局的方法和属性。您可以使用绘图函数在 QWidget 上绘制自定义的图形和图像。通过布局管理器，您可以方便地管理和排列 QWidget 的子部件，如按钮、文本框和标签。

> 2.事件处理：QWidget 支持事件处理机制，通过重写事件处理函数来响应用户输入和操作。您可以处理鼠标事件、键盘事件、焦点事件和其他自定义事件。

> 3.样式和外观：QWidget 具有可自定义的样式和外观。您可以使用样式表（Style Sheets）来设置背景颜色、字体、边框等外观属性，以及状态切换的样式。

> 4.部件通信：QWidget 支持部件间的通信和信号槽机制。通过信号和槽的连接，一个 QWidget 可以发送信号并将其连接到其他 QWidget 的槽函数，以实现部件间的数据传递和交互。

> 5.窗口管理：QWidget 可以作为顶级窗口（Top-level Window）使用，显示为独立的窗口或对话框。它也可以作为子部件嵌入到其他窗口或容器中。

`QWidget` 是一个抽象基类，不能直接实例化，而是需要通过继承它的子类来创建具体的用户界面组件。常见的 `QWidget` 子类包括 `QMainWindow`、`QDialog`、`QPushButton`、`QLineEdit` 等。

### 😊2. 控件介绍

`QWidget` 是 Qt 框架中的基类，用于创建用户界面的可视化组件。`QWidget` 包含多个子控件，可以根据需要将其他控件添加为 QWidget 的子控件。以下是一些常见的子控件类型：

>  QPushButton（按钮）：用于实现用户点击操作的按钮控件。  
>  QLabel（标签）：用于显示文本或图像等静态内容的标签控件。  
>  QLineEdit（文本框）：用于接收用户输入文本的单行文本框控件。  
>  QTextEdit（文本编辑框）：用于接收用户输入和显示多行文本的文本编辑框控件。  
>  QComboBox（下拉框）：用于提供一个下拉选择列表的组合框控件。  
>  QCheckBox（复选框）：用于提供一个可选中或取消选中状态的复选框控件。  
>  QRadioButton（单选按钮）：用于提供一组互斥的选项中的单选按钮控件。  
>  QSlider（滑块）：用于通过拖动滑块来选择数值范围的滑块控件。 QProgressBar（进度条）：用于显示任务进度的进度条控件。  
>  QTableWidget（表格）：用于显示和编辑表格数据的表格控件。

### 😆3. 注册登陆界面示例

打开Qt，创建widget工程，添加设计师类SubWidget，用于登录后的跳转界面。登录界面编辑如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/a282cf7daceb49a69a67b1b2ab97b0b3.png)

在widget.h中定义：

```cpp
QString username;
QString password;

```

widget.cpp编写逻辑：

```cpp
#include "widget.h"
#include "ui_widget.h"
#include "subwidget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 可设置窗口位置、大小、标题、图标等，还可加入资源文件
    this->resize(400, 300); // setFixedSize()
    this->setMaximumSize(800, 600);
    this->setMinimumSize(400, 300);
// this->move(100, 100);
    QRect rect = this->geometry();
    qDebug() << "size: " << rect.width() << rect.height();
    QString window_title = "widget_demo " + QString::number(rect.width()) + "*" + QString::number(rect.height());
    this->setWindowTitle(window_title);
    this->setWindowIcon(QIcon(":/icon/main.png"));

    ui->btn_login->setEnabled(false);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_btn_register_clicked()
{
    username = ui->le_username->text();
    password = ui->le_password->text();
    qDebug() << "user: " << username << " " << password;

    if (username != "" && password != "")
    {
        qDebug() << "注册成功!";
        ui->btn_login->setEnabled(true);
        ui->btn_login->setText("可以登录"); // setText()
        // 还可以将账号密码存储在数据库中
    }
}

void Widget::on_btn_login_clicked()
{
    QString tmp_username = ui->le_username->text();
    QString tmp_password = ui->le_password->text();
    if (tmp_username == username && tmp_password == password)
    {
        // 可创建独立窗口、内嵌（子）窗口
        SubWidget *subWidget = new SubWidget();
// SubWidget *subWidget = new SubWidget(this); // 内嵌
        subWidget->setWindowTitle("SubWidget_demo");
        subWidget->show(); // 独立
    } else {
        qDebug() << "账号不存在!";
    }
}

```

基本界面如下，大家还可自己定义资源文件，以及将账号密码放在数据库或其他地方管理等。


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/bb63168d414c4fc3b1ee275282c6a648.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。
