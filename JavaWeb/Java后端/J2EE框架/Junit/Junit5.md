# 一、概述

- junit是java的一个测试框架

- JUnit 5 = JUnit平台+ JUnit Jupiter + JUnit Vintage

- 只支持Java8


# 二、依赖

- junit5依赖

```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-api</artifactId>
  <version>5.7.0</version>
  <scope>test</scope>
</dependency>
```
- junit5参数化测试依赖

```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-params -->
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-params</artifactId>
  <version>5.7.0</version>
  <scope>test</scope>
</dependency>
```
- junit5运行引擎依赖

```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine -->
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-engine</artifactId>
  <version>5.7.0</version>
  <scope>test</scope>
</dependency>
```
- junit4依赖

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>
```


# 三、使用

- HelloWorld：

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import example.util.Calculator;
import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

  // new一个类
  private final Calculator calculator = new Calculator();

  // 使用@Test注解标明这是一个测试方法：在idea中直接运行即可
  @Test
  void addition() {
    // 断言，第二个参数一定等于第一个参数
    assertEquals(2, calculator.add(1, 1));
  }
}
```





