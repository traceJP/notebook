## 一、@TableName

### 1、描述

- @TableName注解为表名注解，只能作用在类上，主要用于指定当前Mapper接口主要映射到哪个表上。

### 2、属性

- string value：指定当前Mapper映射到数据库的哪个表。

```java
// UserMapper接口映射到数据库tb_user表中，字段映射为User-POJO
@TableName(value = "tb_user")
public interface UserMapper extends BaseMapper<User> { }
```
- boolean keepGlobalPrefix：是否使用全局配置中的tablePrefix值。如果设置了全局tablePrefix和value属性则默认为false。

```java
// 使用全局配置中的tablePrefix值
// 如果tablePrefix = "test";那么value将拼接成testTb_user
@TableName(value = "tb_user", keepGlobalPrefix = true)
public interface UserMapper extends BaseMapper<User> { }
```


## 二、@TableId

### 1、描述

- @TableId注解为主键注解，只能作用在映射到数据库的POJO属性上，主要用于指定该字段对应数据库的主键字段的行为方式


### 2、属性

- string value：用于指定当前POJO属性映射到数据库中的字段。（可以省略）

- enum type：用于指定主键的类型

    - 其他类型可以参考MP文档

```java
// 该POJO中的id字段对应数据库中的主键字段
// 其中该主键类型为自增类型
public class pojo() {
  @TableId(type = IdType.AUTO)
  private Integer id;
}
```


## 三、@TableFieId

### 1、描述

- @TableFieId注解为字段注解（非主键的字段），只能作用在映射到数据库的POJO属性上，主要用于指定该字段对应数据库映射字段。


### 2、属性

- string value：用于指定当前POJO属性对应哪个数据库字段。

- boolean exist：用于指定是否是数据库的字段。一个POJO对应一张表时，可能会有POJO中的属性不是数据库中的字段。所以使用该属性可以使MP忽略POJO的属性。


 



