# 一、EL表达式

## （1）概念

- EL表达式的全称是：Expression Language，是表达式语言。
- EL表达式的什么作用：EL表达式主要是代替jsp页面中的表达式脚本在jsp页面中进行数据的输出。
- 因为EL表达式在输出数据的时候，要比jsp的表达式脚本要简洁很多。
- jsp表达式和EL表达式对比：
```jsp
<!-- Jsp表达式输出 -->
<% request.setAttribute("key", "HelloWorld"); %>
<%=request.getAttribute("key")%>

<!-- EL表达式输出：注意$符与{之间不能有空格，且EL表达式不会输出null值 -->
${key}    // 格式：${表达式}
```
## （2）EL表达式搜索域数据

- EL表达式主要是在jsp页面中输出域对象中的数据。
- 四大域：request、seesion、application、pageContext。
- 当四大域中都具有相同的key数据的时候，EL表达式会按照四个域中从小到大的顺序输出。
    - pageContext > request > session > application
- 也可以直接使用EL表达式中的隐含对象指明用哪个域中的值：

```jsp
${requestScope.key值}
${sessionScope.key值}
${applicationScope.key值}
${pageScope.key值}
```
- 输出复杂类中的单个属性值：
    - 原理：jsp会寻找类中的get方法进行调用输出

```jsp
${key值.属性值}    // 需要javabean中的getset等方法
${key值.属性值[i]}    // 数组或集合
${key值.属性值.map集合中的key值}    // map
```

## （3）EL表达式运算

- 
    empty运算：判断某个对象是否为空，为空返回true，不为空返回false
```jsp
${empty key值}
```
- EL表达式支持三元运算符、点运算、[]运算、()运算：即EL表达式进行复杂类中的单个属性值输出使用的运算。

# 二、JSTL标签库

## （1）概念

- JSTL标签库全称是指JSP Standard Tag Library JSP标准标签库。是一个不断完善的开放源代码的jsp签库。

- EL表达式主要是为了替换jsp中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个jsp页面变得更佳简活。

- 需要引入taglibs-standard-impl-1.2.5.jar和taglibs-standard-spec-1.2.5.jar包依赖：注意，jar包需要放在web文件下。

    - http://tomcat.apache.org/taglibs/standard/中下载。

    - JSTL由五个不同功能的标签库组成：
```
核心标签库    // uri="http://java.sun.com/jsp/jstl/core" prefix="c"
格式化    // uri="http://java.sum.con/jsp/jstl/fmt" prefix="fmt"
函数    // uri="http://java.sum.con/jsp/jstl/functions" prefix="fn"
数据库（弃用）    // uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"
XML（弃用）    // uri="http://java.sun.com/jsp/jstl/xml" prefix="x"
```
- 在jsp标签库中使用taglib指令引入标签库：


```jsp
<@ taglib prefix="" uri="" %>
```
## （2）core核心库使用

- c:set

```jsp
<c:set scope="" var="" value="" />    // 用于向域中保存数据
scope：属性设置保存到哪个域：
    page表示PageContext域（默认值）
    request表示Request域·
    session表示Session域
    application表示ServLetContext域
var：key值
value：value值
```
- c:if    （该标记无if-else写法）

```jsp
<c:if test="" />
    如果test=true,则输出标签中间的内容
</c:if>
test：布尔值,可以是EL表达式返回的布尔值
```
- c:choose、c:when、c:otherwise
- 类似java中的switch、case、default。注意：在标签中不能使用html注释，when标签一定要作choose标签的直接标签。即可以在otherwise标签中继续嵌套choose标签，但不能不加choose标签而直接when标签处理。

```jsp
<c:choose>
    <c:when test="boolean">    // 注意：可以有多个when标签
        如果boolean=true,那么就输出when标签中的内容，然后自动break跳出choose标签。
    </c:when>
    ...
    <c:otherwise>
        如果when标签都不成立（剩下的情况），那么就输出otherwise标签中的内容，然后自动break跳出choose标签。
    </c:otherwise>
</c:choose>
```
- c:foreach（遍历循环）
- 当作for使用时：

```jsp
    <c:forEach begin="" end="" var="" step="">
        循环内容
    </c:forEach>
    begin：开始值（值>=0）
    end：结束值（值>=0）
    var：循环变量（每次将递增后的值赋值给该变量，相当于for中的i）
    step：步长值，即for中的i++，默认为1。
```
- 当作foreach使用时：

```jsp
    <c:forEach items="" var="">
        循环内容
    </c:forEach>
    items：遍历的数据源（集合）
    var：将遍历的数据赋值给该变量
```
- for和foreach结合使用：

```jsp
    <c:forEach begin="" end="" items="" var="">
        循环内容
    </c:forEach>
    此时：迭代会从items[begin]开始到items[end]结束
```
- varStatus属性：varStatus=""
    - 会得到一个var值的对象，并将该对象赋值给varStatus变量。该对象的类实现了LoopTagStatus接口（taglibs-standard-spec-1.2.5.jar），通过该对象可以调用其get方法获取遍历的数据，索引，遍历个数，是否第一个最后一个等...





