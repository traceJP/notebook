## 一、Compose概述

### 1、概述

- 官方文档：https://docs.docker.com/compose/

- Compose是 Docker公司推出的一个工具软件，可以管理多个Docker容器组成一个应用。你需要定义一个YAML格式的配置文件docker-compose.yml，写好多个容器之间的调用关系。然后，只要一个命令，就能同时启动、关闭这些容器。

    - 他是轻量级的编排工具，如果超过10个服务以上，建议采用k8s进行服务的编排。

- 核心概念：

    - 一个文件：docker-compose

    - 两个要素：

        - 服务（Service）：一个个应用容器实例，比如订单微服务、mysql容器等。

        - 工程（Project）：由一组关联的应用容器组成的一个完整业务单元，在docker-compose.yml文件中定义。

        - 即工程 = 多个服务（容器应用实例）


### 2、解决的问题

- 假设需要启动多个服务器，则每个都使用docker run命令启动过于复杂，通过Docker compose即可通过写一个配置文件，达到一键启动的效果。


### 3、安装Compose

- 最新版本在安装Docker引擎时，附带Compose参数，即可安装Docker Compose。此时使用的是docker-compose-plugin，即插件。

- 使用插件过程中，命令语法与docker-compose有些许不同，需要把docker-compose中的横杠去掉.

```shell
docker-compose --version  // 查看compose版本
docker compose version  // plugin
```
### 4、基本流程：

（1）编写Dockerfile定义各个微服务应用并构建出对应的镜像文件。

（2）使用docker-compose.yml定义一个完整业务单元，安排好整体应用中的各个容器服务。

（3）执行docker-compose up命令来启动并运行整个应用程序，完成一键部署上线。即docker-compose up命令等于多个docker run命令。

## 二、常用命令
```shell
docker-compose -h  # 查看帮助
docker-compose up  # 启动所有docker-compose服务  docker compose up
docker-compose up -d  # 启动所有docker-compose服务并后台运行
docker-compose down  # 停止并删除容器、网络、卷、镜像。
docker-compose exec <yml里面的服务ID>  # 进入容器实例内部docker-compose exec docker-compose.yml文件中写的服务id bin/bash
docker-compose ps  # 展示当前compose编排过的运行的所有容器
docker-compose top  # 展示当前compose编排过的容器进程
docker-compose logs <yml里面的服务ID>  # 查看容器输出日志
dokcer-compose config  # 检查配置
dokcer-compose config -q  # 检查配置，有问题才有输出
docker-compose restart  # 重启服务
docker-compose start  # 启动服务
docker-compose stop  # 停止服务
```


## 三、基本操作

### 1、编写Compose.yml文件

- 以一个微服务、一个Mysql、一个Redis的联调为例，详细语法参考官方文档。

```yaml
# docker-compose文件版本号
version: "3"

# 配置各个容器服务
services:
 microService:  # 名字是可以自定义的
  image: springboot_docker:1.0
  container_name: ms01 # 容器名称，如果不指定，会生成一个服务名加上前缀的容器名
  ports:
    - "6001:6001"
  volumes:
    - /app/microService:/data
  networks:
    - springboot_network
  depends_on: # 配置该容器服务所依赖的容器服务
    - redis
    - mysql

 redis:
  image: redis:6.0.8
  ports:
    - "6379:6379"
  volumes:
    - /app/redis/redis.conf:/etc/redis/redis.conf
    - /app/redis/data:data
  networks:
    - springboot_network
  command: redis-server /etc/redis/redis.conf

 mysql:
  image: mysql:5.7
  environment:
   MYSQL_ROOT_PASSWORD: '123456'
   MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
   MYSQL_DATABASE: 'db_springboot'
   MYSQL_USER: 'springboot'
   MYSQL_PASSWORD: 'springboot'
  ports:
    - "3306:3306"
  volumes:
    - /app/mysql/db:/var/lib/mysql
    - /app/mysql/conf/my.cnf:/etc/my.cnf
    - /app/mysql/init:/docker-entrypoint-initdb.d
  networks:
    - springboot_network
  command: --default-authentication-plugin=mysql_native_password # 解决外部无法访问
  
networks:
 # 创建 springboot_network 网桥网络
 springboot_network:
```
- 如上述代码所示，yml文件的每个服务容器，其本质就是变相的docker run命令。

- 例如microService下的配置翻译为docker run命令则为

```shell
docker run -d
-p 6001:6001
-v /app/microService:/data microService
--name="ms1"
--network springboot_network
# 并且需要在mysql和redis服务都是启动的状态下才会执行
```



