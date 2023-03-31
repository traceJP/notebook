# 一、概念

- 使用Wrapper条件构造对象即可搭配MP中通用的CRUD方法进行WHERE语句的条件CRUD。
- 类图：
    - AbstractWrapper类中定义了所有的条件语句
    - QueryWrapper主要用于构造查询、删除的条件
    - UpdateWrapper主要用于构造修改的条件

![clipboard.png](Wrapper%E6%9D%A1%E4%BB%B6%E6%9E%84%E9%80%A0.assets/clip_image002.gif)

 

# 二、使用对象

- 以QueryWrapper对象为例，其QueryWrapper对象和UpdateWrapper对象的本质差别极为他们第二继承的接口不一样。

```java
public class QueryWrapper<T>
  extends AbstractWrapper<T, String, QueryWrapper<T>>
  implements Query<QueryWrapper<T>, T, String>

public class UpdateWrapper<T>
  extends AbstractWrapper<T, String, UpdateWrapper<T>>
  implements Update<UpdateWrapper<T>, String>
```


## 1、创建对象
```java
QueryWrapper<User> wrapper = new QueryWrapper<>();
```
## 2、调用对象方法构造条件（支持链式调用）
```java
// eq为AbstractWrapper接口中的方法，即为等于方法
// 以下语句为：构造一个等于条件，WHERE id = 12
wrapper.eq("id", "12");

// 链式调用构造2个等于条件：WHERE id = 1 AND email="1@1.com"
wrapper.eq("id", "1").eq("email", "1@1.com");
```


## 3、AbstractWrapper类方法

- 等于，不等于，大于等于，between等条件方法。

- 具体可以查看源码和官方文档-核心功能-条件构造器。


# 三、Query和Update功能

## 1、Query接口-select方法

- 用于指定需要查询的字段。如果不使用该方法指定，则默认查询所有字段。

- 传入参数为String可变形参

```java
// 只查询user表中的email字段
wrapper.select("email");
```
## 2、Update接口-set方法

- 用于指定sql-update语句中的SET语句。

- 注意：当wrapper条件对象和POJO中同时指定表中某个元素的值时，优先级最高为set方法。

```java
// SET name="test"
wrapper.set("name", "test");

// SET age="12"：当boolean为false时忽略此set语句元素
wrapper.set(boolean, "age", "12");

// SET name="test", age="12"：当boolean为false时忽略此set语句元素
wrapper.setSql(boolean, "name='test', age='12'")
```





