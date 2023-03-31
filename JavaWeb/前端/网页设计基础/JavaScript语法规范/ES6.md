## 一、概念

- ES全称EcmaScript，是脚本语言的规范，而平时经常编写的JavaScript，是EcmaScript的一种实现，所以ES新特性其实指的就是JavaScript的新特性。

- 官方文档：参见w3cschool官网
- 阮一峰ES6文档：https://es6.ruanyifeng.com/#docs

## 二、基本语法

### 1、let变量声明

- 定义一个变量，变量声明不能重复，块级作用域，不存在变量提升，不影响作用域链。
    - 变量提升：在传统JavaScript标准中，在开始运行时，系统会先收集代码中的所有变量，并统一定义并赋值为undefined。但在ES6中，不会统一收集赋值。
```javascript
let a;
let b,c,d;
let e = 100;
let f = 123, g = "456";
```
### 2、const变量声明

- 定义一个常量，同let类似。一定要赋初始值，常量值不可修改，但可修改数组和对象的元素，块级作用域
```javascript
const ABC = "123";
```
### 3、解构和赋值

- 数组的解构

    - ES6允许按照一定模式从数组和对象中提取值，对变量进行赋值，这被称为解构赋值。
```javascript
let F4 = ["小沈阳", "刘能", "赵四", "宋小宝"];

// 用方括号解构
let [xiao, liu, zhao, song] = F4;  // 四个变量对应F4数组中的四个元素
let [xiao, , zhao, song] = F4;  // 空出来可以跳过该元素对应的映射
```
- 对象的解构
```javascript
let zhao = {
  name: '赵本山",
  age: '不详",
};

// 用花括号解构
let {name, age} = zhao;  // 两个变量对应zhao对象的两个属性，跳过映射同理
```
- 函数形参的解构
```javascript
connect({  // 调用connect函数，传入一个对象；输出结果：不详
  name: '赵本山",
  age: '不详",
});

function connect({age}) {  // 定义connect函数，且形参为传入对象的age属性
  console.log(age);
}
```
### 4、模板字符串

- 使用模板字符串，即写字符串的时候用反引号``包裹字符串，能更方便的对字符串进行拼接。
```javascript
let a = "abc";
let str = `<p>  // 允许回车符在字符串中
    123
    </p>`;
let str = `<p>${a}</p>`;   // ${}引用变量，str = "<p>abc</p>"
```
### 5、简化对象写法

- 更加方便的对象内属性定义语法糖。
```javascript
let name = "jp";
let school = {
  name,  // 直接引用name变量作为属性，等价于name: name
  improve() {
    ...
  }  // 直接定义函数，等价于improve: function() { ... }
};
```
### 6、***箭头函数***

- 快捷定义函数语法糖。以此写法定义的函数与原始函数的区别如下：
    - 函数的this指针是静态的，this始终指向函数声明时所在作用域下的this值；而原始function函数定义的this指针是由谁调用则指向谁。

    - 函数不能作为实例化对象的构造方法。

    - 不能使用arguments变量。

        - arguments变量：JS函数自带的特殊变量，用于引用函数的实参。

```javascript
let fn = (a, b) => {  // 定义一个函数fn，形参为a, b
  return a + b;
}
let fn = () => { ... }  // 定义一个函数fn，没有形参

// 简写1）省略小括号，当形参有且只有1个的时候
let add = n => { ... }

// 简写2）省略h花括号，当代码体只有一条语句时
  // 此时return必须省略（语句的执行结果就是函数的返回值）
let add = n => n * n;  // 等价于 { return n * n; }
```
### 7、函数形参的默认值与解构赋值

- 可以给函数形参设置默认值，当不传递实参时，则形参直接使用默认值。

```javascript
funcation add(a, b, c = 10) {  // 形参c的默认值为10
  return a + b + c;
}
```
- 与解构赋值相结合

```javascript
connect({  // 调用connect函数，传入对象无age属性；输出结果：18
  name: '赵本山",
});

function connect({age = "18"}) {  // 解构 + 默认值为18
  console.log(age);
}
```
### 8、rest参数

- rest参数即可变形参。与Java可变形参类似。在ES6中主要用于代替arguments参数。

```javascript
function fn(a, b, ...c) {
  // c为数组，而arguments为对象
}
```
### 9、ES6扩展运算符

- [...] 扩展运算符能将数组转换为逗号分隔的参数序列。

```javascript
let a = ["小沈阳", "刘能", "赵四", "宋小宝"];
// 调用函数fn，并传入参数 ...a
// 此时a不是作为一个数组传入，而是作为4个参数传入
fn(...a);  // 等价于fn("小沈阳", "刘能", "赵四", "宋小宝");
```
- 其他用法

```javascript
// 1 数组合并
let a = [1, 2];
let b = [3, 4];
let c = [...a, ...b];  // 此时c = [1, 2, 3, 4];

// 2 数组克隆
let a = [1, 2];
let b = [...a];  // b = [1, 2];
```

## 三、基本对象

### 1、Symbol对象

- ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，是一种类似于字符串的数据类型。

- Symbol特点：

    - Symbol的值是唯一的，用来解决命名冲突的问题

    - symbol值不能与任何数据类型进行运算（包括自己）

    - Symbol定义的对象属性不能使用for...in循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名。

```javascript
// 只能使用函数创建Symbol（不能使用new创建）
let s = Symbol();
let s1 = Symbol("<描述字符串>");  // 描述字符串与Symbol无关

// 使用Symbol.for创建
let s3 = Symbol.for("<描述字符串>");
```

### 2、Promise对象

- Promise是ES6引入的异步编程的新解决方案。语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

```javascript
// 1 实例化promise对象，并指定异步方法体
let p = new Promise(
  function(resolve, reject) {
    // ...异步执行代码
    resolve("<返回值>");  // 调用resolve函数表示异步执行成功
    reject("<返回值>");  // 调用reject函数表示执行失败
  }
);

// 2 调用promise对象的then方法，设定异步回调函数
p.then(
  function(value) {  // 若异步函数成功，则执行此方法，value即返回值
    ...
  },
  function(reason) {  // 若异步函数失败，则执行此方法，reason即返回值
    ...
  }
);

// 3 调用promise对象的catch方法，捕获异步函数抛出的异常
p.catch(
  function(reason) {  // 当异步函数抛出异常或执行失败时，执行此方法
    ...
  }
);
```
- 多个异步函数的嵌套（解决原JS的回调地狱问题）

    - 当调用promise对象的then方法时，该方法返回的是promise对象，并且对象的状态由回调函数的执行结果决定。

        - 如果回调函数中返回结果是非promise类型的属性，则状态为成功，返回值为对象的成功值。

        - 如果回调函数中返回结果是promise对象，则内部的promise对象状态决定外部then方法返回的promise对象状态。（嵌套关系是由内决定外）

```javascript
// 实例化promise对象，并指定第1个执行的异步方法体
let p = new Promise(
  function(resolve, reject) {
    resolve("成功");
  }
);

// 1*** 返回非promise对象；result则为包装了状态和值属性的promise对象
let result1 = p.then(
  function(value) {  // 异步函数成功
    return value;
  },
  ...
);

// 2*** 返回promise对象；嵌套执行其他异步方法
let result2 = p.then(
  function(value) {  // 异步函数成功
    // 内部的第2个执行的异步方法体（嵌套第2层）
    return new Promise(
      function(resolve, reject) {
        // 若调用resolve方法，则result2为成功
        // 若调用reject方法，则result2为失败
      }
    )
  },
  ...
);
```
- 采用ES6的写法的promise对象

```javascript
let p = new Promise((resolve, reject) => {
  // ...执行异步函数
}).then((value) => {
  // ...成功执行函数
}, (reason) => {
  // ...失败执行函数
}).catch(reason) {
  // 异常捕获执行函数
};
```


### 3、迭代器和集合对象

#### （1）迭代器

- 迭代器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署lterator接口就可以完成遍历操作。

- for...in和for...of

```javascript
let arr = [a, b, c, d];  // 声明一个数组

// 1 使用for...in迭代数组
for(let v in arr) {
  // v的值每次为1 2 3 4（v值代表数组的key）
}

// 2 使用for...of迭代数组
for(let v of arr) {
  // v的值每次为a b c d（v值代表数组的value）
}
```


#### （2）集合

- Set对象

```javascript
let s = new Set();
let s2 = new Set([]);  // 使用数组作为初始Set
```
- Map对象

```javascript
let m = new Map();
```


## 三、类关键字

- 与常规编译语言的类关键字类似。可以参考Java笔记。


## 四、ES6模块化

- 模块化是指将一个大的程序文件，拆分成许多小的文件,然后将小文件组合起来。通过暴露和导入变量，从而达到防止命名冲突、代码服用、高维护等优势。


### 1、暴露（export）

- 暴露js文件中的变量给其他js文件使用

```javascript
// 1 分别暴露
export let school = "123";  // 暴露变量
export function fn() { ... };  // 暴露函数

// 2 统一暴露
let school = "123";
function fn() { ... };
export {school, fn};  // 统一暴露变量和函数

// 3 默认暴露（只能暴露一个对象）
export default {
  // ...需要暴露的对象属性
}
```
### 2、导入（import）

- 导入其他js文件中暴露的变量，即可在本js文件中使用。

```javascript
// 1 导入全部模块
import * as <别名> from "<导入的js文件路径>";  // as定义别名

// 2 解构赋值导入
import {school} from "<导入的js文件路径>";

// 2.1 加上as定义别名
import {school as <别名>} from "<导入的js文件路径>";

// 3 导入默认暴露变量（必须使用as指定别名）
import {default as <别名>} from "<导入的js文件路径>";

// 4 普通形式导入
import <变量> from "<导入的js文件路径>";
```





