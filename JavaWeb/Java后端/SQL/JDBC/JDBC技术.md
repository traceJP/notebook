# 一、JDBC概念

## （1）数据的持久化

- 持久化(persistence)：把数据保存到可掉电式存储设备中以供之后使用。大多数情况下，特别是企业级应用，数据持久化意味着将内存中的数据保存到硬盘上加以"固化”，而持久化的实现过程大多通过各种关系数据库来完成。（如IO流中将数据存储在本地txt文件中）

- 持久化的主要应用是将内存中的数据存储在关系型数据库中，当然也可以存储在磁盘文件、XML数据文件中。


## （1.1）Java中的数据存储技

- 在Java中，数据库存取技术可分为如下几类：

    - JDBC直接访问数据库

    - JDO (Java Data Object)技术

    - 第三方O/R工具，如Hibernate,Mybatis等

- JDBC是java访问数据库的基石，JDO、Hibernate、MyBltis等框架只是更好的封装了JDBC。


## （2）JDBC调用数据库

- JDBC(Java Database Connectivity)是一个独立于特定数据库管理系统、通用的SQL数据库存取和操作的公共接口(一组API），定义了用来访问数据库的标准Java类库，( java.sqljavax.sql）使用这些类库可以以一种标准的方法、方便地访问数据库资源。

- JDBC为访问不同的数据库提供了一种统一的途径，为开发者屏蔽了一些细节问题。

- JDBC的目标是使Java程序员使用JDBC可以连接任何提供了JDBC驱动程序的数据库系统，这样就使得程序员无需对特定的数据库系统的特点有过多的了解，从而大大简化和加快了开发过程。


![clipboard.png](JDBC%E6%8A%80%E6%9C%AF.assets/clip_image002.gif)

 


# 二、JDBC编程

## （1）JDBC程序编写步骤

![clipboard.png](JDBC%E6%8A%80%E6%9C%AF.assets/clip_image004.gif)

 

## （2）获取JDBC连接

- 方式一 ：通过java接口获取厂商实现类对象。

```java
// java.sql与com.mysql中有两个相同的类名（Driver类），导包只能导一个。
// java.sql是java中的接口，com.mysql是mysql驱动实现java的接口，相当于一个多态
// Driver driver = new com.mysql.jdbc.Driver(); 推荐使用最新的com.mysql.cj.jdbc，原先的类以被弃用

// 1 获取java.Driver的实现对象。这个独享是具体sql厂商提供的。
Driver driver = new com.mysql.cj.jdbc.Driver();

// 2 url：与javaweb中的路径相似
  // jdbc:mysql://  协议
  // localhost:3306  ip地址localhost为本机数据库:mysql的默认端口号3306
  // test  数据库名（根据数据库的名字输入）
  // 需要统一连接mysql中的时间区域时差。所以需要给连接端口中serverTimezone中赋值GMT%2B8
String url = "jdbc:mysql://localhost:3306/test?serverTimezone=GMT%2B8";

// 3 将数据库用户名和密码封装在Properties中
Properties info = new Properties();
info.setProperty("user", "root");
info.setProperty("password", "");

// 4 获取连接
Connection conn = driver.connect(url, info);
```
- 方式二、通过反射获取Driver实现类对象：（使代码不使用第三方API，即两个Driver类一起使用。）

```java
// 1 获取Driver实现对象：
Class clazz = Class.forName("com.mysql.cj.jdbc.Driver");
Driver driver = (Driver) clazz.newInstance();

// 2.3.4
...
```
- 方式三、通过DriverManager类注册驱动

```java
// 1 获取Driver实现对象并注册驱动
...

DriverManager.registerDriver(driver对象);
// 注意：当通过反射获取Driver实现类对象时，该语句可以省略，因为mysql中使用了一个静态代码块。
// 此时反射加载了该Driver类，类中的静态代码块内置了DriverManager.registerDriver语句。

// 4 获取连接
Connection conn = DriverManager.getConnection(url, user, password);
```

## （3）执行SQL语句

- 在java.sql包中有3个接口分别定义了对数据库的调用的不同方式：

    - Statement：用于执行静态SQL语句并返回它所生成结果的对象。

    - PrepatedStatement：SQL语句被预编译并存储在此对象中，可以使用此对象多次高效地执行该语句。

    - CallableStatement：用于执行SQL存储过程


![clipboard.png](JDBC%E6%8A%80%E6%9C%AF.assets/clip_image006.gif)

## （3.1）Statement-CRUD的弊端

- 存在拼串操作，繁琐复杂

```mysql
# 其中加号中的user和password是java中的String变量，需要通过修饰以区别sql和java
select user,password from user_table
where
  user = ' "+ user +" ' and password = ' "+ password +" ';
```
- 存在SQL注入：

    - SQL注入：SQL注入是利用某些系统没有对用户输入的数据进行充分的检查，而在用户输入数据中注入非法的SQL语句段或命令，从而利用系统的SQL引擎完成恶意行为的做法。

```mysql
# 在java代码中上述是没有问题的，当jdbc传入到mysql中时，将出现语义的解释问题
# 如where中的是or优先还是and优先
SELECT user,password FROM user_table
WHERE 
  user='a' OR 1='AND password = 'OR' 1 = '1';
```

## （3.2）使用P2reparedStatement实现CRUD操作

- CRUD基本流程：


![clipboard.png](JDBC%E6%8A%80%E6%9C%AF.assets/clip_image008.gif)

- 增删改：

```java
// 1 加载驱动获取连接
...Connection conn = ...

// 2 预编译sql语句：
String sql = "<sql语句>";
  // 假设此时sql语句为增加一条记录：
  // 可以使用?问号(占位符)代替具体的值
  // insert into 表名(字段1,字段2,...,字段n) values(?, ?, ?);
PreparedStatement ps = conn.prepareStatement(sql);
  // 使用setString或setInt等填充占位符,以问号的索引为单位123..排序
  ps.set...(1, "占位符value值");

// 3 执行sql
ps.execute();

// 4 资源关闭
ps.close;
conn.close;
```
- 查：

```java
// 1 2 加载驱动获取连接...预编译sql..

// 3 执行返回结果集
ResultSet resultSet = ps.executeQuery();

// 3.1 处理结果集
if(resultSet.next()) {  // next()迭代器
  int id = resultSet.getXXX(序列号);  // 序列号为查询返回的结果集字段从1排列
    
  ...

}
```





