# 一、JSP概念

## （1）JSP本质

- jsp页面本质上是一个Servlet程序。
- 当我们第一次访问jsp页面的时候。Tomcat服务器会帮我们把jsp页面翻译成为一个java源文件。并且对它进行编译成为.class字节码程序。我们打开java源文件发现其实里面的HttpJspBase类是继承了HttpServlet类。也就是说，jsp翻译出来的java类间接的继承了HttpServlet类。
- jsp翻译源码查看位置：
    - Tomcat服务器部署工程目录下/work/Catelina/localhost/模块名/org/apaache/jsp内

![clipboard.png](JSP%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1.assets/clip_image002.gif)

## （2）JSP技术出现缘由

- 使用Servlet程序技术进行请求响应增加了开发的成本。尤其是在servlet响应客户端中回传数据使用writer流进行字符串传输，导致开发不便利。

## （3）HelloWorld

- jsp页面与html页面的区别就在第一行
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head><title>Hello</title></head>
  <body>HelloWorld!!!</body>
</html>
```


# 二、JSP指令操作

## （1）jsp头部的page指令

- jsp的page指令可以修改jsp页面中一些重要的属性，或者行为。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

- 常用属性：
    - language属性：表示jsp翻译后是什么语言文件。暂时只支持java
    - contentType属性：表示jsp返回的数据类型是什么。也是源码中response.setContentType()参数值
    - pageEncoding属性：表示当前jsp页面文件本身的字符集。
    - import属性：跟java源代码中一样。用于导包，导类。
    - autoFlush 属性：设置当out输出流缓冲区满了之后,是否自动刷新冲级区。默认值是true
    - buffer属性：设置out缓冲区的大小。默认是8kb
    - errorPage属性：设置当jsp页面运行时出错，自动跳转去的错误页面路径。
        - errorPage表示错误后自动跳转去的路径
        - 这个路径一般都是以斜杠打头，它表示请求地址为http://ip:port/工程路径/
        - 映射到代码的web目录。注意这个/在servlet中仅解析到http://ip:port/中
    
    - isErrorPage属性：设置当前jsp页面是否是错误信息页面。默认是false。如果是true可以获取异常信息。

    - session属性：设置访问当前jsp页面，是否会创建HttpSession对象。默认是true

    - extends属性：设置jsp翻译出来的java类默认继承谁。
    

## （2）jsp脚本

- 声明java代码脚本：

    - <%! %>：servlet程序中_jspService()方法中翻译为的内部语句放置在最前面
    - <% %>：servlet程序按顺序与html等标签一起翻译，java变为内部语句，标签用writer输出。这也是<%%>标签和html标签可以混合使用的原理。

![clipboard.png](JSP%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1.assets/clip_image004.gif)
```jsp
<%!
  int a = 233;  // 带感叹号的脚本java语言只能作为声明语句使用
%>
<%
  a = 233;  // 可以使用任意的java语言代码
%>
```
- 表达式脚本：

    - servlet程序中_jspService()方法中翻译为该类的内部语句out.print()方法的输出
    - 注意：该输出语句不能加分号，因为是out.print()括号内的内容。且_jspService()方法中的对象可以直接使用。

```jsp
<%=输出语句%>  <!-- 可以在jsp页面上输出内容 -->
```
## （3）jsp中的三种注释

- HTML注释：

```jsp
<!-- -->  在_jspService()中以writer()输出到客户端中
```
- java注释：

```jsp
// /* */  在_jspService()中原样翻译，不会被传到客户端中
```
- jsp注释：

```jsp
<%-- --%>  jsp注释可以注掉jsp页面中的所有代码
```

## （4）jsp九大内置对象

- jsp中的内置对象，是指Tomcat在翻译jsp页面成为Servlet源代码后，内部提供的九大对象，叫内置对象。


![clipboard.png](JSP%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1.assets/clip_image006.gif)

- request -> 请求对象

- response -> 响应对象

- pageContext -> jsp上下文对象

- session -> 会话对象

- application -> servletContext对象

- config -> ServletConfig对象

- out -> jsp输出流对象

- page -> 指向当前jsp的对象

- exception -> 异常对象

### （4.1）out输出和response.getWriter输出区别

```jsp
out.write();
response.getWriter().write();
```
- 两者输出的原理图：


![clipboard.png](JSP%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1.assets/clip_image008.gif)

- 其中out对象的write()输出和print()输出：

    - 由于jsp翻译之后，底层源代码都是使用out来进行输出，所以一般情况下。我们在jpp页面中统一使用out来进行输出。避免打乱页面输出内容的顺序。

```jsp
out.write();  // 输出字符串没有问题

out.print();  // 输出任意数据都没有问题（都转换成为字符串后调用的write输出）
```

## （5）jsp四大域对象

- 域对象是可以像Map一样存取数据的对象。四个域对象功能一样。不同的是它们对数据的存取范围。

    - 即可以使用servlet中的域对象方法，如setAttribute(key, value)等

    - pageContext(PageContextlmpl类)  在当前jsp页面范围内有效

    - request(HttpServletRequest类)  一次请求内有效

    - session(HttpSession类)  一个会话范围内有效（打开浏览器访问服务器，直到关闭服务器）

    - application(ServletContext类)  整个web工程范围内有效（只要web工程不停止，数据都在）

- 四个域在使用的时候，优先顺序分别是他们从小到大的范围的顺序：可以使资源可以被快速释放

    - pageContext ==>>> request ==>>> session ==>>> application


## （6）静态包含之include指令

- 当许多页面中的有相同部分的代码需要同时维护，可以使用include指令将其他页面包含进来，页面将解析url内所有的jsp代码。

    - 原理：使用静态包含会将包含的页面代码原封不动拷贝到被包含页面的相应位置进行执行输出。

```jsp
<!-- file属性指定你要包含的jsp页面路径：/斜杠映射web目录 -->
<%@ include file="页面路径" %>
```
### （6.1）动态包含之动作指令jsp:include

- 区别：在效果上无任何区别。

    - 底层原理：动态包含会把包含的jsp页面也翻译成java代码。然后再在底层使用JspRuntimeLibrary.include(...)方法调用被包含的jsp页面执行输出。

```jsp
<jsp:include page="页面路径" ></jsp:include>
```
- 使用动态包含传递参数：

```jsp
<jsp:include page="页面路径" >
  <jsp:param name="" value />  // 键值对传递
  ...
</jsp:include>
```
- 获取：

```jsp
request.getParameter(name参数值);  // 返回String
```

### （6.2）请求转发之forward

- 功能：请求转发

```jsp
<jsp:forward page="" />
```



