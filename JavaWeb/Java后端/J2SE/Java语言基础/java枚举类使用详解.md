枚举：
- 在java中，枚举（enum）的方式以类的形式出现。

- 定义枚举类：（在JDK1.5之前）


```java
class meiju1 {

    private final String name;
    private final String about;
    
    private meiju1(String name, String about) {
        this.name = name;
        this.about = about;
    }
    
    public static final meiju1 NO1 = new meiju1("张三", "nb");
    public static final meiju1 NO2 = new meiju1("李四", "wc");
    public static final meiju1 NO3 = new meiju1("王五", "qwq");
    public static final meiju1 NO4 = new meiju1("老六", "asa");
    
    // 需要重写toString方法以便于直接输出对象
    @Override
    public String toString() {
        return name + "\t" + about;
    }
}
```
- JDK1.5之后，使用enum关键字（类比上述）
    - 枚举类enum隐性继承java.lang.Enum类

```java
enum meiju2 {
    // 注意：枚举元素必须放在enum枚举类的第一排，否则会报错
    NO1("张三", "nb"),    // 每个枚举元素之间用逗号隔开
    NO2("李四", "wc"),
    NO3("王五", "qwq"),
    NO4("老六", "asa");  // 最后一个用分号
    
    private final String name;
    private final String about;
    
    private meiju2(String name, String about) {
        this.name = name;
        this.about = about;
    }
    
    @Override
    public String toString() {
        return name + "\t" + about;
    }
}
```
- 枚举类的调用：
    - 因为每个枚举元素本质上是一个static final的对象，所以一般 类名.枚举标签 使用即可。

```java
meiju1.NO1;
```

