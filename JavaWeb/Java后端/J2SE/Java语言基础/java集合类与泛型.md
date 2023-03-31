# 一、JAVA工具集合类：

- Java集合的产生：
    - 为了在程序中可以保存这些数目本确定的对象, JDK中提供了一系列特殊的类,这些类可以存储任意类型的对象,并且长度可变,在Java中这些类被统称为集合。集合类都位于java.util包中,在使用时一定要注意导包的问题,否则会出现异常。

- 集合类和接口之间的关系：


![clipboard.png](java%E9%9B%86%E5%90%88%E7%B1%BB%E4%B8%8E%E6%B3%9B%E5%9E%8B.assets/clip_image002.gif)

 

![clipboard.png](java%E9%9B%86%E5%90%88%E7%B1%BB%E4%B8%8E%E6%B3%9B%E5%9E%8B.assets/clip_image004.gif)

 

- Collection：是单列集合类的根接口， 用于存储一系列符合某种规则的元素，它有两个重要的子接口，分别是List和Set。其中，List的特点是元素有序、元素可重复。Set的特点是元素无序，而且不可重复。List接口的主要实现类有ArrayList和.LinkedList，Set 接口的主要实现类有HashSet和TreeSet。


- Map：是双列集合类的根接口，用于存储具有键(Key)、值(Value)映射关系的元素,每个元素都包含一对键值，在使用Map集合时可以通过指定的Key找到对应的Value，例如5根据一个学生的学号就可以找到对应的学生。Map 接口的主要实现类有HashMap和TreeMaps


- 区别：Collecti on和Map接口的区别在于Collection中存储了一组对象,而Map存储——组键值对。为了便于初学者进行系统地学习，接下来通过一张图来描述整个集合类的继承体系，如下图所示。
- ArrayList：动态数组 （实现原理：链表），早期实现类为Vector（已淘汰）
- Iterator接口：（迭代器）

```java
/*Iterator迭代器已经被foreach淘汰，该接口即foreach的遍历原理*/

boolean hasNext();  // 如果迭代还有元素，则返回true

E next();  // 返回迭代中的下一个元素

Collection接口：（集合总接口）

boolean add(Object);  // 压入元素

boolean remove(Object);  // 弹出元素

int size();  // 返回集合中的个数

boolean isEmpty();  // 判断是否为空

boolean contains(Object)  // 查看是否包含某元素 

Iterator iterator();  //得到集合的Iterator对象
```
List接口：（线性表linear list）
```java
public interface List<E> extends Collection<E> {

  E get(int index);

  E set(int index, E element);

  void add(int index, E element);

  E remove(int index);

  int indexOf(Object o);

}
```


## 1、接口实现的栈和队列

（1）栈Stack：（算法：后进先出）

- 基本方法：

```java
Object push(Object item);  // 压栈

Object pop();  // 出栈

boolean empty();  // 判断栈中有没有对象元素
```
（2）队列Queue：（算法：先进先出）

- 基本方法：

```java
boolean offer(Object item);  // 入队

E poll();  // 出队
```


（3）LinkedList类（实现了栈和队列的接口，可以用一个类操作栈和队列的功能）
```java
LinkedList<Integer> Test = new LinkedList<Integer>();

/*LinkedList类作普通方法使用：随意进出*/
Test.add(1);Test.add(2);Test.add(3);Test.add(4);
System.out.println("随意进出：");
System.out.println("getFirst()返回第一个元素的值：" + Test.getFirst());
System.out.println("getLast()返回最后一个元素的值：" + Test.getLast());
System.out.println("检索并删除第一个元素" + Test.remove());
System.out.println("检索并删除最后一个元素" + Test.removeLast());
System.out.println(Test);

/*LinkedList类作栈方法使用：先进后出*/
Test.push(1);Test.push(2);
System.out.println("先进后出：");
System.out.println(Test.pop());
System.out.println(Test.pop());
System.out.println("--------");

/*LinkedList类作队列方法使用：先进先出*/
Test.offer(1);Test.offer(2);
System.out.println("先进先出：");
System.out.println(Test.poll());
System.out.println(Test.poll());
System.out.println("--------");
```




## 接口实现Set集：（不可重复集）

- Set接口：

```java
boolean add();  // 添加一个元素

boolean remove(Object o);  // 删除一个指定元素

int hashCode();  // 返回此集合的哈希码值: 计算公式 s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

boolean contains(Object o);  // 如果此集合包含指定元素，则返回true

int size();  // 返回集合中元素个数
```
- HashSet类：使用Hash表原理且实现Set接口的类


- TreeSet类：使用树原理且实现Set接口的类




## 接口实现Map集：（不可重复键-值对）

- Map接口：

```java
Map<K, V>  // 泛型：Key(键)，Values(值)

V put(K key, V value);  // 添加一个键值关联元素

V get(Object key);  // 返回指定键所对应的值

int size();

int hashCode();
```
- HashMap类：使用Hash表原理且实现Map接口

- TreeMap类：使用红黑树原理且实现Map接口


- Java集合类方法详细参考Java-API-Collection；方法实现原理参考：数据结构与算法；


# 二、泛型：（Generic）

（1）泛型类：
```java
public class A<T> {  // <>中可以有多个泛化参数，如<T,U>,在里面可以使用U a;等类型定义

  T a;  // 定义一个变量a，数据类型为泛化参数

  T add() {  // 定义一个方法add，返回值为泛化参数T
  }

  void add2(T t) {  // 定义一个方法add2，传入参数为泛化参数
  }

}

A<Integer> test = new A<Integer>();  // 实例化一个泛化类，将Integer类型代替泛化参数T
```
（2）泛型方法：
```java
public class A {

  void <T> add([T t]) {  // 定义一个泛化方法add，无返回值，可以传入一个泛化参数
  }

  <U> U add2() {  // 定义一个泛化方法add2，返回一个泛化参数U
  } 

}

A test = new A();  // 实例化类A
test.<Integer>add(5);  // 调用对象test中的泛化方法add，并指定泛化类型为Integer，传入一个int参数
```

- 
    注意：泛型代入只能使用java中被包装过的数据类型，类名（自定义的类实际上也可看作是自定义的数据类型）也是一个被包装过的数据类型


（3）对类型的限定：（有界泛型）

- 有界泛型和无界泛型

    - 无界泛型：如（1）（2）中使用的为无界泛型，该泛型传入的参数只能是java中被包装过的数据类型


    - 有界泛型：

```java
public class A<T extends 类或接口> {  // 该泛型T继承了一个类或者接口
  T a;
  T add() { }
  void add2(T t) { }
}

A test<继承类 | 继承类的子类 | 接口 | 实现接口的类> = new A<与定义一致>();  // 传入的泛型参数被限制了
```

- 
    使用 ？ 通配符进行泛型参数传入

    - 在java中，通配符"?"代表未知类型，在java中是不能直接对未知类型进行操作的，所以当不知道使用什么类型又想调用一些和数据无关的方法时 或者 用于泛型方法中不返回泛型类型的值时，可以将问号代入泛型参数

```java
A test<? [extends] [super] 类|接口> test = new A<包装类>();
```

- 
    extends和super的对比：

    - extends（继承父类）：

```java
// extends : 继承父类，即实例化的泛型参数可以使用Object或它的子类
fanxin<? extends Object> test2 = new fanxin<Integer>();

// super （子类超集）
// super : 子类超集，即实例化的泛型参数可以使用Integer或它的父类
fanxin<? super Integer> test = new fanxin<Object>();
```

- 
    总体对比：

```java
fanxin<? super Integer> test = new fanxin<Object>();
fanxin<? extends Integer> test2 = new fanxin<Integer>();
fanxin<?> test3 = new fanxin<>();  // new后的泛型可填可不填，反正也是未知类型

/*只有当使用super的时候无报错，其他均以未知类报错处理*/
test.a = 128;  // 自动装箱，无报错
test2.a = 1;  // 报错
test3.a = 1;  // 报错

/*以下三个对泛型中的函数调用均无报错*/
test.out();
test2.out();
test3.out();
```





