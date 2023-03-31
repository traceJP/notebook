## 一、概念

- Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。


- redis官方文档https://redis.io/documentation


## 二、安装

- redis是由c语言编写的一种数据库结构存储系统，其中内核大部分固定写死了使用linux的系统命令，所以windows系统下的redis是一个删减版的。

- linux下的redis安装

    - 参考笔记linux-环境搭建

- 安装后redis命令默认存放在/usr/local/bin目录下


## 三、HelloWorld

### 1、备份redis默认配置文件

- 希望不破坏初始化的配置文件，以防出问题回退。后续操作将建立在备份文件中。

- 在解压出来的安装文件中存在redis.conf文件，该文件为redis的配置文件

```shell
cp redis.conf <备份路径> 
```
### 2、修改redis.conf文件，允许其进程管理

```properties
\# By default Redis does not run as a daemon. Use 'yes' if you need it.

\# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.

\# When Redis is supervised by upstart or systemd, this parameter has no impact.

daemonize no  #将no改为yes
```
### 3、使用该配置文件启动redis服务

```shell
redis-server <redis配置文件路径>
```
### 3.1、使用命令查看服务是否成功启动

```shell
ps -ef | grep redis
```
### 4、进入redis命令行提示符界面

```shell
redis-cli -p 6379  # redis默认端口6379
```
### 5、开始使用redis命令

```shell
127.0.0.1:6379> ping
# PONG
```
## 四、redis知识点补充

1. redis是以单线程模型来处理客户端请求。

2. 默认有16个数据库（可通过redis.conf修改），这些数据库互相独立，可以通过select命令切换。

```shell
select <数据库索引0-15>
```
3. Dbsize命令可以查看当前数据库key的数量

- 默认存在key、counter、myhash


## 五、redis命令

- redis命令大全可以参考官方文档

    - https://redis.io/commands

    - 中文文档http://doc.redisfans.com/

- 连接基本命令：

```shell
auth  # 验证安全密码
echo  # 打印一条信息
ping  # 测试连通性
quit / exit  # 关闭连接
select  # 切换数据库
```
- 服务器基本命令：

```shell
flushdb  # 清空指定数据库
flushall  # 清口所有数据库
```





