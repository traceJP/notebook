框架定义：JQuery极大地简化了Javascript编程，写的更少，做得更多

- 框架开发意义：将JavaScript事件处理函数和网页内容分离。
- 可以先定义网页的内容，再集中书写事件处理函数。

JQuery官网：http://www.jquery.com

 

（1）使用前需要在官网上下载库文件

（2）用Script引入JS-JQuery框架库：
```html
<head>
    <script type="text/javascript" src="<JQuery库文件.js>"></script>
</head>
```
或者

（1）*使用CDN访问JQuery库（内容(C)分布(D)网络(N)）
```html
<!--通过互联网引用JS-JQuery框架库-->
<hard>
    <script type="text/javascript" src="框架库URL"></script>
</hard>
```
- 当前最新的百度JQuery库URL：
http://apps.bdimg.com/libs/jquery/2.1.1/jquery.min.js

- 优势：可以不必直接下载库文件就可以支持允许JQuery库

 

JavaScript-JQuery示例HelloWorld：
```html
<html>
  <hard>
        <script type="text/javascript" src="JQuery.js"></script>    <!--引入库文件-->
        <script type="text/javascript">
       $(document).ready( function() {  <!--事件处理函数统一写在document.ready中-->
         $("p").click( function() {  <!--当(p)标记被(click)单击时,调用以下匿名函数-->
           $(this).hide();  <!--当前对象this(指p标记),hide(隐藏)掉-->
         });
       });
     </script>
  </hard>
  <body>
        <p>HelloWorld</p>    <!--效果：当单击网页中的HelloWorld时，该段文字会消失-->
  </body>
</html>
```


JQuery基础语法：


（1）所有的JQuery代码都写在以下框架中：

- 表示：只有当所有的网页元素都加载完成之后，才开始执行JQuery代码。否则可能会出现网页元素还未全部加载就开始执行JQuery代码，导致调用元素出错，返回now。

```javascript
$(document).ready(function() {
  <JQuery代码块>
});
```
（1.1）JQuery选择器：

- 使用CSS选择器规范

- $(this)

 

（2）$(selector).action()

- selector："查询"和"查找" HTML元素（选择器，且符合css选择器规定）

- action()：执行对元素的操作

- 示例：

```javascript
$("p").click( function() {  <!--"p"在例子中代表selector，即某元素；click在例子中代表action(),即操作-->

  $(this).hide();

});
```




JQuery框架强大的应用：

![clipboard.png](JQuery%E6%A1%86%E6%9E%B6.assets/clip_image002.gif)

 

JQuery框架与JavaScript代码对比：



JQuery UI：

定义：

- UI：User Interface，用户界面

- JQueryUI是指基于JQuery开发的UI库

- 具体UI库可以在官网网址上查看：http://jqueryui.com/或http://www.jqueryui.org.cn/


基本操作概念：


（1）交互（Interactions）：与鼠标交互相关

- 缩放（Resizable）

- 拖动（Draggabl）

- 放置（Droppable）

- 选择（Selectable）

- 排序（Sortable）


（2）小部件（Widgets）：界面元素

- 折叠面板（Accordion）

- 自动完成（Autocomplete）

- 按钮（Button）

- 日期选择器（Datepicker）

- 对话框（Dialog）

- 菜单（Menu）

- 进度条（Progressbar）

- 滑块（Slider）

- 旋转器（Spinner）

- 标签页（Tabs）

- 工具提示框（Tooltip）


（3）效果库（Effeets）：动画效果

- 特效（Efreet）

- 显示（Show）

- 隐藏（Hide）

- 切换（Toggle）

- 添加Class（AddClass）

- 移除Class（RemoveClass）

- 切换Class（ToggleClass）

- 转换Class（SwitchClass）

- 颜色动画（ColorAnimation）


以上操作概念中都可对应使用js-JQuery选择器进行使用

- 案例：（Widgets中的自动完成部件）
```javascript
$( function() {
  var a = [1,2,3,4];  // 自动填充的数据源，在此例子中为一个数组a
/* 选中html中id=abc的标记，并且autocomplete启用自动填充，用source指定用于填充的数据源 */
   $("#abc").autocomplete( {  
  source:a
  });  
});
```


**基于UI的学习方法：**

1.   用户交互：事件

2.   界面风格：看demo

3.   关注数据来源：如上述例子数据来源为数组a，在开发过程中，更多的数据来源是源于服务器端数据，传输过来的数据格式之一就是JSON


多浏览源代码：对源代码进行本地化拷贝，分析理解学习和复用。



JSON：（JavaScript Object Notation）

定义：JSON是一种客户端和服务器之间进行数据传送的格式，这种格式的数据在JavaScript中可以很方便使用

- JavaScript Object Notation，JS对象标记

- 是一种轻量级的数据交换格式

- 在服务器与客户机之间交换数据

- 注意：json是一种数据格式，核心思想：对象信息的封装




对象的JSON基本格式：

- 以名值对作为一个基本的对象：

```json
var text = {"firstName":"Brett", "lastName":"MeLaughlin"}  // 两个 名值对 构成的一个对象
```
- 对象与对象之间的组合，构成一个巨大的对象：

```json
var text =
{  // 总对象：国家，省份
  "name" : "中国"
  "province" : [  // 省份对象：省份，城市
     {
     "name" : "江西",
     "cities" : {
       "city" : ["吉安", "南昌"]  // 城市对象：城市
     }
  }, {
     "name" : "广东",
     "cities" : {
      "city" : ["广州", "深圳"]
     }
  }]
}
/*
 \* 解读：这是一个包含着三层结构的对象：
 \* 在总对象中有两个名值对，一个是name：中国，一个是province：省份对象数组
 \* 在省份对象中，该对象是一个长度为2的数组，每个数组元素中包含了两个名值对
 \* 分别为：name江西，cities对象数组；name广东，cities对象数组
 \* ......
 */
```
- JSON数据格式的输出：
```
text.name  // 中国

text.province[0].name  // 江西

text.province[1].cities.city[1]  // 深圳
```


JSON详细解读：http://liyanblog.cn/articles/2012/09/28/1348813087862.html

 

 

