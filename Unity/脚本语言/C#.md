 

## 一、特殊语法

### 1、可变形参：

- 使用关键词params修饰方法参数（类似Java中的...三个点修饰）
```c#
void function(params int nums[]) {  // 放在参数列表最后一个
  ...
}
```
### 2、ref和out

- 使用关键词ref或out修饰方法参数（类似C++中&符号修饰参数，可以使得传入参数为一个指针）

- 无修饰：只进不出

- ref：既进又出

- out：只出不进
```c#
int b = 0, c = 0;

functionREF(b);  // b = 1
 
functionOUT(c);  // c = 2

void functionREF(ref int a) {
  a++;
}

void functionOUT(out int a) {
  a = 1;  // 必须要给a进行赋值
  a++;
}
```
### 3、类属性的getter/setter封装 

- 在c#中严格区分属性和字段的区别：

    - 属性：带有getter/setter方法

- 字段：

    - 在vs中使用快捷键ctrl+R+E直接快速生成lambda表达式的getter/setter方法
```c#
class People {
  // 定义字段
  private int age;
  // 定义属性并使用{}指定get/set方法
  // 使用属性的大驼峰命名
  public string Name {
    get {
      // 直接引用name即可
      return name;
    }
    set {
      // 默认带有value参数，当设置值的时候调用此方法
      name = value;
    }
  }
}
```
### 4、属性特性

- 类似Java中的@注解标识符，可以为属性标明一些注解
```c#
class People {
  // 使用[<注解名>]进行标注
  [Serialization]  // 标注序列化字段
  private string name;
}
```
### 5、继承中的方法重写

- 在c#中重写继承父类的方法需要加上特殊的关键字，但是实现接口则不需要。
```c#
class Fu {
  // 使用virtural定义虚函数
  public virtual void test() {
    // ...
  }
}

// 继承和实现接口都写在冒号后面
// 与Java一致，单继承多接口实现，建议优先写继承
class Zi : Fu {
  // 使用override重写父类虚函数
  public override void test() {
    // ...
  }
}
```
### 6、Map集合

- 在c#中，Map集合称为字典（Dictionary）。
```c#
// 创建一个字典
Dictionary<T1, T2> dic = new Dictionary<T1, T2>();
```
### 7、键值对

- 类似算法第四版中的背包，可以存储任意类型的键值对。
```c#
// 创建一个hash表
Hashtable hs = new Hashtable();

// 调用api时传递的实参可以是任意类型
hs.Add(Object key, Object value);
```

