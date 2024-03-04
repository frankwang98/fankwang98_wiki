> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Qt操作sqlite数据库示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. sqlite介绍

`SQLite`是一种嵌入式关系型数据库管理系统（RDBMS），它是一个零配置、无服务器的数据库引擎，以库的形式嵌入到应用程序中。SQLite的设计目标是轻量级、高效、可靠，它占用资源少且易于使用。

以下是SQLite的一些特点和优势：

> 1. 零配置：SQLite不需要独立的服务器进程或配置文件，它直接将数据库存储在普通的磁盘文件中。这使得SQLite非常适合于嵌入式设备和移动应用程序，可以轻松地集成到各种平台和操作系统中。

> 2. 无服务器：与传统的客户端-服务器数据库不同，SQLite没有单独的服务器进程，应用程序直接通过函数调用与SQLite库进行交互。这简化了开发过程，并减少了系统资源的消耗。

> 3. 单一文件：一个完整的SQLite数据库就是一个单一的磁盘文件，包含所有的表、索引和数据。这使得数据库的移植和维护变得非常方便，只需复制和传输一个文件即可。

> 4. 轻量级：SQLite的核心引擎非常小巧，代码库大小通常不超过数百KB，这使得它非常适合于资源受限的环境，如嵌入式设备和移动平台。

> 5. 支持标准SQL：SQLite支持大部分的SQL标准，包括常见的SQL语句、事务、触发器、视图等。它还提供了强大的查询优化器，可以高效地处理复杂的查询操作。

> 6. 可靠性和稳定性：SQLite采用了ACID事务支持，确保数据的完整性和一致性。它还具备自动故障恢复能力，即使在系统崩溃或断电情况下，数据库也能够自动恢复到之前的状态。

> 7. 跨平台支持：SQLite支持多个操作系统和编程语言，包括Windows、Linux、macOS、iOS、Android等。它提供了C/C++、Java、Python、C#等多种接口，可以方便地与不同的开发环境集成。

> 8. 开源免费：SQLite是一个开源项目，遵循Public Domain（公共领域）许可证，可以自由使用、复制和修改，无需支付任何费用。

### 😊2. Qt操作sqlite数据库示例

pro文件：

```cpp
QT       += core gui sql

```

sqlitebasic.h



```cpp
#ifndef SQLITEBASIC_H
#define SQLITEBASIC_H

#include <QObject>
#include <QSqlDatabase>
#include <QSqlQuery>
#include <QSqlError>
#include <QDebug>

typedef struct
{
    int id;
    QString name;
    int age;
}w2dba;

class SqliteBasic : public QObject
{
    Q_OBJECT
public:
    explicit SqliteBasic(QObject *parent = nullptr);

    // 打开数据库
    bool openDb(void);
    // 创建数据表
    void createTable(void);
    // 判断数据表是否存在
    bool isTableExist(QString& tableName);
    // 查询全部数据
    void queryTable();
    // 插入数据
    void singleInsertData(w2dba &singleData); // 插入单条数据
    void moreInsertData(QList<w2dba> &moreData); // 插入多条数据
    // 修改数据
    void modifyData(int id, QString name, int age);
    // 删除数据
    void deleteData(int id);
    //删除数据表
    void deleteTable(QString& tableName);
    // 关闭数据库
    void closeDb(void);

private:
    QSqlDatabase database;

signals:

public slots:
};

#endif // SQLITEBASIC_H

```

sqlitebasic.cpp



```cpp
#include "sqlitebasic.h"

SqliteBasic::SqliteBasic(QObject *parent) : QObject(parent)
{
    if (QSqlDatabase::contains("qt_sql_default_connection"))
    {
        database = QSqlDatabase::database("qt_sql_default_connection");
    }
    else
    {
        // 建立和SQlite数据库的连接
        database = QSqlDatabase::addDatabase("QSQLITE");
        // 设置数据库文件的名字
        database.setDatabaseName("MyDataBase.db");
    }
}

bool SqliteBasic::openDb()
{
    if (!database.open())
    {
        qDebug() << "Error: Failed to connect database." << database.lastError();
    }
    else
    {
        qDebug() << "Open database.";
    }

    return true;
}

void SqliteBasic::createTable()
{
    // 用于执行sql语句的对象
    QSqlQuery sqlQuery;
    // 构建创建数据库的sql语句字符串
    QString createSql = QString("CREATE TABLE student (
 id INT PRIMARY KEY NOT NULL,
 name TEXT NOT NULL,
 age INT NOT NULL)");
    sqlQuery.prepare(createSql);
    // 执行sql语句
    if(!sqlQuery.exec())
    {
        qDebug() << "Error: Fail to create table. " << sqlQuery.lastError();
    }
    else
    {
        qDebug() << "Table created!";
    }
}

bool SqliteBasic::isTableExist(QString& tableName)
{
    QSqlDatabase database = QSqlDatabase::database();
    if(database.tables().contains(tableName))
    {
        return true;
    }

    return false;
}

void SqliteBasic::queryTable()
{
    QSqlQuery sqlQuery;
    sqlQuery.exec("SELECT * FROM student");
    if(!sqlQuery.exec())
    {
        qDebug() << "Error: Fail to query table. " << sqlQuery.lastError();
    }
    else
    {
        while(sqlQuery.next())
        {
            int id = sqlQuery.value(0).toInt();
            QString name = sqlQuery.value(1).toString();
            int age = sqlQuery.value(2).toInt();
            qDebug()<<QString("id:%1 name:%2 age:%3").arg(id).arg(name).arg(age);
        }
    }
}

void SqliteBasic::singleInsertData(w2dba &singledb)
{
    QSqlQuery sqlQuery;
    sqlQuery.prepare("INSERT INTO student VALUES(:id,:name,:age)");
    sqlQuery.bindValue(":id", singledb.id);
    sqlQuery.bindValue(":name", singledb.name);
    sqlQuery.bindValue(":age", singledb.age);
    if(!sqlQuery.exec())
    {
        qDebug() << "Error: Fail to insert data. " << sqlQuery.lastError();
    }
    else
    {
        qDebug() << "singleInsert.";
    }
}

void SqliteBasic::moreInsertData(QList<w2dba>& moredb)
{
    // 进行多个数据的插入时，可以利用绑定进行批处理
    QSqlQuery sqlQuery;
    sqlQuery.prepare("INSERT INTO student VALUES(?,?,?)");
    QVariantList idList,nameList,ageList;
    for(int i=0; i< moredb.size(); i++)
    {
        idList <<  moredb.at(i).id;
        nameList << moredb.at(i).name;
        ageList << moredb.at(i).age;
    }
    sqlQuery.addBindValue(idList);
    sqlQuery.addBindValue(nameList);
    sqlQuery.addBindValue(ageList);

    if (!sqlQuery.execBatch()) // 进行批处理，如果出错就输出错误
    {
        qDebug() << sqlQuery.lastError();
    }
    else
    {
        qDebug() << "moreInsert.";
    }
}

void SqliteBasic::modifyData(int id, QString name, int age)
{
    QSqlQuery sqlQuery;
    sqlQuery.prepare("UPDATE student SET name=?,age=? WHERE id=?");
    sqlQuery.addBindValue(name);
    sqlQuery.addBindValue(age);
    sqlQuery.addBindValue(id);
    if(!sqlQuery.exec())
    {
        qDebug() << sqlQuery.lastError();
    }
    else
    {
        qDebug() << "updated data success!";
    }
}

void SqliteBasic::deleteData(int id)
{
    QSqlQuery sqlQuery;

    sqlQuery.exec(QString("DELETE FROM student WHERE id = %1").arg(id));
    if(!sqlQuery.exec())
    {
        qDebug()<<sqlQuery.lastError();
    }
    else
    {
        qDebug()<<"deleted data success!";
    }
}

void SqliteBasic::deleteTable(QString& tableName)
{
    QSqlQuery query;
    QString deleteTableQuery = QString("DROP TABLE IF EXISTS %1").arg(tableName);
    if (query.exec(deleteTableQuery)) {
        qDebug() << "Table deleted successfully.";
    } else {
        qDebug() << "Error: Failed to delete table.";
    }
}

void SqliteBasic::closeDb(void)
{
    database.close();
    qDebug() << "close success";
}

```

mainwindow.cpp



```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"
#include "sqlitebasic.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    this->setWindowTitle("sqlite基础");

    SqliteBasic sqliteBasic;
    sqliteBasic.openDb();
    sqliteBasic.createTable();

     // 判断数据表是否存在
    QString str1 = QString("student");
    qDebug() << "isTabelExist：" << sqliteBasic.isTableExist(str1);

    // 插入单条数据
    w2dba w2dbaTest1 = {1, "zhangsan", 24};
    w2dba w2dbaTest2 = {2, "lisi", 28};
    sqliteBasic.singleInsertData(w2dbaTest1);
    sqliteBasic.singleInsertData(w2dbaTest2);

    // 插入多条数据
    QList<w2dba> list;
    w2dba w2dbaTest3 = {3, "liwu", 26};
    w2dba w2dbaTest4 = {4, "zhaoliu", 27};
    list.append(w2dbaTest3);
    list.append(w2dbaTest4);
    sqliteBasic.moreInsertData(list);

    // 查询全部数据
    sqliteBasic.queryTable();
    qDebug() << endl;

    // 修改数据
    sqliteBasic.modifyData(2, "modify", 10);
    sqliteBasic.modifyData(3, "modify-2", 20);
    // 查询全部数据
    sqliteBasic.queryTable();
    qDebug() << endl;

    // 删除数据
    sqliteBasic.deleteData(2);
    // 查询全部数据
    sqliteBasic.queryTable();
    qDebug() << endl;

    // 删除数据表
    QString str2 = QString("student");
    qDebug() << "isTabelExist：" << sqliteBasic.isTableExist(str2);

    sqliteBasic.deleteTable(str2);

    qDebug() << "isTabelExist：" << sqliteBasic.isTableExist(str2);

    //关闭数据库
    sqliteBasic.closeDb();

}

MainWindow::~MainWindow()
{
    delete ui;
}

```

运行结果如下（目前只做了终端）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d8c015761a3b49aa9d586db96db0a1dd.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





