# 零、准备Linux安装包

![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image002.gif)

 

Tomcat、JDK、Mysql的安装包后缀为.gz或.xz或.rpm

.gz和.xz为压缩包->解压即为原包 .rpm为原包

- .rpm安装：

    - rpm安装指令：rpm -i <文件名>

    - 所有.rpm文件直接执行rpm命令安装即可

- .gz：
    - .gz文件需要在指定位置解压生效
    - 具体位置创建文件夹：/usr/<安装的应用名：如java|tomcat等>
    - 执行解压命令：tar -xzvf <文件名>
- .xz：
    - 执行解压命令：tar -Jxf <文件名> 

# 一、JavaJDK环境搭建：

 

# 二、Tomcat环境搭建：

### 1、执行.xz文件解压流程：解压后文件如图所示

- 第一个为解压出来的文件夹，即tomcat的文件夹。

![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image004.gif)

### 2、修改第一个文件的名字：
```shell
mv apache-tomcat-9.0.39 <非中午无空格名>
// 最好是能体现端口号的名字，比如在文件的名字的后面多加一个 -80
// 这样通过再次解压tomcat.gz文件即可创建多个tomcat
// 通过访问不同的端口号即可访问不同的站点
```


![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image006.gif)

### 3、更改tomcat端口：

- 端口设置文件路径：tomcat文件夹/conf/server.xml
- 通过vim进行修改一共可以根据需求修改文件三个位置
```xml
// 关闭时使用
<Server port="8005" shutdown="SHUTDOWN">

// 一般应用使用
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />

// 为AJP端口，即容器使用，如 APACHE能通过AJP协议访问Tomcat的8009端口
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```
- tomcat单独使用时修改(一般应用使用)端口即可：
    - 将post属性值改为 80 即可

### 4、启动tomcat和关闭tomcat：

- 输入命令需要进入tomcat文件/bin目录中执行，注意命令前面的./不要漏掉了

```shell
./startup.sh  // 启动tomcat
./shutdown.sh  // 关闭tomcat
```


### 5、浏览器进入网站（显示tomcat主页面）：

- 网址 : http://ip:80  (此处80为一般应用使用的端口号)
- 阿里学生服务器进来很慢

 

### 6、部署war包到tomcat中：

1. 暂停服务器
2. 导入war包到linux服务器tomcat/webapps目录中
3. 启动服务器
4. ip:host/项目工程名 即可



# 三、Mysql环境搭建：

### 1、执行解压.gz文件流程并改名（与tomcat一致）

![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image008.gif)

### 2、创建mysql用户和用户组

- （liunx下的mysql需要专门的用户组和用户去管理）

```shell
cat /etc/passwd  // 查看用户列表

cat /etc/group  // 查看用户组列表

\---

groupadd <用户组名>  // 创建用户组

useradd -g mysql mysql <用户组名> <用户名>  // 创建一个用户并指定添加到一个用户组中 

// 一般用户组名和用户名都为mysql 
```
### 3、修改mysql文件的归属权：（修改权限）
```shell
chown -R <用户组名> : <用户名> <文件路径>  // 将文件权限改为对应的用户组中的用户

-R ：递归的，将该文件夹下所有的文件或文件夹都改成这个用户

// chown -R mysql:mysql <mysql文件夹>
```
- 修改后的文件分组

![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image010.gif)

 

### 4、修改my.cnf配置文件：
```shell
// mysql的配置文件在linux服务器/etc/my.cnf下

// 在mysql文件夹下使用该命令即可进入my.cnf中修改

// vim /etc/my.cnf

---（全部替换为以下内容）

[client]

\#设置mysql客户端默认字符集

default-character-set=utf8

socket=/var/lib/mysql/mysql.sock

[mysqld]

\#设置3306端

port=3306

socket=/var/lib/mysql/mysql.sock

\#设置mysql的安录 

basedir=/usr/local/mysql

\#设置mysql数据库的数据的存放录

datadir=/usr/local/mysql/data

\#设置服务端使用字符集

character-set-server=utf8
```

### 5、创建sock目录：
```shell
// 在/var/lib下创建mysql目录

mkdir /var/lib/mysql

// 给该目录分配权限

chmod 777 /var/lib/mysql
```

### 6、下载mysql共享库依赖：（无libaio库执行第五步初始化mysql时会报错）
```shell
// 需要服务器联网才可以直接下载

yum install -y libaio
```


### 7、初始化mysql：
```shell
// 需要cd到mysql文件中执行该命令：以下4行代码为一行使用，--前加一个空格分开

./bin/mysqld

--user=mysql

--basedir=<mysql文件路径>/

--datadir=<mysql文件路径>/data

--initialize
```
- 最后一行为初始化mysql给的随机密码：root@localhost:后面的那串代码


![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image012.gif)

 

### 8、复制启动脚本到资源目录
```shell
cp ./support-files/mysql.server /etc/init.d/mysqld
```


### 9、修改系统指向目录
```shell
// 修改/etc/init.d/mysqld

// 找到其中的basedir和datadir将其路径修改为对应的mysql文件路径

basedir=/usr/local/mysql

datadir=/usr/local/mysql/data
```


### 10、设置系统服务
```shell
// 1 增加mysqld服务执行权限

chmod +x /etc/init.d/mysqld

// 2 将mysqld服务加入到系统服务

chkconfig --add mysqld

// 检查服务是否生效

chkconfig --list mysqld
```

### 11、启动mysql服务
```shell
service mysqld start
```


### 12、登录mysql
```shell
// 需要在mysql文件/bin目录下执行 注意前面的./不能省略

./mysql -uroot -p

输入密码：<注意，在linux系统下输入密码不会显示>
```


### 13、修改密码
```shell
mysql>ALTER USER USER() IDENTIFIED BY '<新密码>';
```

### 14、开放远程连接允许：
```shell
mysql>use mysql;

msyql>update user set user.Host='%' where user.User='root';

mysql>flush privileges;
```


### 15、通过NavicatPremium远程连接到服务器


![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image014.gif)

 

![clipboard.png](Liunx%E4%B8%8B%E7%9A%84JavaWeb%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/clip_image016.gif)

 

# 四、Redis环境搭建

1、通过官网下载Redis的.gz安装包

2、在linux上解压该.gz安装包

3、通过cd进入解压后的redis包

4、直接使用make指令编译redis

- 需要gcc环境，可以通过yum -y install gcc-c++下载


make

5、使用make install进行redis安装

make install

6、默认安装的redis目录在/usr/local/bin下

 

 

 