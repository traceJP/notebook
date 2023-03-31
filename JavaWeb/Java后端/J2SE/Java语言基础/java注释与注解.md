## 一、注解：

- 从JDK 5.0开始，Java增加了对元数据(MetaData)的支持,也就是Annotation(注解)

- Annotation其实就是代码里的特殊标记，这些标记可以在编译,类加载,运行时被读取,并执行相应的处理。通过使用Annotation,程序员可以在不改变原有逻辑的情况下，在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者进行部署。

- Annotation可以像修饰符一样被使用,可用于修饰包，类，构造器,方法，成员变量，参数，局部变量的声明，这些信息被保存在Annotation的"name=value”对中。

- 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Android中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗代码和XML配置等。

- 未来的开发模式都是基于注解的，JPA是 基于注解的，Spring2 5以上都是基于注解的，Hibernate3.x以后也是基于注解的，现在的Struts2有一 部分也是基于注解的了，注解是一-种趋势，一定程度上可以说：框架=注解+反射+设计模式。


### 1、常见的Annotation示例：

- 使用Annotation时要在其前面增加@符号,并把该Annotation当成一个修饰符使用。用于修饰它支持的程序元素


（1）生成文档相关的注解
```java
@author  // 标明开发该类模块的作者，多个作者之间使用,分割
@version  // 标明该类模块的版本
@see  // 参考转向，也就是相关主题
@since  // 从哪个版本开始增加的
@param  // 对方法中某参数的说明，如果没有参数就不能写
@return  // 对方法返回值的说明，如果方法的返回值类型是void就不能写
@exception  // 对方法可能抛出的异常进行说明，如果方法没有用throws显式抛出的异常就不能写
```
- 其中

    - @param @return @exception这三个标记都是只用于方法的。


    - @param的格式要求: @param形参名形参类型形参说明


    - @return的格式要求:@return返回值类型返回值说明


    - @exception的格式要求: @exception 异常类型异常说明


    - @param @exception可以并列多个


（2）在编译时进行格式检查（JDK内置的三个基本注解）
```java
@Override  // 限定重写父类方法，该注解只能用于方法
@Deprecated  // 用于表示所修饰的元素(类,方法等)已过时。通常是因为所修饰的结构危险或存在更好的选择
@SuppressWarnings  // 抑制编译器警告
```


## 二、自定义注解：

（1）自定义Annotation概述：

- 定义新的Annotation类型使用@interface关键字

- 自定义注解自动继承了java.lang.annotation.Annotation接口

- Annotation的成员变量在Annotation定义中以无参数方法的形式来声明。其方法名和返回值定义了该成员的名字和类型。我们称为配置参数。类型只能是八种基本数据类型、String类型、Class 类型、enum类型、Annotation类型以上所有类型的数组。

- 可以在定义Annotation的成员变量时为其指定初始值,指定成员变量的初始值可使用default关键字

- 如果只有一个参数成员，建议使用参数名为value

- 如果定义的注解含有配置参数，那么使用时必须指定参数值，除非它有默认值。格式是“参数名=参数值”，如果只有一个参数成员，且名称为value,可以省略“value= ”

- 没有成员定义的Annotation称为标记;包含成员变量的Annotation 称为元数据Annotation

- 注意：自定义注解必须配上注解的信息处理流程（反射解释注解）才有意义。


（2）注解定义
```java
public @interface 注解名 {
  // 参数为一个时，可以直接使用String value(); 多个时即使用数组String[]
  String[] value();  // 这是一个属性，不是一个方法

  // 给参数赋默认值
  String value() default "默认值";
}
// 如果上述仅仅只是定义注解但没有成员，表名是一个标识作用。
```
（2.1）可以参考@SuppressWarnings定义

（3）使用注解：
```java
// 有默认值可以不用括号：@注解名
@注解名(value = "参数值")
```


## 三、元注解：

- 元注解：用于修饰注解的注解

    - 元数据：例如String name = "JP";  // “JP”就是数据，String name用于修饰JP，那么String name 就是元数据


- JDK提供的4钟元注解：

```java
// JDK5.0提供了4个标准的meta-annotation类型：
@Retention
@Target
@Documented
@Inherited
```
（1）@Retention

- @Retention：只能用于修饰一个Annotation定义，用于指定该Annotation的生命周期，@Rentention包含一个RetentionPolicy类型的成员变量。

- 使用@Rentention时必须为该value成员变量指定值：

    - RetentionPolicy.SOURCE：在源文件中有效(即源文件保留),编译器直接丢弃这种策略的注释。

    - RetentionPolicy.CLASS（默认）：在class文件中有效(即class保留)，当运行Java程序时，JVM不会保留注解。这是默认值。

    - RetentionPolicy.RUNTIME：在运行时有效(即运行时保留),当运行Java程序时, JVM会保留注释。程序可以通过反射获取该注释。


![clipboard.png](java%E6%B3%A8%E9%87%8A%E4%B8%8E%E6%B3%A8%E8%A7%A3.assets/clip_image002.gif)

 

（2）@Target

- @Target：用于修饰Annotation定义，用于指定被修饰的Annotation能用于修饰哪些程序元素。@Target也包含一个名为value的成员变量。


![clipboard.png](java%E6%B3%A8%E9%87%8A%E4%B8%8E%E6%B3%A8%E8%A7%A3.assets/clip_image004.gif)

- 如该代码块：

```java
// 这个自定义的注解MYinterface只能用于构造器和方法的修饰
@Target({METHOD,CONSTRUCTOR})
public @interface MYinterface {
  String value();
}
```

- 
    一般在自定义注解的时候都会使用@Retention和@Target进行修饰


（3）@Documented

- @Documented：用于指定被该元Annotation修饰的Annotation类将被javadoc工具提取成文档。默认情况下javadoc是不包括注解的。

    - 定义为Documented的注解必须设置Retention值为RUNTIME。


（4）@Inherited

- @Inherited：被它修饰的Annotation将具有继承性。如果某个类使用了被@Inherited修饰的Annotation,则其子类将自动具有该注解。


## 四、注释：

- 使用cmd自动生成一个关于源代码的说明文档：

    - 解释：利用cmd的javadoc命令可以将java源代码和文档注释生成一个HTML格式的文档。


java文档注释符：
```java
/**
	<注释内容>
*/
```
- (文档注释是行注释和块注释衍生出来的注释方法)

- 在java程序中的文档注释将会自动包含在HTML格式文档中。


```shell
javadoc <java程序名>.java
```



