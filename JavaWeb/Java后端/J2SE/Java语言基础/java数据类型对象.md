## java中  int 与 integer 的区别

- int与integer的区别从大的方面来说就是基本数据类型与其包装类的区别：

- int 是基本类型，直接存数值，而integer是对象，用一个引用指向这个对象


1. Java 中的数据类型分为基本数据类型和复杂数据类型int 是前者而integer 是后者（也就是一个类）；因此在类进行初始化时int类的变量初始为0.而Integer的变量则初始化为null.

2. 初始化时：
```java
int i =1；
Integer i= new Integer(1);  // 要把integer 当做一个类看
```
- 但由于有了自动装箱和拆箱，使得对Integer类也可使用：Integer i= 1；　　　　  

    - 　　int 是基本数据类型（面向过程留下的痕迹，不过是对java的有益补充），Integer 是一个类，是int的扩展，定义了很多的转换方法


    - 　　类似的还有：float Float;double Double;string String等，而且还提供了处理 int 类型时非常有用的其他一些常量和方法


    - 　　举个例子：当需要往ArrayList，HashMap中放东西时，像int，double这种内建类型是放不进去的，因为容器都是装 object的，这是就需要这些内建类型的外覆类了。


    - 　　Java中每种内建类型都有相应的外覆类。


- Java中int和Integer关系是比较微妙的。关系如下：


　　1. int是基本的数据类型；

　　2. Integer是int的封装类；

　　3. int和Integer都可以表示某一个数值；

　　4. int和Integer不能够互用，因为他们两种不同的数据类型；

- 举例说明：

```java
ArrayList al=new ArrayList();
int n=40;
Integer nI=new Integer(n);
al.add(n);  //不可以
al.add(nI);  //可以
/*
并且泛型定义时也不支持int: 如：List<Integer> list = new ArrayList<Integer>();可以 而List<int> list = new ArrayList<int>();则不行
*/
```

- 总而言之：如果我们定义一个int类型的数，只是用来进行一些加减乘除的运算or作为参数进行传递，那么就可以直接声明为int基本数据类型，但如果要像对象一样来进行处理，那么就要用Integer来声明一个对象，因为java是面向对象的语言，因此当声明为对象时能够提供很多对象间转换的方式，与一些常用的方法。自认为java作为一们面向对象的语言，我们在声明一个变量时最好声明为对象格式，这样更有利于你对面向对象的理解。





