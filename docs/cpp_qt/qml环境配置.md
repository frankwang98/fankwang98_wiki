> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍QML介绍与入门案例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. Qt Quick与QML介绍

`Qt Quick`是一个用于构建现代、高效、可扩展用户界面的框架。它是Qt开发框架的一部分，旨在通过声明性语法和JavaScript绑定来简化用户界面的设计和实现。

Qt Quick基于`QML（Qt Meta-Object Language）`语言，这是一种类似于JSON的声明性语言，用于描述用户界面的结构和行为。使用QML，您可以使用易于理解和编写的代码来创建用户界面，并通过使用属性绑定和信号槽机制来实现交互逻辑。

以下是Qt Quick的一些关键特点：

> 1. 声明性语法：QML使用类似于CSS和JSON的语法，使得用户界面的描述更加直观和简洁。您可以声明对象、属性、信号和槽，以及定义动画和过渡效果。

> 2. 组件化和重用：Qt Quick鼓励将用户界面拆分为可重用的组件。这样可以提高开发效率，并促进界面元素的一致性和可维护性。

> 3. 属性绑定：通过属性绑定，您可以在QML中声明对象之间的依赖关系。当一个对象的属性发生变化时，绑定的对象会自动更新其相关属性，从而简化了手动处理界面元素之间的同步问题。

> 4. 动画和过渡效果：Qt Quick提供了内置的动画和过渡效果支持，使得创建平滑的用户界面动画变得容易。您可以使用动画来改变属性值、移动、旋转、缩放和淡入淡出等。
 
> 5. 可扩展性：Qt Quick是可扩展的，允许您根据需要编写自定义的QML组件和插件。这样可以轻松地扩展Qt Quick框架，并与其他Qt模块（如C++部分）进行交互。

Qt Quick提供了丰富的控件库和工具，以及强大的功能来处理用户输入、布局管理和数据模型。它广泛应用于跨平台开发，包括桌面应用程序、移动应用程序以及嵌入式设备上的图形界面。

### 😊2. QML示例

示例1：

```cpp
import QtQuick 2.9
import QtQuick.Window 2.2
import QtQuick.Controls 2.0

ApplicationWindow {
    visible: true
    width: 400
    height: 300
    title: "QML 示例"

    Column {
        spacing: 10
        anchors.centerIn: parent

        Button {
            text: "点击我"
            onClicked: {
                label.text = "你点击了按钮！"
            }
        }

        Label {
            id: label
            text: "这里将显示按钮点击的结果"
        }
    }
}

```

示例2：

```cpp
import QtQuick 2.9  //向下兼容到5.9
import QtQuick.Window 2.2   //顶级窗口

/* QML文档可以看做是一个QML对象树,这里创建了Window根对象
和它的子对象Text */
Window {
    visible: true
    width: 800
    height: 600
    title: qsTr("Hello World")  //window对象

    Text {
        id: text1
        text: qsTr("hello QML!")
        anchors.centerIn: parent    //居中布局（anchors）
        //美化字体
        font.bold: true
        font { pointSize: 14; capitalization: Font.AllUppercase }

        Behavior on rotation {
            NumberAnimation { duration: 500 }
        }
    }   //text对象

    Rectangle {
        id: colorRect
        width: 20 * 2
        height: width   //属性绑定
        radius: 20
        border.color: "green"

        anchors.left: text1.right   //绿色圆形anchor在文本右侧
        anchors.leftMargin: 10  //左侧边距10
        anchors.verticalCenter: text1.verticalCenter    //垂直居中

        MouseArea {
            anchors.fill: parent
            onClicked: {
                console.debug("colorRect: ", parent.border.color)   //终端输出颜色编码
                text1.rotation += 360   //旋转事件（点击绿色圆环，旋转360）
                text1.color = parent.border.color
                backImg.source = "images/liding2R.png"   //切换背景
            }
            //实现圆形交互特效
            hoverEnabled: true
            onEntered: {
                parent.width = 32
                parent.color = "black"
            }
            onExited: {
                parent.width = 40
                parent.color = "white"
            }
        }   //mouse鼠标对象（交互）

        Rectangle {
            width: 12 * 2
            height: width
            radius: 12
            color: parent.border.color
            anchors.centerIn: parent
        }   //生成绿色圆环
    }   //rectangle对象

    Image {
        id: backImg
        source: "images/liding2sizeT.png"  //路径
        width: parent.width //大小
        anchors.bottom: parent.bottom
        fillMode: Image.PreserveAspectFit   //填充模式
    }   //images对象
}

```

### 😆3. QML与C++交互示例

创建一个空的Qt Quick程序。

main.cpp

```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include <QObject>
#include <QDebug>

class MyObject : public QObject
{
    Q_OBJECT

public:
    Q_INVOKABLE QString message() const {
        return m_message;
    }

public slots:
    void sayHello() {
        m_message = "Hello from C++!";
        qDebug() << m_message; // 终端中打印
        emit messageChanged();
    }

signals:
    void messageChanged();

private:
    QString m_message;
};

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);

    QQmlApplicationEngine engine;

    MyObject myObject;
    engine.rootContext()->setContextProperty("myObject", &myObject);

    const QUrl url(QStringLiteral("qrc:/main.qml"));
    QObject::connect(&engine, &QQmlApplicationEngine::objectCreated,
                     &app, [url](QObject *obj, const QUrl &objUrl) {
        if (!obj && url == objUrl)
            QCoreApplication::exit(-1);
    }, Qt::QueuedConnection);
    engine.load(url);

    return app.exec();
}

#include "main.moc"

```

main.qml

```cpp
import QtQuick 2.2
import QtQuick.Controls 2.2

ApplicationWindow {
    visible: true
    width: 400
    height: 300
    title: "QML and C++ Interaction"

    Button {
        text: "Click Me"
        onClicked: {
            myObject.sayHello(); // 调用C++对象的函数
        }
    }

// Label {
// text: myObject.message // 显示从C++传递的消息
// anchors.centerIn: parent
// }
}

```

生成效果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/4cb1faa79f894efd96a3ebd24c56ff02.png)

以上。