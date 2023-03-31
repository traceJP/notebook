# JavaReflection：

## 一、解释：

- Reflection (反射)是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

- 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象，这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为:反射。

- 反射相关API：java.lang.reflect.* ； java.lang.Class ；

- 作用：利用反射机制，可以调用运行时的类，甚至可以直接调用私有的属性、方法等等。


![clipboard.png](java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E8%AF%A6%E8%A7%A3.assets/clip_image002.gif)

- 通过反射机制直接访问私有类：
    - 为什么要破坏封装性去访问私有类：在绝大多数情况下，在编译过程中，无法确定自己当前需要new哪个类，这时候可以使用反射机制动态性的特点去new类。

- 关于java. ang. Class类的理解：


### （1）类的加载过程:

- 程序经过javac.exe命令以后，会生成一个或多个字节码文件(. class结尾)。接着我们使用java. exe命令对某个字节码文件进行解释运行。相当于将某个字节码文件加载到内存中。此过程就称为类的加载。加载到内存中的类，我们就称为 运行时类 ，此运行时类，就作为Cass的一个实例。（以当前的类作为对象，作为Class的对象，即Class的实例就对应着一个运行时类）


![clipboard.png](java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6%E8%AF%A6%E8%A7%A3.assets/clip_image004.gif)

#### （1.1）Class类说明：

- Class类是没有创建这一说法的，因为Class类的对象就对应着一个类。

- 即万物皆对象的概念，对于Class类而言，其他的类在这里就是一个对象。

- 外部类，匿名类，接口，数组，注解，基本数据类型等等，都可以有Class对象。 


### （2）获取Class的实例的方式：

- 调用运行时类的属性：.class

```java
Class name = 运行时类名.class;
```
- 调用运行时类的对象：

```java
Class name = new 运行时类.getClass();
```
- 调用Class的静态方法：

```java
Class name = Class.forName("类的路径");  // 当类的路径错误会抛出异常
```
- 调用类的加载器：ClassLoader

```java
ClassLoader ClassLoaderName = 当前类.class.getClassLoader();
Class name = ClassLoaderName.loadClass("类的路径");
```

### （3）创建运行时类的对象：

- newInstance()：调用此方法，创建对应的运行时类的对象。内部调用了运行时类的空参的构造方法（private权限或构造方法不存在将抛出异常）

```java
// 先获取Class的实例
Class name = 运行时类名.class;

// 创建对应的 运行时类名 对象
Object obj = name.newInstance();

// 可以通过强制转换对类名进行修饰
运行时类名 obj = (运行时类名) name.newInstance();

// 通过获取类时指定泛型从而指定类名返回值
Class<运行时类名> name = 运行时类名.class;
运行时类名 obj = name.newInstance();
```
#### （3.1）反射机制带来的动态性效果：

```java
/*该代码表现只有当运行时刻才能判断究竟new的是哪个对象*/
public void Test() {
  String classPash = "";
  int num = new Random().nextInt(2);  // 返回随机数0,1
  switch(num) {
    case 0:
      classPash = "java.util.Date";
      break;
    case 1:
      classPash = "Java.lang.Object";  // 注：随机到这会异常，因为Object类没有空参构造方法
      break;
  }
  try {
    System.out.println(getInstance(classPash));  // 输出new的类的toString方法
  } catch (Exception e) {
    e.printStackTrace();
  }
}

private Object getInstance(String classPath) throws Exception {
  Class name = Class.forName(classPath);
  return name.newInstance();
}
```

### （4）通过反射获取运行时类的完整结构：

- 以下例子都已通过Class test = 运行时类名.class进行获取Class实例。

- 获取属性结构：getFields()：获取当前运行时类及其父类中声明为public访问权限的属性

    - 属性：权限修饰符，数据类型，变量名等

```java
Field[] fields = test.getFields();  // 可以foreach遍历该fields
Field fields = test.getField(name:"<属性名>");  // 可以通过指定属性名单独获取该属性
```
- 获取当前运行时类中声明的所有属性。(不包含父类，不限制属性)

```java
Field[] declaredFields = test.getDeclaredFields();
```
- 获取运行时类中声明的单个属性的具体结构：

```java
Field[] fields = test.getFields();

for(Field f : fields) {

  // 1.获取该属性的权限修饰符
  int modifier = f.getModifiers();  // 返回int型0,1,2等数字
  System.out.println(Modfier.toString(modifier));  // 将int转化为public等字符

  // 2.数据类型
  Class type = f.getType();  // 返回Class.URL.数据类型
  System.out.println(type.getName());  // 使用getName去除多余返回

  // 3.变量名
  String fName = f.getName();  // 返回变量名字符串

}
```
- 获取当前运行时类的方法：

```java
Method[] methods = test.getMethods();  // 获取当前运行时类及其父类中声明为public访问权限的方法

Method[] declaredMethods = test.getDeclaredMethods();  // 获取当前运行时类中声明的所有属性。(不包含父类，不限制属性)
```

- 
    除了可以获取属性和方法外还能获取其他的如数组，构造方法等等，详细参考JDK-API文档


### （5）调用运行时类指定的属性：

```java
// 获取运行时类
Class test = 运行时类名.class;

// 创建运行时类的对象
运行时类名 p = (运行时类名) p.newInstance();

// 获取指定属性：要求该属性必须是public权限的
Field id = test.getField("属性名");

// 设置当前属性的值：
属性名.set(<参数1:指明设置哪个对象的属性>, <参数2: 将此属性值设置为多少>);  //如id.set(p,1001)

// 获取当前属性的值：
int pID = (int) id.get(p);
```
- 另一种模式：(常用)

```java
// 获取指定属性：可以是任何权限的
Field name = test.getDeclaredField("name");

// 在修改前需要 保证当前属性是可访问的
name.setAccessible(true);

// 设置当前属性的值：
name.set(p,"Tom");

// 获取当前属性的值：
String name = name.get(p);
```

### （6）调用运行时类指定的方法：

```java
// 获取运行时类
Class test = 运行时类名.class;

// 创建运行时类的对象
运行时类名 p = (运行时类名) p.newInstance();

// 获取指定方法：可以是任何权限的（也可以是仅public权限的，参考属性第一个代码块）
Method show = test.getDeclaredMethod("<指定获取方法的名称>", <形参名>);

// 在调用前需要 保证当前方法是可访问的
show.setAccessible(true);

// 调用方法：
show.invoke(<指名调用哪个对象的方法>, "<给方法赋形参>");  // 如show.invoke(p, "CHN");

// 方法的返回值：
返回值类型 a = show.invoke(...);  // invoke()的返回值即为对应类中调用的方法的返回值
```


## 反射与注解：

### （1）获取注解：

```java
Class test = 运行时类.class;

Annotation[] annotations = test.getAnnotations();  // 注解有多个，用数组
```

(2) 通过反射获取注解以及注解的value，这样就可以知道通过注解究竟需要做什么。





