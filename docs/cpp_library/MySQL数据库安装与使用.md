








#### 文章目录


* + [1. MySQL介绍](#1_MySQL_1)
	+ [2. Windows端](#2_Windows_15)
	+ - [环境安装](#_16)
		- [数据库的基本操作](#_110)
	+ [3. Ubuntu端](#3_Ubuntu_261)
	+ - [环境安装](#_262)
		- [数据库的基本操作](#_325)




### 1. MySQL介绍


MySQL是一种开源的关系型数据库管理系统（RDBMS），广泛应用于各种规模和类型的应用程序中。以下是MySQL的一些主要特点和功能：



> 
> 1.开源性：MySQL是开源软件，可以免费使用和修改，具有强大的社区支持。  
>  2.可扩展性：MySQL支持高度可扩展的架构，适用于小型应用到大型企业级应用。  
>  3.跨平台支持：MySQL可以在多个操作系统上运行，包括Windows、Linux、macOS等。  
>  4.高性能：MySQL具有出色的性能和处理能力，能够处理大量的并发请求，并提供高效的查询和数据操作。  
>  5.安全性：MySQL提供了多种安全机制来确保数据的机密性和完整性，包括用户身份验证、访问控制等。  
>  6.支持标准SQL语言：MySQL遵循SQL标准，并提供了广泛的SQL功能，包括数据查询、更新、事务处理等。  
>  7.多种存储引擎：MySQL支持多种存储引擎，如InnoDB、MyISAM等，每种引擎都有不同的特点和适用场景。  
>  8.数据复制和高可用性：MySQL提供了复制功能，可以将数据复制到多个服务器以实现高可用性和数据备份。  
>  9.数据库管理工具：MySQL提供了命令行工具和图形化管理工具（如Navicat、phpMyAdmin等），方便用户管理和监控数据库。
> 
> 
> 


无论是小型网站还是大型企业应用，MySQL都是一个强大而可靠的选择。它广泛用于Web开发、数据分析、电子商务、日志存储等各种应用场景，被许多开发者和组织所采用和信赖。


### 2. Windows端


#### 环境安装


安装mysql不再赘述。


windows下环境为：`VS2017+mysql8.0.31`


新建工程，然后将编译器配置为`Release x64`；


添加包含目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/664f4479b71544df859ab6419072be84.png)


添加库目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/66ca14772d814904ad3579459c6479ca.png)


添加附加依赖项（静态库）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/600cfb98a115458ab57a80d27dafd764.png)


将`libmysql.dll`复制到工程所在目录：


![在这里插入图片描述](https://img-blog.csdnimg.cn/5282a3e76631450494b3984ef52ba1aa.png)


测试程序：



```
#include <iostream>
#include "mysql.h"

using namespace std;

int main(int argc, char \*argv[])
{
	///< 创建数据库句柄
	MYSQL mysql;
	//MYSQL \*mysql = mysql\_init(nullptr);

	///< 初始化句柄
	mysql\_init(&mysql);

	///< 连接的数据库（句柄、主机名、用户名、密码、数据库名、端口号、socket指针、标记）
	if (!mysql\_real\_connect(&mysql, "localhost", "root", "root", "test\_mysql", 3306, nullptr, 0))
	{
		cout << "数据库连接失败" << mysql\_errno(&mysql) << endl;
		return -1;
	}

	cout << "数据库连接成功" << endl << endl;

	///< 创建数据库回应结构体
	MYSQL_RES \*res = nullptr;
	///< 创建存放结果的结构体
	MYSQL_ROW row;

	char sql[1024]{ 0 };
	sprintf\_s(sql, 1024, "select \* from user\_info");

	///< 调用查询接口
	if (mysql\_real\_query(&mysql, sql, (unsigned int)strlen(sql)))
	{
		cout << "查询失败" << ": " << mysql\_errno(&mysql) << endl;
	}
	else
	{
		cout << "查询成功" << endl << endl;

		///< 装载结果集
		res = mysql\_store\_result(&mysql);

		if (nullptr == res)
		{
			cout << "装载数据失败" << ": " << mysql\_errno(&mysql) << endl;
		}
		else
		{
			///< 取出结果集中内容
			while (row = mysql\_fetch\_row(res))
			{
				cout << row[0] << " " << row[1] << endl;
			}
		}
	}

	///< 释放结果集
	mysql\_free\_result(res);

	///< 关闭数据库连接
	mysql\_close(&mysql);

	system("pause");

	return 0;
}

```

#### 数据库的基本操作


插入数据：



```
void insert(MYSQL\* conn, int ID, char name[20], int age, float score)
//插入数据
{
	char str[64] = "INSERT INTO student VALUES( ";
	char buffer[128] = { 0 };
	char str2[4] = ",'";
	char str3[4] = "',";
	char str4[2] = ",";
	char str5[2] = ")";
	int len = sprintf\_s(buffer, "%s%d%s%s%s%d%s%f%s", str, ID, str2, name, str3, age, str4, score, str5);
	mysql\_query(&mysql, buffer);
	if (len < 0)
		cout << "存档失败！" << endl;
	if (len > 0)
		cout << "存档成功！" << endl;
}

```

删除数据：



```
void delete(char str2[])
//删除数据
{
	char str1[64] = "DELETE FROM student WHERE name='";
	char str3[10] = "'";
	char buffer[1024];
	int len = sprintf\_s(buffer, "%s%s%s", str1, str2, str3);
	mysql\_query(&mysql, buffer);
	if (len < 0)
		cout << "删除失败！" << endl;
	else
		cout << "删除成功！" << endl;
}

```

查询所有数据：



```
#include <mysql.h> // mysql文件
#include <iostream>
#include <cstring>
#include <stdio.h>
 
using namespace std;
 
MYSQL mysql;    //数据库句柄
MYSQL_RES\* res; //查询结果集
MYSQL_ROW row;  //记录结构体
 
//查询全部数据并输出
void display()
{
	//查询数据
	int ret = mysql\_query(&mysql, "select \* from student;");
 
	//获取结果集
	res = mysql\_store\_result(&mysql);
 
	cout << "ID " << "name " << "age " << "score" << endl;
    //给ROW赋值，判断ROW是否为空，不为空就打印数据。
	while (row = mysql\_fetch\_row(res))
	{
		cout << row[0] << " ";//打印ID
		cout << row[1] << " ";//打印name
		cout << row[2] << " ";//打印age
		cout << row[3] << endl;//打印score
	}
	//释放结果集
	mysql\_free\_result(res);
 
	//关闭数据库
	mysql\_close(&mysql);
}
 
int main()
{
	//初始化数据库
	mysql\_init(&mysql);
 
	//设置字符编码
	mysql\_options(&mysql, MYSQL_SET_CHARSET_NAME, "gbk");
 
	//连接数据库
	if (mysql\_real\_connect(&mysql, "localhost", "root", "root", "student", 3306, NULL, 0) == NULL) 
    //localhost为服务器，root为用户名和密码，school为数据库名，3306为端口
    {
		printf("错误原因： %s\n", mysql\_error(&mysql));
		printf("连接失败！\n");
		exit(-1);
	}
 
 	// 显示数据库
    display();
 
    //关闭数据库
	mysql\_close(&mysql);
	return 0;
}

```

查询指定数据并转换为int或float类型：



```
//查询特定数据
void select(int& ID, char str[], int& age, float& score)
{
	char str1[64] = "SELECT \* FROM student WHERE NAME='";
	char str2[2] = "'";
	char buffer[1024];//缓冲区数组
 
	sprintf\_s(buffer, "%s%s%s", str1, str, str2);
	mysql\_query(&mysql, buffer);
	res = mysql\_store\_result(&mysql);
	//给ROW赋值，判断ROW是否为空，不为空就打印数据。
	while (row = mysql\_fetch\_row(res))
	{
		ID = atoi(row[0]);
		age = atoi(row[2]);
		score = atof(row[3]);
		cout <<"ID=" << ID << " name=" << row[1] <<  " age=" << age << " score=" << score << endl;
	}
	//释放结果集
	mysql\_free\_result(res);
}

```

更新数据：



```
void update(MYSQL\* conn, int ID, char name[20], int age, float score)
//更新数据
{
	char str[64] = "UPDATE student SET ID=";
	char buffer[128] = { 0 };
	char str2[16] = ",age=";
	char str3[16] = ",score=";
	char str4[32] = " WHERE name='";
	char str5[10] = "'";
	int len = sprintf\_s(buffer, "%s%d%s%d%s%f%s%s%s", str, ID, str2, age, str3, score, str4, name, str5);
	mysql\_query(&mysql, buffer);
	if (len < 0)
		cout << "修改失败！" << endl;
	if (len > 0)
		cout << "修改成功！" << endl;
}

```

### 3. Ubuntu端


#### 环境安装


安装mysql：`sudo apt install mysql-server`


验证安装成功：`sudo systemctl status mysql` 或 `mysql -V`



```
# 管理员进入mysql
sudo mysql
# 给root用户设置密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new\_password';
FLUSH PRIVILEGES;	# 刷新
EXIT;	# 退出
# 允许任意host访问（如果只允许某个ip访问，则可以改成相应的ip）
mysql -uroot -p
use mysql;
update user set host = '%' where user = 'root';
select host, user from user;
flush privileges;
exit;

```

然后就可以用`navicat`连接ubuntu端的`mysql`了（配置ssh和mysql信息）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f3afaf21c56a4603ab6dce0b0818ce57.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2995b7f551f046ee93962bc52628b6cb.png)  
 Navicat可以方便地对数据库进行可视化操作，例如从excel导入数据到表，运行sql语句增删查改等。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e9230ee8d93540319290cb396b77d41c.png)


安装c++支持库：`sudo apt-get install libmysqlclient-dev libmysqlcppconn-dev`


测试程序：



```
#include <mysql\_connection.h>
#include <mysql\_driver.h>
#include <cppconn/driver.h>

using namespace sql;
using namespace std;

#define DBHOST "tcp://127.0.0.1:3306"
#define USER "root"
#define PASSWORD "xxx"

int main()
{
    Driver \*driver;
    Connection \*conn;
    driver = get\_driver\_instance();
    conn = driver->connect(DBHOST, USER, PASSWORD);
    cout << "DataBase connection autocommit mode = " << conn->getAutoCommit() << endl;
    delete conn;
    driver = NULL;
    conn = NULL;
    return 0;
}

```

编译运行：



```
g++ -o main main.cpp -lmysqlcppconn
# DataBase connection autocommit mode = 1

```

#### 数据库的基本操作



```
#include <iostream>
#include <mysql/mysql.h>

using namespace std;

// sql\_create 创建数据库
int sql\_create()
{
    MYSQL mysql;
    mysql\_init(&mysql);
    if (!mysql\_real\_connect(&mysql, "localhost", "root", "wangzf123", 0, 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(&mysql) << " "
             << mysql\_errno(&mysql) << endl;
        return -1;
    }

    // run mysql sentence
    string str = "create database school;";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    str = "alter database school charset=utf8mb4;";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    str = "create table school.students(id int(10) primary key auto\_increment, name varchar(20) not null, age int(3) not null);";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    mysql\_close(&mysql);
    return 0;
}

// sql\_add 增加数据
int sql\_add()
{
    MYSQL mysql;
    mysql\_init(&mysql);
    mysql\_options(&mysql, MYSQL_SET_CHARSET_NAME, "utf8mb4");
    mysql\_options(&mysql, MYSQL_INIT_COMMAND, "SET NAMES utf8mb4");

    if (!mysql\_real\_connect(&mysql, "localhost", "root", "wangzf123", "school", 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(&mysql) << " "
             << mysql\_errno(&mysql);
        return -1;
    }

    string str = "insert into students(id, name, age) values(null, \'小明\', 30)";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());
    str = "insert into students(id, name, age) values(null, \'小红\', 28)";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());
    str = "insert into students(id, name, age) values(null, \'小白\', 33)";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());
    str = "insert into students(id, name, age) values(null, \'小黑\', 29)";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());
    str = "insert into students(id, name, age) values(null, \'小黄（哥）\', 29)";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    mysql\_close(&mysql);
    return 0;
}

// sql\_modify 修改数据
int sql\_modify()
{
    MYSQL mysql;
    mysql\_init(&mysql);
    mysql\_options(&mysql, MYSQL_SET_CHARSET_NAME, "utf8mb4");
    mysql\_options(&mysql, MYSQL_INIT_COMMAND, "SET NAMES utf8mb4");

    if (!mysql\_real\_connect(&mysql, "localhost", "root", "wangzf123", "school", 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(&mysql) << " "
             << mysql\_errno(&mysql);
        return -1;
    }

    string str = "update students set age = 35 where name = \'小明\'";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    mysql\_close(&mysql);
    return 0;
}

// sql\_delete 删除数据
int sql\_delete()
{
    MYSQL mysql;
    mysql\_init(&mysql);
    mysql\_options(&mysql, MYSQL_SET_CHARSET_NAME, "utf8mb4");
    mysql\_options(&mysql, MYSQL_INIT_COMMAND, "SET NAMES utf8mb4");

    if (!mysql\_real\_connect(&mysql, "localhost", "root", "wangzf123", "school", 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(&mysql) << " "
             << mysql\_errno(&mysql);
        return -1;
    }

    string str = "delete from students where name = \'小明\'";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());

    mysql\_close(&mysql);
    return 0;
}

// sql\_query1 查询数据1
int sql\_query1()
{
    MYSQL mysql;
    mysql\_init(&mysql);
    mysql\_options(&mysql, MYSQL_SET_CHARSET_NAME, "utf8mb4");
    mysql\_options(&mysql, MYSQL_INIT_COMMAND, "SET NAMES utf8mb4");

    if (!mysql\_real\_connect(&mysql, "localhost", "root", "wangzf123", "school", 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(&mysql) << " "
             << mysql\_errno(&mysql);
        return -1;
    }

    string str = "select \* from students;";
    mysql\_real\_query(&mysql, str.c\_str(), str.size());
    MYSQL_RES \*result = mysql\_store\_result(&mysql);

    MYSQL_ROW row;
    while (row = mysql\_fetch\_row(result))
    {
        cout << "id: " << row[0] << " name: " << row[1] << " age: " << row[2]
             << endl;
    }

    mysql\_free\_result(result);
    mysql\_close(&mysql);
    return 0;
}

// sql\_query2 查询数据2
int sql\_query2()
{
    MYSQL \*conn = mysql\_init(NULL);
    mysql\_options(conn, MYSQL_SET_CHARSET_NAME, "utf8mb4");
    mysql\_options(conn, MYSQL_INIT_COMMAND, "SET NAMES utf8mb4");

    if (!mysql\_real\_connect(conn, "localhost", "root", "wangzf123", "school", 3306,
                            NULL, 0))
    {
        cout << "mysql connect error: " << mysql\_error(conn) << " "
             << mysql\_errno(conn);
        return -1;
    }

    char sql[1024];
    int cool = 30;
    sprintf(sql, "select \* from students where age < %d", cool);
    if (mysql\_query(conn, sql))
    {
        cout << "mysql connect error: " << mysql\_error(conn) << " "
             << mysql\_errno(conn);
        mysql\_close(conn);
        return -1;
    }

    MYSQL_RES \*result = mysql\_store\_result(conn);

    MYSQL_ROW row;
    while (row = mysql\_fetch\_row(result))
    {
        cout << "id: " << row[0] << " name: " << row[1] << " age: " << row[2]
             << endl;
    }

    mysql\_free\_result(result);
    mysql\_close(conn);
    return 0;
}

int main()
{
    cout << "创建数据库..." << endl;
    sql\_create();
    cout << "添加数据..." << endl;
    sql\_add();
    cout << "查询数据..." << endl;
    sql\_query1();
    cout << "修改数据..." << endl;
    sql\_modify();
    cout << "查询数据..." << endl;
    sql\_query1();
    cout << "删除数据..." << endl;
    sql\_delete();
    cout << "查询数据..." << endl;
    sql\_query1();
    cout << "条件查询数据..." << endl;
    sql\_query2();

    return 0;
}


```

编译运行：



```
g++ -o main main.cpp -lmysqlclient
./main

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f5719482477c44f485686e88d69cff00.png)


以上。





