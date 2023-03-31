## 一、概念

#### 1、基本概念

- Stream API（java.util.stream）可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤、映射数据等操作。

- Stream是数据渠道，是用于操作数据源（集合、数组等）所生成的元素序列。与Collection集合的区别则是collection是一种静态的内存数据结构，讲的是数据，而Stream是有关计算的，讲的是计算。前者是主要面向内存，存储在内存中，后者主要是面向CPU，通过CPU实现计算。


#### 2、需要注意的是

- Stream自己不会存储元素。

- Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream。

- Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行（中间操作只有在终止操作存在时才会执行）。即一旦执行终止操作，就执行中间操作链，并产生结果

- Stream一旦执行了终止操作，就不能再调用其他中间操作或终止操作了。


#### 3、操作流程

（1）Stream的实例化

（2）一系列的中间操作

（3）执行终止操作

![截图.png](Stream%E6%B5%81.assets/clip_image002.gif)

## 二、基本使用

#### 1、创建Stream流（返回流）
```java
// 1 通过集合创建
List<T> list = ...
// 顺序流：按顺序操作，并行可以多线程操作
Stream<T> stream = list.stream();  // 返回一个顺序流 default Stream<E> strean()
Stream<T> stream = list.parallelStream();  // 返回一个并行流

// 2 通过数组创建
Interger [] arr = ...
Stream<Interger> stream = Arrays.stream(arr);  // Arrays静态方法

// 3 使用Stream静态方法
Stream<Interger> stream = Stream.of(1, 2, 3 ...);  // of传入可变形参
```
#### 2、中间操作（返回流）

（1）过滤：判断流中的集合记录是否都满足该条件，不满足则剔除。
```java
stream.filter(item -> {
  // 返回一个条件，例如 item.getAge() > 18
}).forEach(System.out::println);  // 使用 forEach 终止操作，打印结果
 
// 实例：过滤出年龄大于18的记录
stream.filter(item -> item.getAge > 18).forEach(System.out::println);
```
（2）截断：使其记录不超过给定的数量，留下前 N 个记录，丢掉其他记录
```java
stream.limit(n)  // n为截断的个数 int
  .forEach(System.out::println);

// 实例：取记录的前4条
stream.limit(4).forEach(System.out::println);
```
（3）跳过：丢掉前 N 个记录，留下剩下记录（与截断相反），若流中记录不满足 n 个，则返回一个空流，即所有记录都排除，没有任何元素符合。
```java
stream.skip(n)  // n为截断的个数 int
  .forEach(System.out::println);

// 实例：排除记录前4条
stream.skip(4).forEach(System.out::println);
```
（4）筛选：通过记录的hashCode和equals方法判断去除重复元素
```java
stream.distinct()  // stream的元素需要实现hashCode和equals方法，否则无法判断是否唯一
  .forEach(System.out::println);

// 实例：去除重复记录
stream.distinct().forEach(System.out::println);
```
（5）映射：将当前元素位置的值映射成返回的值。类似sql中的选择列。整个集合当前元素的值会被统一替换成映射的值
```java
stream.map(item -> {
  // 返回当前记录需要覆盖的值
}).forEach(System.out::println);

// 实例：获取员工姓名
// stream list => name,age,sex
stream.map(item -> item.getName).forEach(System.out::println);  // list => name
```
（6）排序：将集合排序
```java
stream.sorted()  // 自然排序，集合元素必须实现Comparable接口，否则报错
  .forEach(System.out::println);
stream.sorted((item1. item2) -> {  // 定制排序，通过参数实现比较方法
  // 返回比较值，类似Comparable接口
}).forEach(System.out::println);

// 实例：按年龄大小排序，可以使用方法引用 String::compareTo
stream.sorted((item1. item2) -> item1.getAge() - item2.getAge()).forEach(System.out::println);
```
#### 3、终止操作（返回其他结果类型）

（1）匹配：检查流中的元素是否符合给定匹配的条件，终止返回boolean结果
```java
// 1 allMathch检查是否匹配所有记录
boolean res = stream.allMathch(item -> {  // 终止
  // 返回比较值
});

// 2 anyMathch检查是否至少匹配一个记录
boolean res = stream.anyMathch(item -> {
  // 返回比较值
});

// 实例：检查流中是否所有员工年龄都大于18
boolean res = stream.allMathch(item -> item.getAge() > 18);
```
（2）查找：返回流中需要的信息
```java
// 1 findFirst 返回流中的第一个元素
Optional opt = stream.findFirst();  // Optional为Java中的一个包装类型
T res = res.get();  // 通过get方法可以取出原来的元素

// 2 count 返回流中的记录个数
int res = stream.count();

// 3 返回最大值 或 最小值 max | min
T res = stream.max((item1, item2) -> {
  // 返回比较值
});

// 4 流的遍历
stream.forEach(item -> {
  // 遍历操作
});
```
（3）规约：将流中的记录反复结合起来，得到一个值返回T，此时会返回流中遍历的当前记录和上一条记录
```java
T res = stream.reduce(n, (x1, x2) -> {  // n为记录的额外值
  // x1 为上条记录的值， x2 为当前记录的值
});

// 实例：list stream => 1, 2, 3, 4, 5 求集合的总和 + 10
int res = stream.reduce(10, (x1, x2) -> x1 + x2);  // res = 25

/* 过程：
1、 首先reduce将初始值10传给x1，x2取第一条记录1，所以 x1 + x2 = 11
2、 然后reduce保留上一次运算结束的记录（规约）赋值给x1=11，x2再取第二条记录2，所以 x1+x2=13
... 重复步骤 2 直到所有记录运算完毕返回规约结果
*/
```
（4）搜集：将流中的记录全部再次转换成集合返回
```java
T list = stream.collect(<需要收集成为什么>);

// 实例：将流全部收集成 list 集合
List<T> list = stream.collect(Collectors.toList());
```



