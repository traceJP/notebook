html语法学习网站：https://www.w3school.com.cn/

 网页和网站的区别：

- 网页是指带有html等代码的页面

- 网站是指服务器上某个目录下的全部目录和文件，如网页、图片、音频、视频等

    - 综上，网站包含于网页

- 静态网页和动态网页的区别：

（1）静态网页是不包含服务器端代码的html文件，Web服务器只是负责把静态网页发送给浏览器,由浏览器解释执行

（2）动态网页中含有服务器端代码，需要先由Web服务器对这些服务器端代码进行解释执行生成客户端代码后再发送给客户端浏览器



# 一、我的第一个网页： 

-  超链接：web网页的核心特征，即点击跳转到其他网页

- 超文本：超文本(HyperText)是一种电子文档,其中的文字包含有可以链接到其他字段或者文档的超文本链接,允许从当前阅读位置直接切换到超文本链接所指向的文字。

-  html超文本标记语言：超文本标记语言HTML作为一种语言,它具有语言的一般特征,即它是一种符号系统，具有自己的词汇(符号)和语法(规则)。

    - 注：标记语言是不同与编程语言的，所谓标记就是 作记号。

- HTML文件的结构：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

- 标记(Tags) : HTML文档中一些有特定意义的符号,这些符号指明内容的含义或结构。


（1）在一对尖括号之间

```html
<标记名称>HTML内容</标记名称>

例: <h2>你好! </h2>
```

（2）分为“起始标记”、"结束标记”，名称相同，结束标记在名称前有/

（3）大小写无关(最好小写)

- html的标记分为单标记和配对标记:
    - 单标记最好自封闭：``` <br/><img/> <hr/> ```等

    - ``` <br/>```：表示换行。


- 自封闭：写法如上述类似，认为他将起始标记和结束标记写在了一起。标记的名称与大小写无关，且标记可以联合使用和嵌套使用。
    - 如<p><b>粗体字</b></p>为一个严格的嵌套标记格式

- */标记可以带有一个或多个属性参数：
    - <标记名称 属性1="属性值1" ... 属性n="属性值n">内容</标记名称>
    - 注意：标记名称后面的属性和属性值是成对出现的，称该成对出现方式叫做名值对。

- 注释标记：如c语言中的//以及/**/注释使用类似：<!--注释内容-->


- 元素:把HTML标记(如<p.....</p>) 和标记之间的内容组合称为元素

（1）构成HTML文档的基本部件

（2）一般起始标记是元素的开始，结束标记表示元素的结束

（3）有内容的元素、空元素

（4）内容:文字或其他元素，如下例，p元素的内容就是b元素＋粗体字//且其中一共有2个元素<p><b>粗体字</b></p>

# 二、网页中的元素：

（1）文本<p>: ```<p>文本内容</p>```

（2）图像、音频、视频

（3）图像标记<img> ：```<img src="图像URL" alt="文字说明" title="文字提示"```

-  align="left|middle|right|top|bottom" border="图像边框宽度(像素) 


- "width="图像宽" height="图像高">

- src：图像的位置路径或互联网上的超链接

- alt：当图像位置寻找失败或加载不出时显示的文字说明

- title：将鼠标悬停在图像上时显示的内容

- 网页中支持的图像格式: gif、jpg、jpeg、png


（4）超链接<a>：```<a href="互联网链接|路径">文字内容</a>```

- 可选属性：target="_blank"//在新窗口打开，而不是原窗口打开

- 链接源：文字、图像

 

（5）锚(anchor)：(链接到文档指定位置)

先用<a>进行取名，使其插入一个锚：

```<a name="该点锚的名称"></a>```

再在跳转锚点处使用<a>插入路径跳转至该锚点：

```<a href="网页链接|路径#锚点名称"></a>```


HTML块标记：没有预定义格式，只是给内容打了一个标记

```<div>文本内容</div>        //块级元素```

```<span>文本内容</span>   //内联级元素```

- 与文本标记<p>的区别：<p>标记是有预定义的格式的，其中<p>的段落之间是有明显的间距。而<div>标记的文本段落之间基本没有间距。

重要概念：

块级元素(block) & 内联元素(inline)

- 块级元素独占一行，内联元素可与其他元素共享一行

- 块级元素自上向下排列，内联元素自左向右排列且自动换行

- 补充：即在浏览器中使用<div>进行标记，则文本的内容只占一行，标记结束后自动换行。而使用<span>进行标记后该行还可以添加其他元素。



# 三、地址的相对路径和绝对路径：

-  URL：所有的网址形式都是web在资源上的定位形式。这些资源地址被称为URL ( Uniform Resource Locator，统一资源定位器)，用于在Internet上定位资源。


- 如：```<a href="http://www.baidu.com"></a>```这之中的```http://www.baidu.com就是一个URL```



- 
    相对的URL：地址不包含主机名

- 绝对的URL：包含了主机名和协议名的格式，如 http_URL="http:"  "//"  host [ ":" port ] [abs_path  ["?"  query] ]




- 相对URL和绝对URL与基URL的关系举例：

    - 基URL： http://localhostdashboard/week2/2-2- 1.html   //其中红色字体为网页的目录(站点)

    - 相对URL： w3c-logo.jpg




相对目录中常用的符号：

- ..   //父目录，即上一级目录

- .    //当前目录

- /    //根目录，也叫子目录，即下一级目录




base标记：（改变网页的基URL）

设置网页基URL：

```<base href="新的基URL地址" />```

设置默认链接打开方式：

```<base target=" _blank" />  //此时网页中的所有链接都将在新窗口中打开```

 

 

网页的样式：样式就算web页面内容在浏览器中的显示方式。

基本样式：字体、字号、前景色、背景色

布局样式：页边距、元素在页面上的位置

 

HTML文字样式标记：

文字样式标记:

```<font>...</font>```

常用属性：face（字体），size（字号），color（文字前景色）

 

html特殊文字样式标记：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

# 四、HTML布局样式、属性：

（1）标记

```<center>...</center> ``` 

//居中标记，为内容打上center标记后，内容在浏览器中会以居中显示呈现

（2）属性

```<h1 align="left | center | right">标题文字</h1>    //align:对齐属性```

页面背景色:
```html
<body bgcolor=#aa8cc祝贺你，初始成功  //bgcolor：背景颜色
 </body>
```
文字前景色```: <div color=red>文字内容</div>  //color：前景颜色```


style属性：(规范的标记属性)
```html
<body style="background-color:yellow">

<p style="font-family:arial;color:red;font-size:20px;">A paragraph.</p>

//background-color 背景颜色 ; font-family 字体 ; font-size 字体大小 ;

*/其中用style属性定义样式的方式是CSS样式中的一种使用方式
```

- 
    HTML的不足：


1. html的规则不严格，如使用标记时有太多的自由

2. 内容含义和显示格式混合

 

# 五、XHTML

综上，发明了XHTML(更规范的html)：

- 标记嵌套结构,如``` <div><span></span></div>```

- 标记和属性名称必须小写，属性值大小写不做要求

- 所有属性值都必须加引号，即使是数字也需要加引号，且不能省略属性值

- 必须有尾标记，即使是单个标记也要使用" />",如``` <br />,<img />```

- 样式定义使用CSS,不适用样式标记和属性

- 标记语言目前的三大版本：HTML4.01（标记、属性、属性值），XHTML（更严格），HTML5（富媒体：多媒体标记、支持交互）



# 六、CSS基本使用

CSS：css是针对html对样式的定义太过灵活与混乱而提出的一种技术

（Cascading Style Sheets）级联样式表

CSS是html的扩展标准，扩展了html在排版和文字样式上的功能


CSS的开发目的（希望能将内容与样式分离）：

把网页展现的样式从网页中独立出来集中管理，如果需要改变网页样式，只需要改变样式设定部分，HTML文件本身不需要修改。

（此时可以使用一个文件用于书写css样式代码，一个文件用于写html代码，然后使用html代码调用css样式代码。可以理解为c语言中的函数关系）

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image006.gif)

优势：可以使用一个文件编辑css字体样式，一个文件编辑css颜色样式。然后由主网页html调用这两个css样式文件（分工，使网站代码更加规范简洁）


目标：对网页内容进行样式定义。

对哪些内容进行样式定义?

- 指定网页-->应用结构

- 指定内容-->选择器

什么样式?

- 规则：样式名称、样式取值


CSS的创建：

将样式表应用到Web页面中的4种方法：

（1）行内式(内联样式表) :直接定义HTML标记中的style属性

```<HTML标记名称 style="样式属性1:属性值1;样式属性2:属性值2; ...">```

（2）嵌入式(内部样式表) :在网页内的```<style></style>```之间定义CSS样式
```html
<style type= "text/css">

<!--

  选择器A1,选择器A2,... {样式属性1:属性值1;样式属性2:属性值2;...}

  选择器A1,选择器A2,... {样式属性1:属性值1;样式属性2:属性值2;...}

  ...

-->

</style>

***//<!---->注释符在此处是为了兼容不支持css的浏览器（在此处可有可无）

如当浏览器不支持css时，浏览器就会将其转化为注释，不进行编译。当浏览器支持css时，就会自动消去注释符。

**/其中CSS的注释符号为/*注释内容*/
```
（3）导入式(外部样式表):
```html
<style type="text/css>

<!--

@import url("外部样式文件URL");

-->

</style>
```
（4）链接式(外部样式表)：
```html
<link type="text/css" rel= stylesheet href="外部样式文件URL" />
```



# 七、CSS样式的基本语法：

基本格式： 选择器{规则}

```hl{color:blue;text-align:center}```

- 选择器(Selector) :是样式要套用的对象。例如: h1,在HTML文件中<h>...</h1>标记之间的内容将全部继承h1的全部规则

- 规则(Rule) :是样式设定的内容，用{}括起来




样式定义格式：

- 属性名:属性值
    - 同一个选择器定义多个样式

- 样式之间用 ; (分号)相隔
    - 某个样式的属性值不是一个单词 ，则值需要用引号括起来
```css
 p{font-family: "sans serif'}
```
- 一个属性有多个值，值之间用空格隔开
```css
 p{padding: 6px 4px 3px 5px}  //padding表示离它边框的距离，上右下左```
```
- 一个属性有多个候选值，值之间用逗号隔开
```css
p{font-family:"Times New Roman",Times,serif}  //如果有第一个字体，那就选第一个，如果没有就往后推，都没有就选一个系统默认字体```
```


font-属性类、text-属性类

（1）font-family:定义字体

- 取值例如 font-family:隶书

（2）font-size:定义字体大小

- 绝对大小: xx-small | x-small | small | medium | large | x-large | xx-large

- 相对大小: large | small

- font-size：x-large、 font-size:60px、 font-size: 160%、font-size: large

（3）font-weight :字体粗细

- normal | bold I bolder I lighter | 100-900

- font-weight:bold

（4）font-variant :字体变形

- nomal (普通) | small-caps (小型大写字母)

- font-variant:normal

（5）font-style:字体效果

- normal I italic I oblique

- font-style:italic

（6）text-align :定义文字水平显示位置

- left(centerlright

- text- align.center

（7）text-indent :定义首行缩进

- text-indent: 2em

（8）text-decoration :定义下划线

- text-decoration: none

（9）line-height :定义行间距

- line-height: 150%

（10）letter-spacing :定义字符间的水平间距

- letter-spacing: 0.3em


关于css的一些长度单位：

相对单位：

（1）em:相对字符尺寸，以当前元素本身的font-size为参考依据

- margin:4em

（2）px:以像素为单位

- font-size: 14px 

（3）ex:以字体中小写x字母为基准单位

绝对单位：

（1）in :英寸，1 in = 2.54cm

- margin:4in

（2）cm:厘米

- font-size:0.6cm

（3）mm:毫米

- font-size:6mm

（4）pt:点数，1 pt = 1/72 in

- font-size:40pt

（5）pc:印刷单位，1 pc = 12pt

- font-size:4pc

百分比单位：

- td {font-size: l2px;line -height:160%}  /*段落行间距为字体尺寸的160%*/

 

颜色属性：

（1）color :定义前景色

- color:red

（2）background-color :定义背景色（html属性定义背景色bgcolor）

- 颜色 | transparent (透明)

- background-color:white

其中颜色的取值:

(1)颜色命名: red, blue, white, black,

(2)RGB颜色
```css
td {color: rgb( 139,31,185);}

\#RRGGBB   #f7cd32   #fe3即: #ffcc33
```



# 八、CSS选择器：

 

css的基本选择器：标记选择器、类选择器、ID选择器、通配选择器(*) *为通配符

（1）标记选择器：选中网页中的h1元素
```html
css文件:

  h1{color:red}

html文件:

  <h1>HelloWorld</h1>
```
（2）类选择器：选中网页中class相同的元素
```html
css文件:

  .defineclass{color:red}

html文件:

  <h1 class="defineclass">HelloWorld</h1>
```
（3）ID选择器：选中网页中id相同的元素
```html
css文件:

  \#defineID1{color:red}

html文件:

  <h1 id="defineID1">HelloWorld</h1>
```
（4）通配选择器：选中网页中的所有元素
```html
css文件:

  *{color:red}

html文件:

  此时所有元素都显示为红色
```


CSS的复合选择器：(复合选择器=基本选择器+不同的分隔符)

（1）交集选择器（连续写2个选择器）
```html
标记.class名  //h1.defineclass{color:red}   两个条件都要满足才能选中

标记#ID名    //h1#defineID1{color:red}
```
（2）并集选择器（选择器分组）（逗号相隔）
```html
h1,h2,h3,p   //h1,defineclass{color:red}  只要满足一个条件即可选中
```
（3）后代选择器（空格相隔）
```html
css文件：

  h2 div{color:red}

html文件：

  <h2> <span>233  <div>HelloWorld</div>  </span> </h2> 

//含义：选中div元素，但是div元素一定要是h2的后代，即包含在内
```
（4）子代选择器（>）
```html
css文件：

  h2>div{color:red}

html文件：

  <h2><div>HelloWorld</div></h2>  
```
//子代:子代和它的父元素必须是紧紧挨着的嵌套结构，即仅次于它的后一代

**/(3)(4)子代和后代的区别：可以理解为爷爷，爸爸，儿子三个辈分。其中爸爸是爷爷的子代，而儿子是爷爷的后代。不能说儿子是爷爷的子代

 

# 九、CSS层叠性：

层叠性的含义：多个选择符作用于同一元素时，CSS怎样处理?

处理原则：

- 如果多个选择符定义的规则未发生冲突，则元素将应用所有选择符定义的样式

- 如果多个选择符定义的规则发生冲突，则CSS按选择符的优先级让元素应用优先级高的选择符  定义的样式。

    - CSS选择符优先级：内联样式>ID样式>类别样式>标记样式

- 优先级记忆方法：内联样式是写在style中的，离元素是最近的，在元素看来，它是为我定制的，所以优先级最高。ID类别标记为外联样式，离元素比较远，所以优先级低于内联。这其中,id可以理解为是有唯一性的，类通常是将相同特征的元素归为一类，类要比id更泛化一些，而我们能给相同的标记元素定义不同的类，故标记比类又更泛化一些。总上可得出ID>类>标记.

- 优先级相同的选择符：后定义的起作用（可理解为：后定义的覆盖了前面定义的）


 ```css
!important关键字：提升某个选择器的优先级为最高优先级
.green{color:green!important;}   //此时green的优先级为最高优先级
// 不建议使用，可能会导致代码阅读性下降
 ```

 

伪类选择器：以冒号开头

:active（鼠标在这个元素上按下）、:hover（鼠标移动到这个元素上）、 :link（这个元素是个链接）、 :visited（这个元素是个访问过的链接）

 

- love hate原则：


一定要按LVHA的顺序进行设计：即:link，:visited，:hover，:active顺序，否则会出现优先级覆盖问题。
```css
a:link{color:black}

a:visited{color:green}

a:hover{color:red}

a:active{color:olive}
```
原理：假设active在hover的前面，则会导致鼠标按下无效，原因：当鼠标按下时，active和hover选择器是同时选中了该元素(鼠标悬停在上面才能按下)，所以此时后定义的hover起作用，从而导致鼠标按下失效。

 

CSS继承性：

继承性含义：如果子元素定义的样式没有和父元素定义的样式发生冲突，那么子元素将继承父元素的样式风格，并可以在父元素样式的基础上再加以修改，自己定义新的样式，而子元素的样式风格不会影响父元素。

 

一般地

CSS文本属性:具有继承性

- color、font-类、 text-indent、text-align、 text-decoration、line-height、letter-spacing、border- collapse等


其他属性不具有继承性

- text-decoration:none、所有背景属性、所有盒子属性、布局属性




*CSS样式优先级总结：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image008.gif)


# 十、表格设计：


表格的开始和边框：```<table><tr><td>```
```html
<table [border="n"]>    //table是表格标记的开始，属性border：该表格的边框间距为n，如果不使用border属性，则边框为，即无表格边框
  <tr>   //tr:表示表格的一行
     <td>第一行第一列</td><td>第一行第二列</td> //td:表示表格的一列
  </tr>
  <tr>
     <td>第二行第一列</td><td>第二行第二列</td> 
  </tr>
</table>  //上述代码表示的是一个两行两列的表格 
**/注意：网页中的表格是按行定义的
```


表格的标题和表头：```<caption><th>```
```html
<caption>标题内容</caption>  //该标题标记插入在<table>标记中
<th>表头内容</th>   //插入在<tr>第一行中，用于代替第一行中的<td>，浏览器会用粗体代替该内容
```
效果如图：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image010.gif)

（1）跨列：colspan属性

```<td|th colspan="n">内容</td|tr>  //属性colspan表示与其他行对应垮了n格```

效果如图：

分析：此时第一行有2个<th>,第二行有3个<td>,代表第一行有两个单元格，第二行有三个单元格,此时colspan属性的n=2，代表电话改单元格对应第二行一共要跨2格。

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image012.gif)

（2）跨行：rowspan属性

```<td|th rowspan="n">内容</td|tr>  //与跨行类似```

效果如图：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image014.gif)

 

列表标记：

（1）无序列表：```<ul><li>```
```html
<ul [type="n"]> //type="disc(默认值:实心黑色原点，如下图)|circle(空心小圆点)|square(正方形)"
  <li>内容</li>表示列表的一行
</ul>  
```
效果：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image016.gif)

（2）有序列表：(与无序列表基本一致，只是将小圆点换成了有序的阿拉伯数字1.2.3.等)
```html
<ol type="a" start="m">  //type="A|a|I|i" 表示标记的样式可以是A.B.C.或者a.b.c.等 
  <li>内容</li><li>内容</li>  //start:起始编号(假设m=3，即从3.4.5.开始)
</ol>
```
（3）定义列表：(自定义列表的布局方式)
```html
<dl>
  <dt></dt>  //dt：定义项标记
  <dd></dd><dd></dd>  //dd:定义项的解释,效果类似于对dt进行换行Tab缩进
</dl>  
```
（4）列表嵌套：(将上述三种情况的列表互相嵌套在列表之中)

如下图所示：该嵌套会自动产生一种缩进的效果

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image018.gif)

 

# 十一、HTML表单：

**/表单功能提供了用户与Web服务器的信息交互功能(收集用户的信息带到服务器上)

表单接收用户信息，并把信息提交给服务器，然后由服务器端的应用程序处理信息，把处理结果返回给用户并向用户显示。（常见的如注册账号需要在浏览器中输入信息，此处就由表单对后台进行交互）


表单标记：

表单定义标记```<form>...</form>```

输入标记```<input>...</input> ```  -表单输入类型

列表框标记```<select>...</select>```

文本输入区标记```<textarea>...</textarea>```


（1）表单定义标记：（表单标记的最外层form标记）
```html
<form method="post|get" action="URL"></form>

//action: 指定完成表单信息处理任务服务器程序的完整URL(告诉服务器我的信息应该交给哪个程序处理)

//method: 指定表单中输入数据的传输方法，它的取值是get或post,默认值是get。(了解即可)
```
（2）input输入标记：

定义输入控件的一些属性：

1.   type:指定控件类型，默认属性值是text，可选属性值如下：

- button:按钮，可以创建提交按钮、复位按钮和普通按钮

- text:文本框，接受任何形式的输入，如字母、数字等

- password:口令框，用*(星号)代替输入字符的显示

- checkbox:复选框，提供多项选择 (通常是正方形按钮)

- radio:单选按钮，提供单项选择 (通常是圆圈按钮)

- submit:提交按钮，单击提交按钮,发送表单信息提交至服务器

- image:图像按钮，单击图像，发送表单信息提交至服务器

- reset:复位按钮，把表单上的组建值恢复为默认值

- hidden:隐藏表单组件，把表单中一个或多个组件隐藏起来

- textarea:多行文本框

- - file:上传文件

2. name:控件标识

3. value:控件初始值

4. maxlength:控件输入域中允许输入的最多字符数

5. size: 控件输入域的大小

6. checked: 提供复选框和单选按钮的初始状态

（3）供用户输入选择选项的控件<select>标记：（常见于网页中的下拉菜单）

*/其中每一个选项都是select元素的一个子元素,用option标记进行标记。
```html
<select name="下拉列表框名称" size="下拉列表框显示的条数">
    <option value="控件的初始值"selected>选项描述  //选项描述是用户可以看到的文字信息
    <option value="控件的初始值">选项描述
	...
</select>
```
（4）多行文本框```<textarea>```:

作用与文本框相同,常见于一些论坛的发表区，如贴吧的楼层
```html
<textarea cols="文本宽度" rows= "文本区行数">...</textarea>

textarea与input输入标记中type属性值中的textarea是等效的
```

表单实际操作举例代码：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image020.gif)

代码展示效果：

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image022.gif)



# 十二、关于html标记语言的其他知识点补充：

 

一、标记语言的文档类型：

- <!DOCTYPE>声明

(不是HTML标签;它是指示Web浏览器关于页面使用哪个HTML版本进行编写的指令）通常写在代码的第一行，html的前面

- HTML5文档类型之一 ```<!DOCTYPE html>```


二、网页内容包含特殊字符，html编码和字符代码

htm编码：为避免html语言解析产生误解的编码方案叫做html编码（含义大致和c语言中的转义字符相当，当想写入小于符号文字时，会被浏览器判定为标记代码的左尖括号，用html编码即可避免）

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image024.gif)


三、HTML头部元素：

即嵌套在```<head>```标记中的标记符号，前面有记录```<title><base>```标记，以下为补充：

```<meta>```标记：（告知浏览器的信息描述）

```html
<meta http-equiv="Content-Type" content ="text/html;charset=gb2312"/>

Content-Type：内容类型  content：字符集  //（用于指示网页采用的编码方案）

<meta name="description" content="HTML examples"> 

<meta name="keywords" content="HTML, DHTML,CSS,XML, XHTML, JavaScript, VBScript">

//4.5行表示告知网页的主要内容和主要关键字 （用于对网页的搜素引擎和排序）
```



# 十三、CSS布局：

 网页布局概述：

- 网页的基本单位:网页元素
    - 网页元素——盒子

- 布局:摆盒子


-  盒子模型：


![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image026.gif)


一、padding(内边距)：

（1）样式名分边：

- padding-top（上内边距）

- padding right （右）

- padding-bottom（下）

- padding-left（左）


（2）一个内边距的样式名可以带有多个样式值：

- padding: 2px (四边)

- padding: 5px 10px(上下、左右)

- padding: 5px l0px 4px(上、左右、下)

- padding: 5px 10px 4px 8px (上、右、下、左)


二、border(边框)：

（1）border-style：

- border- top-style、border-right-style、border-bottom-style、border-left-style

- border-width、border-color: 四边


例
```css
p.aside {border-style: solid dotted dashed double;}

//表示边框上、右、下、左采用不同的style(风格)

p {border-style: solid; border-width: 15px 5px 15px 5px;}

//上、右、下、左
```


三、margin(外边距)：

margin-top、margin-right、margin-bottom、 margin-left（与内边距一样）

- p {margin: 20px 30px 30px 20px;}

- 其中属性值还可以是auto(自动)，由网页自动调整外边距,通常为margin:0 auto;使元素进行居中显示

**/margin合并问题：

（当两个盒子外边距覆盖在其他盒子的外边距之中，浏览器就会将两个外边距进行合并）

![clipboard.png](HTML+CSS%E5%9F%BA%E7%A1%80.assets/clip_image028.gif)

 

如何摆盒子：


流布局(flow)：（默认布局方式）

display样式：

（1）行内元素在同一行中水平排列

- 行内元素：inline元素 

- span, a, img,...

（2）块级元素的盒子占据一整行

- 块级元素：block元素

- div, p, hl, h2,...

（3）元素不显示：display：none;

 ```css
h1{display:block;}//按块级元素显示
 ```
通过流布局display可以改变一些元素的定义，如行内元素和块级元素可以互相转化，从而实现布局摆盒。


浮动布局：

（1）float样式
```css
h1{float:left|right;}
```
- 浮动的框可以向左(left)或向右(right)移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止

- l 由于浮动框不在文档的普通流中(被浮动的边框不在流布局中存在)，所以文档的普通流中的块框表现得就像浮动框不存在一样（即，浮动样式在原来的流布局的位置将会被其他没有浮动的元素挤占）


定位布局：

position属性：

（1）相对定位(position: relative)

- 相对于元素在流布局中的位置进行再次定位

- 原本所占的空间仍保留

（2）绝对定位(position: absolute)

- 以距离它最近的且定义了position样式的父元素为基准进行定位

    - 解释：1.这个父元素是被设置了position样式的；2.这个父元素是离它最近的

- 脱离标准流(在标准流中不再给他流位置，与浮动类似)

位置属性：top（定位在上下维度距离）、left（定位的时候左边的位移）、z-index（第三维坐标，维度垂直于显示器平面，用于控制元素的图层，元素的重叠。取值越大，越在上层）





