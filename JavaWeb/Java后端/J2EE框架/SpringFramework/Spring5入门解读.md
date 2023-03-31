# 一、Spring框架总览

![clipboard.png](Spring5%E5%85%A5%E9%97%A8%E8%A7%A3%E8%AF%BB.assets/clip_image002.gif)

- Spring官方文档https://www.springcloud.cc/spring-reference.html

# 二、Spring框架概述

1、Spring是轻量级的开源的JavaEE框架

2、Spring可以解决企业应用开发的复杂性

3、Spring有两个核心部分: IOC和Aop

- IOC: 控制反转，把创建对象过程交给Spring进行管理

- Aop: 面向切面，不修改源代码进行功能增强


4、Spring特点

- 方便解耦，简化开发

- Aop 编程支持

- 方便程序测试

- 方便和其他框架进行整合

- 方便进行事务操作

- 降低API开发难度


5、下载Spring框架及配置环境变量：

- Spring下载后的文件目录


![clipboard.png](Spring5%E5%85%A5%E9%97%A8%E8%A7%A3%E8%AF%BB.assets/clip_image004.gif)

- 使用idea创建新的工程，然后讲Spring框架中libs中的jab导入到该工程模块的lib文件（需要自己创建）下


![clipboard.png](Spring5%E5%85%A5%E9%97%A8%E8%A7%A3%E8%AF%BB.assets/clip_image006.gif)

- 如图导入到idea中


![clipboard.png](Spring5%E5%85%A5%E9%97%A8%E8%A7%A3%E8%AF%BB.assets/clip_image008.gif)

- 在项目结构的依赖中导入


![clipboard.png](Spring5%E5%85%A5%E9%97%A8%E8%A7%A3%E8%AF%BB.assets/clip_image010.gif)

 

6、HelloWorld：

- 该HelloWorld示例一共包含3个文件：主类，调用类，配置Spring的xml文件。

- 具体过程为：主类用于构造主体代码部分；然后由xml文件配置Spring管理，在文件中由<bean>标记进行创建主类的对象；最后再由调用类加载xml文件和获得其中的对象。


（1）主类：
```java
package SpringHello;
public class HelloSpring {
  public void go() {
      System.out.println("HelloWorld!HelloSpring!");
  }
}
```
（2）Spring配置文件.xml：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans
      xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

  <!--配置行创建对象-->
  <bean id="Hello" class="SpringHello.HelloSpring" />

</beans>
```
（3）调用类：
```java
package SpringHello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class Test {
  public static void main(String[] args) {
      // 1 加载Spring配置文件
      ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
      // 2 获取配置创建的对象
      HelloSpring hello = context.getBean("Hello", HelloSpring.class);
      // 3 即可实现调用通过Spring框架创建的HelloSpring类的对象
      hello.go();
  }
}
```





