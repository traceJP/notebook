# 一、介绍

- Lombok全名为Project Lombok。

- Lombok能以简单的注解形式来简化java代码，提高开发人员的开发效率。例如开发中经常需要写的javabean，都需要花时间去添加相应的getter/setter，也许还要去写构造器、equals等方法，而且需要维护，当属性多时会出现大量的getter/setter方法，这些显得很冗长也没有太多技术含量，一旦修改属性，就容易出现忘记修改对应方法的失误。

- Lombok涉及功能：

    - 自动变量类型


    - 自动判空NPE注解


    - 自动调用流close方法


    - 自动Getter/Setter/ToString/有参无参构造等方法


    - 标识不可变类注解（即对象创建后所有属性值不可以发生改变）


    - 自动建造者模式注解


    - 自动方法锁注解


    - 自动日志属性


- 在Intellij idea中有lombok对应的插件，需要配合使用。

- 官方文档：[projectlombok.org/features/all](http://projectlombok.org/features/all)

- 其他建议：

    - 不建议过度依赖lombok，使用lombok会使代码变得亚健康。

    - 其lombok的val-var等变量都不建议使用。

    - 只建议使用javabean相关注解，log相关注解，NonNull相关注解


# 二、使用

- 引入依赖jar包：

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version></version>
  <scope>provided</scope>
</dependency>
```


# 三、原理浅析

- 项目的源代码文件在经过编译处理以后，lombok会使用自己的抽象语法树去进行注解的匹配，如果在项目中的某一个类中使用了lombok中的注解，那么注解编译器就会自动去匹配项目中的注解对应到在lombok语法树中的注解文件，并经过自动编译匹配来生成对应类中的getter或者setter方法，达到简化代码的目的。

- 即lombok利用java文件在编译成class文件的途中，对源码进行了增改。


# 三、lombok-JavaBean注解

## 1、@Data

- @Data注解只能作用在类上，它会为类的所有属性自动生成setter/getter、equals、canEqual、hashCode、toString方法。如为final属性，则不会为该属性生成setter方法。

```java
// .java源文件
@Data
public class TestBean {
  private Integer id;
  private final String name = "test";
}

// junit
public void test() {
  TestBean bean = new TestBean();
  bean.setId(123);
  System.out.println(bean);
}
```
## 2、@Getter / @Setter

- @Getter / @Setter注解可以作用在类上，也可以作用在属性上。它只会为对作用的所有属性自动生成getter/setter方法。

```java
// 会为当前类下的所有属性添加getter/setter方法
@Getter
@Setter
public class TestBean {
  private Integer id;
  private String name;
}

// 会为id属性添加getter方法，为name属性添加setter方法
public class TestBean {
  @Getter
  private Integer id;
  @Setter
  private String name;
}
```
## 3、@EqualsAndHashCode

- 与Getter/Setter注解同理，只能作用在类上，为当前类自动重写equals方法和hashCode方法。

```java
@EqualsAndHashCode
public class TestBean {
  private Integer id;
  private String name;
}
```
## 4、@ToString

- @ToString注解只能作用在类上，它会自动为该类的所有字段生成toString方法。

```java
@ToString
public class TestBean {
  private Integer id;
  private String name;
}
```
## 5、@AllArgsConstructor

- @AllArgsConstructor注解只能作用在类上，会为当前类自动生成一个有参构造方法，参数为当前类的所有属性。如果只使用这个注解，当前类的无参构造方法将被覆盖。

```java
// TestBean类只有有参构造方法public TestBean(Integer id, String name);
@AllArgsConstructor
public class TestBean {
  private Integer id;
  private String name;
}
```
## 6、@NoArgsConstructor

- @NoArgsConstructor注解只能作用在类上，会为当前类自动生成一个无参构造方法。因为类默认自带无参构造方法。所以可以配合@AllArgsConstructor注解使用。

```java
// TestBean类有无参构造方法和有参构造方法
@AllArgsConstructor
@NoArgsConstructor
public class TestBean {
  private Integer id;
  private String name;
}
```
## 7、@Accessors(chain = true)

- @Accessors为开启链式调用注解，需要指定chain属性为true，需要本身有关Setter的方法或注解。其本质为将整个JavaBean改为建造者模式。即每个Setter方法都会返回当前的对象。在调用该JavaBean的setter方法时，即可直接在setter语句后继续setter。

```java
@Data
@Accessors(chain = true)
public class TestBean {
  private Integer id;
  private String name;
}

// 调用
TestBean bean = new TestBean();
bean.setId(123).setName("123");
```


# 四、lombok-日志注解

## 1、@Slf4j

- 当项目中使用了Slf4j日志框架时，在类上使用该注解，会自动为当前类中生成一个日志变量的属性，属性变量默认为log。即在当前类中自动生成了原框架的private Logger log = LoggerFactory.getLogger(this.getClass());

```java
@Slf4j
public class Test {
  public void out() {
    log.info("该方法已被调用");
  }

  // 使用占位符进行日志输出，可以使用[{}]大括号中括号的形式进行占位
  // 可以使用多个占位符，该方法为可变形参
  public void out1() {
    log.info("该方法已被[{}]调用", "123");  // 该日志方法已被[123]调用
  }
}
```
## 2、@Log4j

- 当项目中使用了Log4j日志框架时，也可以使用该注解简化开发的流程，具体参考官方文档。

```java
@Log4j
public class Test {
  public void out() {
    log.info("该方法已被调用");
  }
}
```



