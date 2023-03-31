# Javascript-ES6规范：

https://es6.ruanyifeng.com/#docs

 

JavaScript概念：

1.JavaScript是一种常见的客户端脚本

2.脚本(Script)是一种介于HTML与编程语言(如Java、C++等) 之间的特殊语言

 

JavaScript定义：JavaScript是一种嵌入HTML文档中的、跨平台、基于对象和事件驱动的脚本语言

 

JavaScipt运行原理：

在客户端执行脚本，需要在客户机上有使用脚本的应用程序和脚本引擎

- 脚本引擎 (Script Engine)：通常是一个库或库的集合，完成解释脚本的具体任务
    - Jscript.dlI  （此文件在系统安装时会一并安装）

运行过程：脚本可被嵌入HTML文档， 嵌入了脚本的HTML文档加载到浏览器内的解释器上，浏览器将把脚本程序交给脚本引擎执行



基于对象：即面对对象设计（oop）

一些基于JavaScript的输出语句：
```javascript
alert();  //输出语句，可在浏览器上弹出一个对话框
document.write("字符串");  //向网页输出一串字符串
```

JavaScript中的类和对象都与java一致（包括构造方法等）


JavaScript中的一些对象及主要属性：

事件驱动：（在一些事件触发后，驱动Javascript对事件进行处理）

事件驱动需求分析：当什么时候做什么事  //javascript的一切事件驱动都要围绕这个句型

事件处理：

（1）处理事件的程序或函数称为事件处理程序

（2）JavaScript常用事件：

1. onClick：鼠标左键单击页面对象时发生

2. onLoad：网页载入浏览器时发生，发生对象为HTML的<body>标记

3. onUnload：用户离开当前页面时发生，发生对象为HTML的<body>标记
4. onMouseOver：鼠标移动对象上时发生

5. onMouseOut：鼠标离开对象上时发生

 

嵌入HTML文档：（与css嵌入html类似）

（1）行内式：（通常用于临时测试一个事件采用的方法）
```html
<p onClick="alert(1)">2</p>    //当鼠标单击网页中的2数字时，会弹出数字1的对话框
```
（2）嵌入式：
```html
<script [language="javascript | jscript"]>
  JavaScript代码
</script>
```
或者采用更加兼容的嵌入式处理：
```html
<script [language="javascript | jscript"]>
<!-- 
  JavaScript代码
-- >
</script>
```
（3）链接式：（JavaScript与html文件分离）
```html
.htm文件：
    <script src="JavaScript文件名 | 文件URL"></script>
.js文件:
  JavaScript代码
```

Javascript基本语法：

代码书写格式：

1. 行结束标志：回车换行  以及   ；

2. 几行代码写在同一行中，实用分号( ；)隔开

3. 其他书写格式，以及代码注释都与java一致

 

Javascript数据类型：与java一致，但数据类型必须都用var进行定义和声明。

**与c语言数据类型最大区别：Javascript允许一个变量或常量在使用时确定它的数据类型**

**与c语言数据类型最大区别：Javascript允许一个变量或常量在使用时确定它的数据类型**

**与c语言数据类型最大区别：Javascript允许一个变量或常量在使用时确定它的数据类型**

 

Javascript常量：与java语言一致。

 

<pre>文字内容</pre>   //域格式标记，可以保留文字内容中的源代码，如保留n

 

Javascript变量：

命名规则、保留字：与java语言一致。

 

变量的声明与赋值：

***在JavaScript中统一使用var声明变量并分配存储空间**

如var a = 1;var b = "张三";var c = true;


全局变量：

- 省略var，或在函数外声明

局部变量：

- 在函数内声明

全局变量可在整个脚本中被使用，可在不同的窗口(指定窗口名)

```javascript
function test{
  a = 1   //a为全局变量,其中a在之前是没有被定义过的
  var b = 2  //b为函数test的局部变量
}
```


JavaScript运算符以及运算符优先级：与java一致，但有两个特殊的运算符in，instanceof

（1）比较运算符in：（用于判断某个对象中是否有相应的html属性）
```javascript
语法格式：op1 in op2

op1为字符串或可以转换为字符串的数据类型；op2为数组或对象；如果op1是op2的属性名或之一，则返回true，反之则返回false。

var a = {title:"Informatice",author:"Tang"} //a可理解为一个已经被实例化的对象，且该对象有两个属性

表达式"title" in a 则返回true  //表明a对象中有title属性

表达式"age" in a  则返回false  //表名a对象中没有age属性
```


（2）比较运算符instanceof：（用于判断一个对象是否为某个类的实例）
```javascript
var a = new Date();

表达式a instanceof Date  则返回true
```

控制语句：与java语言一致，如if...else,for等等

（1）with语句：（对象操作语句，为一段语句组建立默认的对象）

语法格式：
```javascript
with(对象名){

  <语句组>

}
```
例如：
```javascript
例1

with(document){           //选择document为对象名

write("限时抢购物品: <br>")     //此时write("");相当于document.write("");

write("ViewSonic 17"显示器。<br>")

write("EPSON打印机。<br>") }

例2

document.write("限时抢购物品: <br>")

document.write("ViewSonic 17" 显示器。<br>")

document.write("EPSON打印机。<br>")

//这里例1等同于例2
```


（2）for...in语句：重复执行指定对象的所有属性（遍历对象的属性）
```javascript
语法格式：（java语言中的foreach）

for(变量名 in 对象名){

  {语句组}

}
```
例如：
```javascript
function member(name, gender){  //构造函数member

this.name = name

this.gender = gender

}

function showProperty(obj, objString){

var str = ""   //定义一个局部字符串变量str

for (var i in obj)  

  str += objString + "." + i +"=" + obj[i]+ "<BR>" 

return str

}

papa = new member("杨宏文","男生")  //建立对象实例papa

document.write(showProperty(papa,"papa"))  //输出语句

7.8行代码运行过程：for...in遍历时，先将对象papa的第一个属性name赋值给i，然后执行第八行ObjString+"."+i 执行为 ObjString.name  ;  obj[i]  执行为  papa[name] 即papa对象的值 ； 执行一遍后再返回for...in语句进行第二次遍历，此时将对象papa的第二个属性gender赋值给i，继续执行第八行，最后return字符串变量str。

输出结果：

papa.name=杨宏文

papa.gender=男生
```


JavaScript的函数：

语法格式：
```javascript
function 函数名(形参1,形参2,...形参n){

  <语句组>

}
```
与java函数的最大区别就是JavaScript定义函数不需要声明函数的返回类型，只需要使用function定义即可

 

JavaScript的对象：

Javascript是一个脚本语言，对象包含属性和方法两种成分。

（1）自定义对象：

JavaScript不需要通过class进行类的设计，只需要直接通过finction进行构造函数即可，如
```javascript
function 对象名称(形参1,形参2,...形参n){

  this.属性1 = 形参1

  this.属性2 = 形参2

  ...

  this.方法1 = 函数名1

  this.方法2 = 函数名2

  ...

}
```
例题如下：
```javascript
function student(a,b,c){  //a,b,为形参，且不需要约定数据类型

  this.a = a   //JavaScript中的类成员a,b,c

  this.b = b

  this.c = c

  this.putout = student_putout  //将外部函数包含在类中

}

function student_putout(){  //通过function创建一个输出函数

  document.writeln(this["a"]);  //通过实例化后的下标进行类成员调用

  ...

}

通过以上构造函数即可直接实例化对象和函数

如MyStudent = new student("1001","张三","男");  MyStudent.putout();
```


（2）字符串对象(String对象)：

1.   anchor(链接名)：创建HTML中的anchor标记

2.   big()：以大字体显示字符串

3.   small()：以小字体显示字符串

4.   italic()：以斜体字显示字符串

5.   bold()：以粗体字显示字符串

6.   blink()：字符串闪烁显示

7.   fixed()：以固定字高显示字符串

8.   fontsize(size)：控制字体大小

9.   toLowerCase()：将字符串改变为小写

10.  toUpperCase()：将字符串改变为大写

11.  indexOf(str,start-position)：在start-position查找str字符串

12.  subtring(start,end)：返回start与end位置之间的字串

 

（3）Math对象（主要可以用于做一些网页计算器）：

Math对象的一些属性：

E：常数e，Math.E=2.718...

1.   LNI0: 10的自然对数，Math.LN10=2.302...

2.   LN2： 2的自然对数，Math.LN2=0.693...

3.   LOG2E：以2为底E的对数，1.442...

4.   LOG10E：以10为底E的对数，0.434...

5.   PI:圆周率，3.141...

6.   SQRT1_ 2： 0.5的平方根，0.707...

7.   SQRT2： 2的平方根，1.414...

Math对象的主要方法：

1.   sin(), cos()： 正弦、余弦

2.   asin(), acos()：反正弦、反余弦

3.   tan(), atan()： 正切、反正切

4.   sqrt()：平方根

5.   pow(by,ev)：以bv为底的ev次方

6.   abs()：绝对值

 

Date对象（与时间相关的对象）：

date对象是一个只有方法的对象，且该对象必须实例化才可以实用

 

（4）Date对象的主要方法：

GAT系列：

1.   getYear()：返回年号

2.   getMonth()：返回月份数，其值为0~11, 0代表1月

3.   getDate()：返回日期，其值为1~31

4.   getDay()：返回星期，其值为0~6, 0表示星期日

5.   getHours()：返回小时数，其值为0~23

6.   getMinutes()：返回分钟数，其值为0~59

7.   getSeconds()：返回秒数，其值为0~ 59

8.   getTime()：返回表示时间的整数，该时间从1970年1月1日午夜开始以毫秒为单位进行计算

SET系列：

1.   setYear(timevalue)：设置年份，timevalue为大于1900的整数

2.   setMonth(timevalue)：设置月份数，timevalue的值为0~11，0代表1月

3.   setDate(timevalue)：设置日期，timevalue的值为1~31

4.   setDay(timevalue)：设置星期，timevalue的值为0~6, 0表示星期日

5.   setHours(timevalue)：设置小时数，timevalue的值为0~23

6.   setMinutes(timevalue)：设置分钟数，timevalue的值为0~59

7.   setSeconds(timevalue)：设置秒数，timevalue的值为0~59

8.   setTime(timevalue)：设置时间，timevalue为从1970年1月1 日午夜开始以毫秒为单位进行计算

 

JavaScript在客户端上的扩展：

 

JavaScript核心语言：

Javascript客户端扩展：

- 浏览器对象模型（BOM）-将浏览器看作一个对象


- 文档对象模型（DOM）-将浏览器中的网页看作一个对象

    - 浏览器中页面上的对象




**一、浏览器对象模型：**

![clipboard.png](JavaScript%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

window对象：（window对象是BOM的顶层对象，指的是浏览器窗口对象）

（1）主要属性：name（浏览器窗口的名字）、opener（在新的窗口点击超链接，对于被打开的浏览器，他的opener就是打开它的浏览器）、self（当前浏览器自己）、top（浏览器窗口层层打开，当访问这些浏览器窗口的top属性时，会访问到最顶层的浏览器）

（2）主要方法：close()、open()、 alert()、 confirm()、 prompt()、 setTimeout()、clearTimeout()、setInterval()、 clearInterval()、moveBy()、moveTo()、 resizeBy()、 resizeTo()

- close()、open()方法：关闭以及打开浏览器窗口

- alert()、 confirm()、 prompt()：三种对话框

- setTimeout()、clearTimeout()、setInterval()、 clearInterval()：与计时器相关的方法

- moveBy()、moveTo()、 resizeBy()、 resizeTo()：与浏览器窗口的移动和改变大小有关


（2.1）主要方法概述：

1.1. open()：打开一个新的浏览器

- open(page_url[,window name][,window_features])


page_url: 打开窗口中所要显示的页面的URL

- window_name: 给该窗口起一个名字

- window_features: 窗口的特征，以字符串形式给出，包含一至多个选项，选项之间用","相隔


返回一个对所打开窗口的引用

- window选项:（窗口特征：可以是下面的一些项）

- width:窗口宽度像素值

- height:窗口高度像素值

- toolbar:是否有工具栏，yes|no (缺省)

- menubar:是否有菜单栏，yes|no (缺省)

- location:是否有地址栏，yes|no (缺省)

- status:是否有状态栏，yes|no (缺省)

- scrollbars:是否产生滚动条，yes|no (缺省)


1.2. close()：关闭当前打开的浏览器窗口

- window.close()  // 一般网页会提示当前浏览器想关闭该窗口，是否同意


2.1. alert()：创建一个只有确定按钮的对话框

- alert(shown_string)

- shown_string:显示在对话框中的字符串文本


返回值：无返回值 

2.2. confirm()：创建一个带确定和取消按钮的对话框

- confirm(shown_string)

- shown_string: 显示在对话框中的文本信息


返回值:用户点击确认，返回true;用户点击取消，返回false

2.3. prompt()：创建一个带确定、取消按钮及输入字符串字段的对话框

- prompt (shown_string, default_string)

- shown_string: 弹出对话框中的提示信息

- default_string: 弹出对话框中文本输入框的缺省值


返回值:用户点击确定，返回文本输入框中的信息;用户点击取消，返回"null'

3.1. setTimeout()：设置一个时间控制器

- setTimeout(function_string, msec)

- function_string: 定时器到时调用的函数名

- msec:定时器时间，单位为ms


3.2. clearTimeout()：清除原来时间控制器的时间设置

- window.clearTimeout()




（3）主要事件：

onLoad（加载）、onUnload（卸载）、 onBeforeUnLoad、OnResize（重定义大小）、OnScroll（滚动）、OnError（发生错误）

JavaScript代码中，属性或方法的默认对象名是window对象

 

location对象：（页面的URL地址，是window和document的属性）

（1）常用属性：hash、 host(URL中的主机)、 href、 pathname(URL中的路径)、 port、 protocol、 search

其余对象属性可以使用模板进行测试：
```html
<html>
<head>
<title>...</title>
    <script language= "JavaScript" >
         alert(location.href);  //通过输出语句输出location对象代表的值
    </script>
</head>
<body>
</body>
</html>
```
（2）主要方法：replace()、reload()  ;

- 
    history对象：

    - history.go(-1) 或 history.back()      //使浏览器倒退到之前的页面

    - history.go(1)  或 history.forward()    //使浏览器前进到下一个页面

    - history.go(0)  或 location.rcload)     //刷新浏览器


-  navigator对象：

    - 属性：userAgent（显示浏览器信息，可通过改属性对浏览器类型进行判断）


-  document对象：（window对象下层中使用最多的是document对象，指的是当前窗口显示的Web页面）


（1）主要属性：（可以通过这些属性输出或者改变原设置的值）

- alinkColor：页面中活动超级链接的颜色

- bgColor：页面背景颜色

- fgColor：页面前景颜色

- linkColor：页面中未曾访问过的超级链接的颜色

- vlinkColor：页面中曾经访问过的超级链接的颜色

- lastModified：最后一次修改页面的时间

- location：页面的URL地址

- title：页面的标题


（2）主要方法：

- clear()：清除文件窗口内的数据

- close()：关闭文档

- open()：打开文档

- write()：向当前文档写入数据

- writeln()：向当前文档写入数据 + " n "


（3）主要事件：(这些事件可以在html-input标记中当作属性使用，如onmouseover="<方法块>")

- onClick：在页面上单击鼠标左键时发生

- ondblClick：在页面上双击鼠标左键时发生

- onMouseDown：在页面上按下鼠标左键时发生

- onMouseMove：在页面上移动鼠标时发生

- onMouseOut：鼠标离开对象时发生

- onMouseOver：鼠标移到对象上时发生

- onMouseUp：放开鼠标左键时发生

- onSelectStart：开始选取对象内容时发生

- onDragStart：以拖曳方式选取对象时发生

- onChange：(文本框的常用事件)焦点移出时发生



**二、文档对象模型：**

定义：DOM模型由元素节点（element node）构成。一段HTML代码对应一段DOM树，每个HTML元素都是DOM树中的一个结点（node）。

如这样一段html代码：
```html
<html>
<head><title></title></head>
<body>
    <table>
        <tr> <td> </td> <td> </td> </tr>
        <tr> <td> </td> <td> </td> </tr>
  </table>
</body>
</html>
```
展开成树之后：(这颗树就是DOM树)
```
html------head
          ------title
    ------body
          ------table
                ------tr
                      ------td
                      ------td
             ------tr
                      ------td
                      ------td    
解释：它定义了用户操纵文档对象的接口
```


node的常用属性：

1.   nodeName: String   (结点名称一般大写)

2.   nodeValue: String   （节点的值）

3.   nodeType: Number   （节点的类型）

4.   firstChild: Node  // 该节点的first子节点

5.   lastChild: Node  // 该节点的last子节点

6.   childNodes: NodeList   (子节点列表，0起始)

7.   parentNode: Node  // 父节点

8.   previousSibling: Node  // 前序节点

9.   nextSibling: Node  // 后序节点

10.  attributes: NameNodeMap

1.上述节点中，子节点父节点前序后序节点的类型都为Node节点类型

2.其中第6条childNodes(节点集合)<java中的List集合类型>：对于集合类型的数据属性，并不能直接访问节点的属性，必须通过下标的方法（类似数组的方法）访问到其中的一个元素，再访问到这个元素相应的节点属性。

 

node的常用方法：
```javascript
hasChildNodes()  //判断某一个节点是否有子节点，返回Boolean

---

appendChild(node)  // 追加节点，返回Node

removeChild(node)  // 删除节点，返回Node

replaceChild(newnode, oldnode)  // 替换节点，返回Node

insertBefore(newnode, efnode)  // 插入节点，返回Node

/*3-6行操作的都是当前节点的子节点。*/

3-6行带来的增删改查节点效果，意味着网页源代码的变化，网页源代码的变化意味着前端的显示就变了，用户就会觉得网页变了，网页也就动起来了。
```




DOM模型在网页设计中的强大应用：（对网页进行增删改查）

![clipboard.png](JavaScript%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

 

（1）访问指定节点：
```javascript
getElementById()  // 通过节点的ID获得节点

getElementsByName()  // 通过节点的名字获得节点

getElementsByTagName()  // 通过节点的标记获得节点

getElementsByTagName("*")  // 仅火狐浏览器支持！返回所有元素的节点的集合

注意：第2,3行getElement后面加了s，即复数，所以2,3返回的是节点集nodeList，所以需要使用数组下标的方式去访问指定的节点。
```


（2）访问元素属性：

- 注意html元素的属性，不是document的属性
```javascript
getAttribute(name)  // 获得指定属性的值

setAttribute(name, value)  // 未指定属性赋值

removeAttribute(name)  // 删除指定的属性
```


（3）访问相关节点：
```javascript
var htmlnode=document.documentElement;  // 访问html节点

var headnode=htmlnode. firstChild | childNodes[0];  // 访问head节点

var bodynode=htmlnode. lastChild | childNodes[1];  // 访问body节点

// 访问body节点还可以直接使用document.body访问
```
（3.1）访问子、父、兄弟节点：

- childNodes、firstChild、lastChild、parentNode、previousSibling、nextSibling


（4）检查节点的类型：

- HTML中节点的类型：元素节点、属性节点、文本节点

- 不同的节点类型，相应的nodename、nodevalue的取值是不一样的


（5）创建节点：
```javascript
createElement()  // 创建元素节点

createTextNode()  // 创建文本节点

createDocumentFragment()  // 创建文本片段
```
- 创建实例：
```javascript
var a = document.createElement("p")  // 创建一个<p></p>节点

var b = document.createTextNode("This is a text line")  // 创建一个文本节点

a.appendChild(b)  // 将文本节点b放入<p>节点a中（追加函数）

// 1:<p></p> 2:"This is a text line" 3:<p>This is a text line</p>

注意：代码第3行，节点创建完毕后一定要使用appendChild()函数声明创建好的节点应该放在html代码的具体哪个位置(具体哪个标记后面)。
```


（6）操作节点：
```javascript
appendChild()  // 追加

insertBefore()  // 替换

replaceChild()  // 插入

removeChild()  // 删除

// 通过这些操作方法，就可以定位到html代码中具体位置进行增删改查
```
（6.1）操作CSS样式：

- 访问或修改行内样式（nde.styled）

- 样式名符合CSS规范

- 不带 - 的样式名称直接使用，带 - 的样式名称去掉并且非首词的首字母大写(font-size 改为 fontSize)

- float样式：火狐浏览器—style.cssFloat、IE浏览器—style.styleFloat

- style只能操作行内样式 （改变行内样式足以使用，因为行内样式优先级是最高的）


（6.2）获取元素最终的CSS样式

- IE浏览器：元素的curentStyle属性

- 火狐浏览器：document.defaultView.getComputedStyle方法






