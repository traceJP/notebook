# 一、AR简介

- ActiveRecord模式，简称AR模式。是一种可以直接操作POJO，并自动更新数据库记录的方法。


# 二、使用

- 使用与数据库表映射相同的POJO类继承Model抽象类。并重写pkVal方法。

```java
@Data
public class User extends Model<User> {
    
  // pkVal()方法：用于指定数据库表主键的方法
  // 重写只需返回POJO中代表数据库表主键的字段即可
  @Override
  protected Serializable pkVal() {
     return id;
  }

  // 注意：必须要指定主键的策略类型，否则将出现数值插入错误
  @TableId(type = IdType.AUTO)
  private Long id;

  private String name;
}
```


# 三、基本CRUD

- 以下为基本的方法。所有方法可以查看Model抽象类。


## 1、插入

- 通过设置POJO的属性，然后调用insert方法即可。

```java
User user = new User();
user.setEmail("123@q21.com");

// insert方法返回boolean，表示是否插入成功
boolean ok = user.insert();
```
- 插入或修改方法

```java
User user = new User();
user.setId(2);
user.setEmail("123@q21.com");

// 如果数据库存在相同的id则进行修改，否则插入一条新记录
user.insertOrUpdate();
```
## 2、删除

- deleteById方法直接指定id值删除

```java
User user = new User();

// 删除id=7的记录
user.deleteById(7);
```
- deleteById方法读取POJO中设置的id值删除

```java
User user = new User();
user.setId = 2;

// 删除id=2的记录
user.deleteById();
```
- 使用条件构造对象进行删除

```java
boolean delete(Wrapper<T> queryWrapper);
```
## 3、修改

- 通过读取POJO中的id值进行修改

```java
User user = new User();
user.setId = 2;

// 修改id=2的记录
user.updateById();
```
- 通过条件构造对象进行修改

```java
boolean update(Wrapper<T> updateWrapper);
```
## 4、查询

- 查询全部记录

```java
User user = new User();
List<User> list = user.selectAll();
```
- 通过给定id查询 / 通过读取POJO中的id查询 单个记录

```java
User user = new User();
user.setId(1);
user.selectById();  // 读取POJO
user.selectById(1);  // 给定id查询
```
- 通过条件构造对象查询所有记录

```java
List<T> selectList(Wrapper<T> queryWrapper);
```
- selectOne（根据构造对象查询一条记录）

- selectPage（分页查询）

- selectCount（查询记录数量）


 

 

