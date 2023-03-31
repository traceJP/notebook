# 一、xml定义

（1）什么是xml：xml是可扩展的标记性语言

（2）xml的主要作用：

- 用来保存数据，而且这些数据具有自我描述性
- 它还可以做为项目或者模块的配置文件
- 还可以做为网络传输据的格式（现在JSON为主）。

# 二、xml语法

（1）xml文件声明：
```xml
<!-- version表示xml文件的版本,encoding表示xml文件本身的编码 -->
<?xml version="1.0" encoding="utf-8" ?>
```

（2）xml元素（标签）：

- xml元素是指开始标签到结束标签的部分（与html标签一样）

- xml标签命名规则：

    - 名称可以包含字母数字以及其他字符（包括中文）

    - 名称不能以“xml（Xml、XML等）”字符串开始，且不能包含空格

- xml标签，属性，特殊字符，单双标签等都与html类似。

- xml必须要有根标签：

    - 根元素就是顶级元素，没有父标签的元素，叫顶级元素。根元素是没有父标签的顶级元素，而且是只能有一个才行。


（3）文本区域（CDATA区）：

- CDATA格式：

```xml
<!-- CDATA语法可以告诉xml解析器,找CDATA里的文本内容,只是纯文本,不需要xml语法解析 -->
<![CDATA [这里可以把你输入的字符原样显示，不会解析xml] ]>
```


# 三、xml解析技术

- dom解析和sax解析：

    - 早期JDK为我们提供了两种xml解析技术DOM和sax简介（已经过时，但我们需要知道这两种技术）

        - dom解析技术是W3C组织制定的，而所有的编程语言都对这个解析技术使用了自己语言的特点进行实现。Java对dom技术解析标记也做了实现。

    - sun公司在JDK5版本对dom解析技术进行升级: SAX(Simple API for XML)

        - SAX解析，它跟W3C制定的解析不太一样。它是以类似事件机制通过回调告诉用户当前正在解析的内容。它是一行一行的读取xml文件进行解析的，不会创建大量的dom对象。所以它在解析xml的时候，在内存的使用上。和性能上。都优于Dom解析。

- 第三方的解析：jdom在dom基础上进行了封装、dom4j又对jdom进行了封装。


## 1、dom4j解析技术

（1）前提：需要dom4j.jar的jar包依赖，可在https://github.com/dom4j/dom4j中下载。
```java
// 通过4个步骤，即可获得xml文件中的各个标签或属性或元素
// 1 读取books.xmL文件
// 2 通过Document对象荻取根元紊
// 3 通过根元紊获取book标签对象
// 4 遍历，处理每个book标签转换为Book类（或者遍历获取每个标签中的子标签）
```
（1.1）假设现有一个LearnXml.xml文件：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<books>
  <book sn="SN233">
    <name>算法</name>
    <id>1</id>
    <jiage>50</jiage>
  </book>
  <book sn="666">
    <name>算法</name>
    <id>1</id>
    <jiage>50</jiage>
  </book>
</books>
```
（2）xml文件通过Reader流创建文档对象：
```java
// 创建读取xml文件的流
SAXReader saxReader = new SAXReader();

// 创建文档对象：读取哪个xml文件：这里指LearnXml
Document document = saxReader.read("LearnXml.xml");
```
（3）文档对象的一些API：
```java
// 根据Document对象得到根元素（根标签）（Element对象）
Element rootElement = document.getRootElement();
```
（4）通过根对象即可开始遍历（获取）整个xml文件的标签及内容：这里以LearnXml.xml为例
```java
// .element(String s) 方法可以返回一个该标签下的指定标签名（s)，如books的element() -> 返回book的Element对象
Element element = rootElement.element();  // 返回单个
List<> elements = rootElement.elements();  // 因为标签底下可能有多个相同的标签，所以要返回集合
```
- 其他基本API

```java
// 1 asXML:以字符串的形式打印该标签内的所有内容(包括标签属性等)
System.out.println(book.asXML());

// 2 获取element对象的属性值：attribute(属性),Value(值),s:属性名
String snText = book.attributeValue("sn");

// 3 可以使用getText获得Element对象中的文本：指两个标记之间的内容
String nameText1 = nameElement.getText();

// 4 也可以直接用Element对象中的elementText方法直接输出（指定String s）文本内容
String nameText2 = book.elementText("name");
```





