### 一、Java中指针的体现：

定义：在java中，引用的实质就是指针。（该指针它是受控的，是安全的）


c语言与java指针对比：

（1）传地址 -> 对象

- 在java中，引用类型本身就相当于指针，可以用来修改对象的属性、调用对象的方法。


（2）指针运算 -> 数组

- *(p+5) 在java中可以用args[5]

（3）函数指针 -> 接口、Lambda表达式

- Lambda表达式用->箭头进行函数地址赋用


（4）指向结点的指针 -> 对象的引用

- 链表的实现，用数据结构传入节点，在java中可以直接赋值对象


（5）使用JNI

- Java Native Interface(JNI)技术，它允许Java代码和其他语言写的代码进行交互。




### 二、java新语句新特性&语法糖:

（1）java中的foreach语句：

- 解释：foreach语句的循环会将集合中的值逐个赋给变量名，每次赋值相当于一次循环


实现原理：数据结构util.Iterator实现代码原理
```java
//变量的定义只能在for中定义，遍历的集合一般为Item型数组
for (Item 变量名 : 元素所在集合（数组）名) {     
  <语句块>       //通过调用变量名对数据进行处理
}
```


（2）Java断言语句：
```java
assert condition
```
- 这里condition是一个必须为真(true)的表达式。如果表达式的结果为true，那么断言为真，并且无任何行动

- 如果表达式为false，则断言失败，则会抛出一个AssertionError对象。这个AssertionError继承于Error对象，而Error继承于Throwable，Error是和Exception并列的一个错误对象，通常用于表达系统级运行错误。



```java
asser condition:expr;
```
- 这里condition是和上面一样的，这个冒号后跟的是一个表达式，通常用于断言失败后的提示信息，说白了，它是一个传到AssertionError构造函数的值，如果断言失败，该值被转化为它对应的字符串，并显示出来。


（3）循环中的label标签：

- 解释：通过label标签可以使内循环中的条件可以直接跳出外循环。

```java
que:   //此程序中的label标签为que。
    for (int i = 0; i < 5; i++) {
      for (int j = 0; j < 5; j++) {
        for (int k = 0; k < 5; k++) {
           count++;
           //break;          //ij,一共循环5次       ijk.一共循环25次
           //break que;        //ij,一共循环1次       ijk.一共循环1次
           //continue;         //ij,一共循环25次      ijk.一共循环125次
           //continue que;       //ij,一共循环5次       ijk.一共循环5次
        }
      }
    }
```


（4）不规则二维数组：

- 解释：java的多维数组可以看作是数组中的数组

```java
int[][] n = new int[5][];  //只有最后维度可以待定不给值，其他都要给

int[0] = new int[3];

int[1] = new int[4];

//相当于二维数组中第一行有3列，第二行有4列....
```


（5）方法的可变参数列表：

- 解释：在参数列表中使用格式为 数据类型...形参名  可以使API中传入的实参可变。

- 原理：利用数组实现传入可变个参数。

```java
public float avg(int ... nums){   //传入int类型的可变参数，参数名为nums
  for(int x : nums)   //foreach遍历传入的参数
    sum += x;
}
```
- 调用： avg(10,20,30); avg(3,4,5,6,7,8,9)  //无论多少实参都可以


- 可变参数只能作为方法参数列表中的最后一个参数

- 避免和方法体重载一起混合使用，以免引起歧义


（6）java中的第四种权限标识符 default：

- 解释：不加任何权限就是第四种权限（即默认访问权限），也称default或package权限。类似于c++中的友元类。

- default权限表示可以被本身和同一个包中的类所访问。在其他包中定义的类即使是这个类的子类也不能访问这些成员。

```java
package Greek;
class Alpha{
  int iampackage;  //default权限
}

package Greek;
class Beta{
  void accessMethod(){
    Alpha a = new Alpha();
    a.iampackage = 10;  //合法访问
  }
}
```


（7）super关键字：

- 解释：与this关键字类似，super关键字指向该关键字所在类的父类，用来引用符类中的成员或方法以及构造函数。

```java
super(参数1,...参数n);  //引用父类的构造函数
super. <变量名> | <函数名()>  //引用父类的成员变量以及成员方法
```

### 三、java不能这么用:

（1）java中的 % 取余运算符可以取浮点数。
```java
9.5 % 3 = 0.5
```


（2）java中任何数据类型（包括基本数据类型和复合类型）都可以用比值运算符（==,!=）进行比较

（3）java中的判断表达式必须得到一个逻辑值。不能用一个单纯的数值来代替，如：

- c语言中：

```c
int x = 3;
if(x) {...}
```

- 
    而java中必须采用：

```java
int x = 3;
if(x != 0) {...}
```


（4）java中的数组变量之间赋值是引用赋值，不能实现数组数据的复制。
```java
int[] a = new int[6];
int[] b;
b = a;   //此时是将b的指针指向a，而不是将a的所有数据复制给b
```


（5）java中没有全局变量的概念。java中使用的是static变量（即静态变量），静态变量是在一个类的所有实例对象中都可以访问的变量。

（6）java中boolean表达式判断：

- 基本类型：

    - 数值类型：转换后比较

    - 浮点数，最好不直接用 == 进行比较 （会有误差，极其容易判断错误）

    - Double.NAN==Double.NAN结果为false  （Double.NAN用来表示不定量的数，即不是一个数的数）

    - boolean型无法与int相比较

- 封装数据类型：

```java
Integer i = new Integer(10);
Integer j = new Integer(10);
System.out.println(i==j); //false ,因为对象是两个

Integer m= 10;  // 自动装箱
Integer n = 10;
System.out.println(m==n); //true ,因为对象有缓存

/*
自动装箱原理：在java中自动装箱需要经过value函数，在函数内有一个缓存，凡是经自动装箱得到的-128到+127的数都会进行缓存
*/

Integer p= 200;  // 超过-128到+127，无缓存
Integer q = 200;
System.out.println(p==q); //false ,因为对象是两个
```
- 枚举类型：

    - 内部进行了唯一实例化，所以可以直接进行运算符判断

- 引用对象：

    - 是直接看两个引用是否一样

    - 如果要判断内容是否一样,则要重写equals方法

    - 如果重写equals方法,则最好重写hashCode()、overwrite()方法

        - 原理：该方法原理是将要判断的两个对象计算出一个值，再判断该值是否相等

- String对象：

    - 判断相等,一定不能使用 == 进行判断 ,要用equals方法

    - 但是字符串常量(String literal)及字符串常量会进行内部化(interned) ,相同的字符串常量是相等的

        - 原理：当两个字符串是一样的时候，编译器会将这两个标识都引用到同一个字符串常量上去

```java
String hello = "Hello", lo = "lo";

System.out.println(hello == "Hello"); //true, "Hello"和hello中的"Hello"发生了类补化，即同一个字符串合并到一个字符串上去了

System.out.println(Other.hello == hello); //true ，其他类中的hello与上述进行对比，也会发生类补化

System.out.println(hello == ("Hel"+"lo")); //true，先执行"Hel"+"lo"，此时两个字符串合并成一个字符串常量，再进行类补化对比

System.out.println(hello == ("Hel"+lo)); //false，引用是不会被类补化的

System.out.println(hello == new String("Hello"); //false，两个引用是肯定不相等的

System.out.println(hello == ("Hel"+lo).intern(); //true，intern()函数：求得它类补化之后的字符串，即合并完之后进行类补操作
```





