> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍QCustomPlot图表绘制库配置与示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. QCustomPlot介绍

`QCustomPlot`是一个基于Qt框架的开源图表绘制库，用于绘制各种类型的二维图表和科学数据可视化。它提供了丰富的绘图功能和灵活的定制选项，使开发者能够轻松创建交互式和高度可定制的图表。

以下是一些`QCustomPlot`库的特点和功能：

> 1.多种图表类型：QCustomPlot支持绘制各种常见的二维图表类型，包括散点图、线图、柱状图、饼图、等值线图等。你可以根据数据的特点选择合适的图表类型。
 
> 2.数据可视化：该库提供了丰富的功能来可视化科学数据。你可以通过绘制数据点、曲线、颜色映射和等值线等方式，直观地展示数据的分布、趋势和关联性。

> 3.交互式操作：QCustomPlot支持交互式操作，允许用户通过鼠标与图表进行交互。你可以缩放、平移、选择数据点、显示工具提示等，以便用户对图表进行探索和分析。

> 4.定制选项：该库提供了丰富的定制选项，可以根据需要调整图表的外观和行为。你可以设置轴的刻度、标签和范围，选择图例的位置和样式，自定义绘图元素的样式和颜色等。

> 5.轻量级和易于集成：QCustomPlot是一个轻量级的库，易于集成到现有的Qt应用程序中。它只依赖于Qt库本身，没有其他外部依赖，使得它成为一个方便和灵活的选择。

### 😊2. 环境安装与配置

官网：`https://www.qcustomplot.com/index.php/`

QCustomPlot可直接从官网下载，在工程中引入`.h .cpp`就可以，此外，官网也提供了几个示例程序，可参考。

引用这个库，需要在pro文件加入：`QT += printsupport`

### 😆3. 应用示例

基本绘图示例：

```cpp
// mainwindow.h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include "qcustomplot.h"

namespace Ui {
class MainWindow;
}

class QCustomPlot;

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

    void setupQuadraticDemo(QCustomPlot *customPlot);

private:
    Ui::MainWindow *ui;
};

#endif // MAINWINDOW_H

```


```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    QCustomPlot* customPlot = new QCustomPlot;
    setCentralWidget(customPlot);

    setupQuadraticDemo(customPlot);
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::setupQuadraticDemo(QCustomPlot *customPlot)
{
    QVector<double> x(101), y(101);
    for (int i = 0; i < 101; ++i) {
        x[i] = i / 50.0 - 1; // -1 到 1
        y[i] = x[i] * x[i];
    }

    customPlot->addGraph(); // 添加一个曲线图QGraph
    customPlot->graph(0)->setData(x, y); // 为曲线图添加数据
    customPlot->graph(0)->setName(QString::fromLocal8Bit("customplot_quadratic_demo")); // 设置曲线图的名字
    customPlot->xAxis->setLabel("x"); // 设置x和y轴的标签
    customPlot->yAxis->setLabel("y");
    customPlot->xAxis->setRange(-1, 1); // 设置x轴的范围为(-1,1)
    customPlot->yAxis->setRange(0, 1);
    customPlot->legend->setVisible(true); // 显示图例
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/22521e3a157949168ffaab6cc7f842bc.png)


以上。
