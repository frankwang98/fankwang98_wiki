> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Qt事件介绍与正弦曲线绘制示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. Qt事件介绍

Qt 的事件机制是一种基于事件驱动的机制，用于处理用户输入、系统事件和自定义事件。

以下是一些常见的 Qt 事件：

> 1.鼠标事件（Mouse Events）：包括鼠标按下、释放、移动、滚轮滚动等事件。可以通过重写 QWidget 或 QGraphicsItem 的鼠标事件处理函数来响应这些事件，例如 mousePressEvent、mouseReleaseEvent、mouseMoveEvent 等。

> 2.键盘事件（Keyboard Events）：包括按键按下、释放、重复等事件。可以通过重写 QWidget 或 QGraphicsItem 的键盘事件处理函数来响应这些事件，例如 keyPressEvent、keyReleaseEvent、keyReleaseEvent 等。

> 3.绘图事件（Paint Events）：当需要绘制或更新窗口内容时触发。可以通过重写 QWidget 或 QGraphicsItem 的绘图事件处理函数 paintEvent 来自定义绘图操作。

> 4.定时器事件（Timer Events）：用于定时执行某个操作。可以通过 QObject 的 startTimer 函数启动一个定时器，并重写 QObject 的 timerEvent 函数来处理定时器事件。

> 5.窗口事件（Window Events）：包括窗口的打开、关闭、激活、失去焦点等事件。可以通过重写 QWidget 的窗口事件处理函数，如 closeEvent、activateEvent、focusInEvent 等。

> 6.自定义事件（Custom Events）：您可以使用 QEvent 的派生类来定义自己的自定义事件，并通过 QCoreApplication::sendEvent 或 QCoreApplication::postEvent 来触发和处理这些事件。

除了上述事件外，Qt 还提供了其他类型的事件，如拖放事件、滚动事件、焦点事件等，以满足不同的应用需求。

在 Qt 中，可以通过以下方式来处理事件：

> 1.重写相应的事件处理函数：通过重写 QWidget 或 QGraphicsItem 的事件处理函数来处理特定类型的事件。

> 2.使用信号和槽机制：将事件连接到信号槽，从而触发相应的槽函数进行处理。

> 3.使用事件过滤器（Event Filters）：通过安装事件过滤器，拦截并处理特定类型的事件。

事件处理是 Qt 程序中很重要的一部分，它允许应用程序与用户交互并响应外部事件。开发者可以根据实际需求选择适当的事件处理方式来实现所需的功能。

### 😊2. 正弦曲线绘制示例

首先，创建widget工程，在头文件定义：

```cpp
// widget.h
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QPainter>
#include <QtMath>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget();

protected:
    void paintEvent(QPaintEvent *event) override;
    void timerEvent(QTimerEvent *event) override;

private:
    Ui::Widget *ui;

    int x;  // 当前点的x坐标
    int y;  // 当前点的y坐标
    QVector<QPoint> points;  // 存储绘制曲线的点
};

#endif // WIDGET_H

```

源文件：

```cpp
// widget.cpp
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);

    resize(400, 300);
    startTimer(10);  // 设置定时器，每10毫秒触发一次timerEvent

    // 设置初始位置
    x = 0;
    y = height() / 2;
}

Widget::~Widget()
{
    delete ui;
}

void Widget::paintEvent(QPaintEvent *event)
{
    Q_UNUSED(event);

    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing);
    painter.fillRect(rect(), Qt::white);

    int width = this->width();
    int height = this->height();
    int padding = 0;

    painter.drawLine(padding, height / 2, width - padding, height / 2);  // X 轴
    painter.drawLine(padding, padding, padding, height - padding);       // Y 轴

    // 绘制正弦函数曲线
    painter.setPen(Qt::blue);
    painter.drawPolyline(points);
}

void Widget::timerEvent(QTimerEvent *event)
{
    Q_UNUSED(event);

    // 计算下一个点的位置
    x += 1;
    y = height() / 2 - 50 * qSin(qDegreesToRadians(static_cast<double>(x)));

    // 添加点到曲线上
    points.append(QPoint(x, y));

    // 如果超出窗口宽度，清空曲线重新开始
    if (x > width()) {
        x = 0;
        points.clear();
    }

    update();  // 更新绘图
}

```

生成示例如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/409ee23e15304776806cca6eec919f98.png)


### 😆3. 其他


事件过滤器


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





