







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Redis数据库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__51)




### 😏1. 项目介绍


项目Github地址：`https://github.com/redis/redis`


Redis（Remote Dictionary Server）是一款开源的内存数据结构存储系统，它提供了一个键值对存储模型，其中的键可以包括`字符串、哈希、列表、集合和有序集合`等数据类型。它支持持久化、主从复制、集群和事务等功能。


以下是一些关键特性和用途：



> 
> 1.高性能：Redis将数据存储在内存中，因此可以实现非常高的读写性能。它使用单线程模型和异步I/O操作来实现高效的处理请求。
> 
> 
> 



> 
> 2.丰富的数据结构：Redis不仅仅支持简单的字符串键值对存储，还提供了各种数据结构，如哈希表（hashes）、列表（lists）、集合（sets）和有序集合（sorted sets）。这使得Redis非常适合于在内存中处理各种类型的数据。
> 
> 
> 



> 
> 3.持久化：Redis提供了两种方式的持久化机制，即RDB（Redis数据库文件）和AOF（Append-only File）。RDB通过将数据集快照写入磁盘，以便在重新启动时重新加载数据。而AOF则通过记录所有写操作来实现持久化。
> 
> 
> 



> 
> 4.主从复制：Redis支持主从复制，可以将一个Redis实例配置为主服务器，而其他实例作为从服务器。主服务器上的写操作会被自动地复制到所有从服务器上，从而实现数据的冗余备份和负载均衡。
> 
> 
> 



> 
> 5.发布与订阅：Redis支持发布与订阅模式，允许客户端订阅一个或多个频道，以接收特定类型的消息。这使得Redis非常适合于构建实时应用程序、消息队列和聊天系统等。
> 
> 
> 



> 
> 6.事务支持：Redis支持事务操作，可以将多个命令打包成一个事务进行执行。通过MULTI、EXEC、DISCARD和WATCH等命令，可以实现原子性的提交或回滚多个命令。
> 
> 
> 



> 
> 7.缓存：由于Redis的快速读写能力和丰富的数据结构，它经常被用作缓存层。将经常访问的数据存储在Redis中，可以极大地提升应用程序的性能和响应时间。
> 
> 
> 


总之，Redis是一款功能强大且灵活的内存数据存储系统，适用于处理高速读写和实时数据处理的应用场景，例如缓存、会话存储、计数器、排行榜和消息队列等。


### 😊2. 环境配置


下面进行安装运行：



```
# ubuntu安装
sudo apt install redis-server
# 检查运行状态
sudo systemctl status redis-server
# 测试服务器是否正常
redis-cli ping （可以通过redis-cli进入客户端）
# 配置远程连接
sudo gedit /etc/redis/redis.conf
将 bind 127.0.0.1 ::1 改为 bind 0.0.0.0
# 设置密码
找到# requirepass foobared 改为 requirepass 自己的密码
sudo systemctl redis-server restart

```

服务端配置完成后，可以安装图形化工具`Redis desktop manager`便于管理。


可通过ssh远程连接到远端的redis数据库。


![在这里插入图片描述](https://img-blog.csdnimg.cn/ac15f3ca27604ea6baff990ddb8ad445.png)


### 😆3. 使用说明


首先安装redis c++依赖：`sudo apt-get install libhiredis-dev`


下面是一个数据库操作示例：



```
#include <iostream>
#include <hiredis/hiredis.h>

int main()
{
    // 连接 Redis 服务器
    redisContext \*redis = redisConnect("127.0.0.1", 6379);

    // 检查连接是否成功
    if (redis == NULL || redis->err)
    {
        std::cout << "无法连接到 Redis 服务器：" << redis->errstr << std::endl;
        return 1;
    }

    // 执行 AUTH 命令，提供密码
    redisReply \*reply = (redisReply \*)redisCommand(redis, "AUTH password");

    // 检查认证是否成功
    if (reply == NULL || reply->type == REDIS_REPLY_ERROR)
    {
        std::cout << "Redis 认证失败：" << reply->str << std::endl;
        freeReplyObject(reply);
        redisFree(redis);
        return 1;
    }

    std::cout << "Redis 认证成功！" << std::endl;
    freeReplyObject(reply);

    // 执行 SET 命令
    reply = (redisReply \*)redisCommand(redis, "SET key1 value1");
    std::cout << "SET 命令执行结果：" << reply->str << std::endl;
    freeReplyObject(reply);

    // 执行 GET 命令
    reply = (redisReply \*)redisCommand(redis, "GET key1");
    std::cout << "GET 命令执行结果：" << reply->str << std::endl;
    freeReplyObject(reply);

    // 关闭连接
    redisFree(redis);

    return 0;
}

```

编译运行：



```
g++ main.cpp -o redis_example -lhiredis
Redis 认证成功！
SET 命令执行结果：OK
GET 命令执行结果：value1

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





