## 一、概念：

- 在Java中可以使用Properties类读取配置文件中的信息，以key-value的形式读取并传回。

- Properties类是HashTable的子类


## 二、操作步骤：

（1）使用.properties配置文件进行读取：

- 创建Resource配置文件

    - 该文件是一个.properties为空的配置文件，可以在里面书写键值对，如xxx=xxx

    - 注意：等于号旁两个值不能有空格

```properties
key=value
...
name=jp233
```
- Java类：

```java
// 1 创建配置信息类
Properties prop = new Properties();

// 2 导入配置文件流
FileInputStream fis = new FileInputStream("<配置文件路径>");

// 3 加载流对应的文件
prop.load(fis);

// 4 通过配置文件中的key获取值
prop.getProperty("<配置文件中的key值>");  // return value
```


（2）使用.xml配置文件进行读取：

- 创建xml配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
  <comment>测试</comment>  <!-- comment为注释信息 -->
  <!--entry为多个键值对-->
  <entry key="name">jp233</entry>
  ...
  <entry key="phone">13717067350</entry>
</properties>
```
- Java类：

```java
// 其他步骤和方法都与properties配置读取一致
// 但是加载流对应文件时应当使用loadFromXML方法
properties.loadFromXML(inputStream);
```


（3）其他方法：

- 读取键集合：

    - 通过此方法可以返回一个配置文件所有键的set集合。

```java
Properties.ketSet();  // return Set<Object>
```





