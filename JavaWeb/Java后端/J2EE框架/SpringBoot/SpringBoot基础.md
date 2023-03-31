# 一、SpringBoot概念

- SpringBoot是整合整个Spring生态圈的一站式框架，是简化Spring技术栈的快速开发脚手架。
- SpringBoot是一个高层框架，底层本质是Spring框架。
- Springboot能快速创建出生产级别的Spring应用。
    - 创建独立pring应用
    - 内嵌web服务器（自带tomcat，直接运行即可）
    - 自动starter依赖，简化项目构建配置
    - 自动配置spring以及第三方跨王家功能
    - 提供生产级别的监控，健康检查以及外部化配置
    - 无代码生成，无需编写xml文件

# 二、依赖配置

- 使用maven配置SpringBoot应用：


```xml
<!-- 1 引入springboot父工程 -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.4.2</version>
</parent>

<!-- 2 引入SpringBoot依赖：注意依赖无需写版本号 -->
<dependencies>
  <!-- springboot依赖 -->
  <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```


# 三、HelloWorld

- 使用SpringBoot快速搭建一个J2EE应用


## 1、定义一个SpringBoot的启动类
```java
@SpringBootApplication  // 这是一个springboot应用
public class MainApplication {
  // Springboot启动入口，运行main方法即可启动应用
  public static void main(String[] args) {
     SpringApplication.run(MainApplication.class, args);
  }
}
```
## 2、直接编写Controller层代码即可
```java
@RestController
public class HelloController {
  // 注意：SpringBoot应用下，映射的地址直接为ip，无需再加上工程名
  @GetMapping("/hello")
  public String helloWorld() {
     System.out.println("请求已成功接收");
     return "HelloSpringBoot！！！";
  }
}
```
## 3、访问ip地址
```http
http://localhost:8080/hello
```


# 四、依赖管理

- 在传统springmvc中，使用maven创建一个web应用往往需要导入一大堆的jar包依赖配置。而在SpringBoot中拥有版本仲裁机制。

- 由父项目做依赖管理，父项目几乎声明了所有开发中常用的依赖版本号，子项目继承后则不需要声明版本号。所以在SpringBoot应用中，开发无需关注版本号。


## 1、修改springboot默认版本号
```xml
<!-- 只需要在当前项目的pom文件中使用properties标签指定即可覆盖 -->
<properties>
  <xxx.version>版本号</xxx.version>
  <!-- eg.修改lombok依赖为1.4.2 -->
  <lombok.version>1.4.2</lombok.version>
</properties>
```
## 2、Starters场景启动器

- SpringBoot为所有项目整合制定了各种场景，如web应用场景、分布式场景等。

- 而springboot通过选择引入某个场景，那么这个场景中所有常规需要的依赖都会被自动引入。

- 场景启动器分类：

    - 官方的场景启动器命名规则为spring-boot-starter-*，如上面依赖的spring-boot-starter-web。

    - 第三方场景启动器命名规则为*-spring-boot-starter，如thirdpartyproject-spring-boot-starter。

- 版本号的继承：

    - 继承场景启动器后，若该场景中仲裁了相关依赖，那么在本身的项目中可以不写版本号。若没被仲裁，则需要手动写版本号。

    - 其原理是利用了maven中的依赖传递就近原则。


# 五、自动配置

## 1、概述

- 分析上述HelloWorld中可知，在SpringBoot中搭建一个web应用开发环境，所有有关Spring、SpringMVC的xml文件/包扫描等都不用书写。这是因为SpringBoot在引入Starters场景启动器的时候，根据场景为我们自动配置了这些。


## 2、SpringBoot为我们自动配置的东西

- 使用启动类getBean获取已经自动配置的bean，查看输出即可。

- 如输出中的dispatcherServlet（web.xml配置），characterEncodingFilter（字符编码过滤器）等。

```java
ConfigurableApplicationContext context = SpringApplication.run(MainApplication.class, args);

// 获取所有已经装配的bean
String[] beans = context.getBeanDefinitionNames();
for (String bean : beans) {
  System.out.println(bean);
}
```
## 3、总览Spring-Web场景下SpringBoot自动配置的东西

- 引入了tomcat依赖，并自动配置tomcat

- 引入了SpringMVC全套组件，并自动配好SpringMVC常用功能组件

- 自动配置了web相关的常见功能

- 自动配置了包扫描结构（主类所在的包及其下面的所有子包内的所有组件都会被默认扫描并自动装配）


## 4、自动配置原则

- 按需加载原则：

    - SpringBoot拥有几乎所有常见组件功能的自动配置，而这些配置是按需加载的，即引入了某个场景，这些场景的自动配置才会开启。

- 其自动配置的所有组件内元素都拥有默认值，而所有默认配置最终都是映射到MultipartProperties上。配置文件的值最终会绑定到某个类上，而这个类则会在容器中创建对象。


## 5、修改自动配置的部分参数

- 直接通过在resources资源目录下使用以application为名字的properties文件或yaml文件，并在其中指定修改即可。

- 具体自动配置可修改参数可以参考源码或者参考官方文档-应用程序属性

    - https://docs.spring.io/spring-boot/docs/2.4.3/reference/html/appendix-application-properties.html#common-application-properties


## 6、自动配置所在依赖包

- spring-boot-starter依赖下的spring-boot-autoconfigure依赖


 

 

 