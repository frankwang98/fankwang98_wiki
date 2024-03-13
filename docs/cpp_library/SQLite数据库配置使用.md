







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍sqlite3数据库配置使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__55)




### 😏1. 项目介绍


项目Github地址：`https://github.com/sqlite/sqlite`


`SQLite` 是一种嵌入式的关系型数据库管理系统，它是一个开源项目，已经被广泛应用于各种应用程序和操作系统中。


以下是一些 SQLite 的特点：



> 
> 1.轻量级：SQLite 是一个非常轻量级的数据库系统，它的设计目标之一是简单、高效、占用资源少。SQLite 的核心库非常小巧，以静态或动态链接方式与应用程序集成，使得它适用于嵌入式设备和资源受限的环境。
> 
> 
> 



> 
> 2.无服务器架构：SQLite 是一种无服务器架构的数据库，意味着它不需要单独的数据库服务器进程，数据库操作直接在应用程序内部进行。这种架构使得 SQLite 在本地应用和单用户场景中非常方便和易用。
> 
> 
> 



> 
> 3.单一文件存储：SQLite 数据库以单一文件的形式存储在磁盘上，这个文件可以包含整个数据库结构和数据。这种单一文件存储的特点使得 SQLite 数据库非常易于备份、传输和部署。
> 
> 
> 



> 
> 4.支持标准 SQL：SQLite 支持标准的 SQL 查询语言，包括常见的增删改查操作、视图、触发器、索引等功能。它遵循 ANSI-SQL 标准，并且提供了丰富的数据类型和内置函数支持。
> 
> 
> 



> 
> 5.ACID 事务支持：SQLite 支持 ACID（原子性、一致性、隔离性和持久性）事务，可以确保数据库操作的可靠性和一致性。它使用写-读锁定来实现并发控制和多用户访问。
> 
> 
> 



> 
> 6.跨平台：SQLite 是跨平台的数据库系统，它可以运行在各种操作系统上，包括 Windows、macOS、Linux、Android 等。
> 
> 
> 



> 
> 7.开源和自由：SQLite 是一个完全开源的项目，遵循公共领域（Public Domain）版权协议，可以免费使用、复制和分发。
> 
> 
> 


SQLite 具有的这些特点使得它成为一种非常流行的数据库选择，尤其适合于小型和中小型的应用程序、移动应用、嵌入式设备等场景。无论是作为独立的数据库引擎还是与其他编程语言和框架集成，SQLite 提供了一种轻便、灵活和可靠的解决方案。


### 😊2. 环境配置


下面进行环境配置：


ubuntu可直接apt安装，另外可安装`sqlitebrowser`可视化工具便于管理。



```
# 安装sqlite3
sudo apt install sqlite3 libsqlite3-dev
sqlite3 --version
sqlite3 test.db
# 安装sqlitebrowser
sudo apt-get install sqlitebrowser
sqlitebrowser test.db

```

sqlite常用命令：



```
.databases：列出当前连接的数据库
.tables：列出当前数据库中的表
.schema tablename：显示指定表的结构
CREATE TABLE tablename (column1 datatype, column2 datatype, ...);：创建表
INSERT INTO tablename (column1, column2, ...) VALUES (value1, value2, ...);：插入数据
SELECT * FROM tablename;：查询表中的数据
UPDATE tablename SET column1=value1, column2=value2 WHERE condition;：更新表中的数据
DELETE FROM tablename WHERE condition;：删除表中的数据
.exit：退出命令行

```

### 😆3. 使用说明


下面进行使用分析：


数据库创建、插入、查询、关闭示例：



```
#include <iostream>
#include <sqlite3.h>

int main(int argc, char\*\* argv) {
    sqlite3\* db;
    char\* errorMsg = nullptr;

    // 打开或创建数据库文件
    int rc = sqlite3\_open("example.db", &db);
    if (rc != SQLITE_OK) {
        std::cout << "无法打开数据库: " << sqlite3\_errmsg(db) << std::endl;
        return rc;
    }

    // 创建表
    const char\* createTableQuery = "CREATE TABLE IF NOT EXISTS employees (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)";
    rc = sqlite3\_exec(db, createTableQuery, nullptr, nullptr, &errorMsg);
    if (rc != SQLITE_OK) {
        std::cout << "无法创建表: " << errorMsg << std::endl;
        sqlite3\_free(errorMsg);
        sqlite3\_close(db);
        return rc;
    }

    // 插入数据
    const char\* insertDataQuery = "INSERT INTO employees (id, name, age) VALUES (1, 'Alice', 25)";
    rc = sqlite3\_exec(db, insertDataQuery, nullptr, nullptr, &errorMsg);
    if (rc != SQLITE_OK) {
        std::cout << "无法插入数据: " << errorMsg << std::endl;
        sqlite3\_free(errorMsg);
        sqlite3\_close(db);
        return rc;
    }

    // 查询数据
    const char\* selectDataQuery = "SELECT \* FROM employees";
    rc = sqlite3\_exec(
        db,
        selectDataQuery,
        [](void\* data, int argc, char\*\* argv, char\*\* colNames) -> int {
            for (int i = 0; i < argc; i++) {
                std::cout << colNames[i] << " = " << argv[i] << std::endl;
            }
            return 0;
        },
        nullptr,
        &errorMsg
    );
    if (rc != SQLITE_OK) {
        std::cout << "无法查询数据: " << errorMsg << std::endl;
        sqlite3\_free(errorMsg);
        sqlite3\_close(db);
        return rc;
    }

    // 关闭数据库连接
    sqlite3\_close(db);

    return 0;
}

```

编译运行：



```
g++ -o main main.cpp -lsqlite3 && ./main

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





