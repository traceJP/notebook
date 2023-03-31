## 一、MyBatis-Plus简介

1. MyBatis-Plus (opens new window)（简称 MP）是一个 MyBatis (opens new window)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

2. MyBatis-Plus官方文档：https://mp.baomidou.com/guide/

- 框架结构：

![clipboard.png](MyBatisPlus%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

## 二、HelloWorld

### 1、使用maven引入
```xml
<!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus -->
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus</artifactId>
  <version></version>
</dependency>
```
- 注意事项：

    - 引入mybatis-plus依赖后，无需再次引入mybatis依赖以及Mybatis-Spring整合依赖，在mp中会自动维护这两个依赖。


### 2、配置Spring和MP

- Spring配置：

    - 与原有Mybatis配置的差异：只需要将配置的工厂bean从SqlSessionFactoryBean替换成Mybatis提供的工厂bean（com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean）即可。

```xml
<!-- 配置mybatis-spring -->
 <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
  <property name="configLocation" value="classpath:mybatis-config.xml"/>
  <property name="dataSource" ref="dataSource"/>
</bean>
```


### 3、MP与MyBaits的对比

- MyBatis：

    - 在原生MyBaits中，通常需要书写一个DAO层的接口，然后在接口中定义方法，再通过配置与该接口绑定的sql映射文件进行方法-sql语句的调用。

- 接口

```java
public interface UserMapper {
  void selectXXX();
}
```
- sql映射文件

```xml
<mapper namespace="UserMapper">
 <select id="selectXXX" ...>
  ...
 </select>
</mapper>
```
- 调用

```java
@Test
public void test() {
  userMapper.selectXXX();  // 调用接口中自定义的方法
}
```
- MP：

    - 在MP中，它封装了一些针对表CRUD的一些接口操作。Mapper层接口只需要继承MP的接口，并提供需要操作与数据库相映射的POJO类即可直接调用内部方法进行CRUD。

```java
// 接口
  // BaseMapper<?>为MP中封装的接口之一;User为一个数据库表的映射POJO
public interface UserMapper extends BaseMapper<User> {
}

// 无需书写sql映射文件

// 调用
@Test
public void test() {
  userMapper.selectList();  // 调用BaseMapper中封装的方法
}
```


## 三、全局策略配置

- 在Mybatis中，有专门的全局配置文件，即Mybatis-xml配置文件，可以对Mybatis进行``<Settings>``的设置。

- 而在MP中，也有相应的全局配置文件：

    - 其中在MP中，将所有的配置分为三个接口

        - com.baomidou.mybatisplus.core.MybatisConfiguration

        - com.baomidou.mybatisplus.core.config.GlobalConfig

        - com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig

    - 在Spring中配置其中需要设置的bean后注册到mybatis-spring中即可。

```xml
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
  <property name="configuration" ref="configuration"/> <!-- 非必须 -->
  <property name="globalConfig" ref="globalConfig"/> <!-- 非必须 -->
  ......
</bean>

<bean id="configuration" class="com.baomidou.mybatisplus.core.MybatisConfiguration">
  ......
</bean>

<bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">
  <property name="dbConfig" ref="dbConfig"/> <!-- 非必须 -->
  ......
</bean>

<bean id="dbConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">
  ......
</bean>
```
- 其中三个接口分别对应配置

    - Configuration：（Configuration）的配置大都为 MyBatis 原生支持的配置，这意味着您可以通过 MyBatis XML 配置文件的形式进行配置。


    - GlobalConfig：主要为MP相关的配置


    - DbConfig：主要为数据库相关的配置


- 具体配置可以参考Mybatis-Plus官方文档-配置


 

 

 
