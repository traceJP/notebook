# 一、Servlet技术概念

## （1）什么是Servlet

- Servlet是lavaEE规范之一。规范就是接口。

- Serviet就JavaWeb三大组件之一。三大组件分别是:**Servlet****程序、Filter过滤器、Listener监听器**。

- Servet是运行在服务器上的一个java小程序，它可以接收客户端发送过来的请求，并响应数据给客户端。


## （2）HelloWorldServlet

- 制作一个类实现Servlet接口：

```java
import javax.servlet.*;
import java.io.IOException;
public class HelloWorld implements Servlet {

  public void init(ServletConfig servletConfig) throws ServletException { }

  public ServletConfig getServletConfig() {
    return null;
  }

   // service方法是专门用来处理请求和响应的方法
  public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    System.out.println("HelloWorld!!!\n成功调用到HelloWorld.java中的service方法");
  }

  public String getServletInfo() {
    return null;
  }

  public void destroy() { }

}
```
- 在web.xml文件中添加配置：

```xml
<!-- servlet标签给Tomcat配置Servlet程序 -->
<servlet>
    
  <!--servlet-name标签：给servlet程序起一个别名（一般是类名）-->
  <servlet-name>HelloWorld</servlet-name>

  <!--servlet-class是Servlet程序的全类名-->
  <servlet-class>HelloServlet.HelloWorld</servlet-class>

</servlet>

<!--servlet-mapping标签给servlet程序配置访问地址，否则servlet-name会报错-->
<servlet-mapping>
    
  <!--servLet-name标签的作用是告诉服务器我当前配置的地址给哪个servLet程序使用-->
  <servlet-name>HelloWorld</servlet-name>

  <!--
      url-pattern标签配置访问地址
        / 斜杠在服务器解析时，表示地址为：http://ip:port/工程路径
        /hello 表示访问地址为：http://ip:port/工程路径/hello（该地址可以随意更换）
  -->
  <url-pattern>/hello</url-pattern>

</servlet-mapping>
```
- 访问：

```html
// 1 在idea中启动tomcat之后，地址工程路径后修改为/hello直接访问该servlet即可
// 2 通过html页面的from表单请求：(当提交按钮提交请求后开始执行Servlet)
<from action="http://ip:port/工程路径/hello" method="get | post"></from>
```

## （2.1）Servlet执行过程原理

![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image002.gif)

## （3）Servlet的生命周期

1.   执行servlet杓造器方法：在第一次访问时创建servlet程序会调用

2.   执行init初始化方法：在第一次访问时创建servlet程序会调用

3.   执行service方法：除第一次外，每次访问都会调用（刷新时）

4.   执行destroy销毁方法：web工程停止时调用

## （4）Service方法区分get请求和post请求

- 当html表单提交之后，Servlet可以根据表单中的method属性请求方式做出相应的操作。

- 在service方法中获取html表单中的method属性值即可实现：

```java
// 类型转换，因为只有该类（httpServletRequest为servletRequest的子类）有getMethod()方法
HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;

// 获取请求方式：返回字符串"GET" or "POST" （注意该字符串是大写的）
String method = httpServletRequest.getMethod();

// 根据字符串做出对应的操作：（代码书写建议：建议写doGet方法和doPost方法，然后在service方法中分别调用）
if("GET".equals(method)) {
  System.out.println("get请求");
} else if("POST".equals(method)) {
  System.out.println("post请求");
}
```

## （5）HttpServlet类

- 在实际的开发中，都是使用继承HttpServlet类的方式去实现Servlet程序。

- 流程：

    - 编写一个类去继承HttpServlet类

    - 根据需要重写HttpServlet类中的doGet方法或doPost方法

        - 在对应的请求时会调用对应方法

    - 到web.xml中的配置servlet程序的访问地址


# 二、Servlet接口与类的体系

## （1）Servlet接口与子类体系图

![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image004.gif)

 

## （2）ServletConfig类详解

- ServletConfig是servlet程序的配置信息类，即web.xml文件的一个对应的类

- 该类在servlet接口中的init方法中有一个对象，可以使用该对象进行操作。也可以通过getServletConfig方法获取对象（在其他方法中使用该对象）。

- 注意：每个Servlet程序对应自己独立的ServletConfig对象。

    - 且ServletConfig对象最初是来源GenericServlet类中的引用，当子类重写了init方法时，需要调用父类的构造方法构造ServletConfig对象。（super.init(config)）

- 三大作用：

    - 可以获取Servlet程序的别名servlet-name的值（xml配置中的servlet-name标签）

    - 获取初始化参数init-param

    - 获取 ServletContext对象


## （2.1）获取servlet-name值

```java
servletConfig.getServletName();  // 返回String
```
## （2.2）初始化参数init-param

- 在web.xml配置文件下的``<servlet>``标签中（即servlet程序配置别名的标签）可以定义多个<init-param>标签：

```xml
<init-param>
  // 采用name-value方式 -> 即键值对形式
  <param-name>qwq</param-name>
  <param-value>233</param-value>
</init-param>
```
- 然后在init方法中即可通过键获取值：

```java
servletConfig.getInitParameter(name值);  // 返回String
```

## （2.3）获取 ServletContext对象

```java
servletConfig.getServletContext();
```
### （2.3.1）ServletContext类

#### 1、ServletContext类概念

- ServletContext是一个接口，它表示Servlet上下文对象。

- 一个web工程，只有一个servletContext对象实例。

- 生命周期：在web工程部署启动的时候创建，在web工程停止的时候销毁。

- ServletContext对象是一个域对象。

    - 域对象：是可以像Map一样存取数据的对象，这里的域指的是存取数据的操作范围。

    - 操作范围：整个web工程，跟随context生命周期创建销毁。


|        | 存数据         | 取数据        | 删除数据          |
| ------ | -------------- | ------------- | ----------------- |
| Map    | put()          | get()         | remove()          |
| 域对象 | setAttrivute() | getAttibute() | removeAttribute() |

#### 2、Context类的四个作用

- 获取web.xml中配置的上下文参数context-param

- 获取当前的工程路径，格式为 /工程路径

- 获取工程部署后在服务器硬盘上的绝对路径

- 像Map一样存取数据


#### 2.1、获取web.xml中配置的上下文参数context-param

- web.xml文件中配置上下文参数：

```xml
<context-param>
  // 与init初始化参数一样是采用键值对形式
  // 不一样的是init中的参数只能在初始化调用，而context参数是整个工程都可以被调用
  <param-name>qwq</param-name>
  <param-value>233</param-value>
</context-param>
```
- 获取该键值对：
    - 注意区分context-param和init-param的区别：两者作用与不同的对象，不能互相获取xml文本。

```java
// 1 获取web.xml中配置的上下文参数ccontext-param的Context对象
ServletContext context = getServletConfig().getServletContext();

// 1.1 注意：ServletContext对象是在GenericServlet类中创建的，所以当继承它的子类时可以直接调用getContext方法
  ServletContext context = getServletContext();

// 2 获取键值对：此方法与init-param一样，只是使用的对象不同。
context.getInitParameter(name值);  // 返回String
```
#### 2.2、获取当前的工程路径，格式为 /工程路径

```java
// 1 获取Context对象context
// 2 调用方法
context.getContextPath();
```

#### 2.3、获取工程部署后在服务器硬盘上的绝对路径

- 什么是硬盘上的绝对路径：参考Tomcat笔记->idea部署原理，可知，idea下的工程路径和部署路径是不一样的。


- 获取方法：

```java
context.getRealPath("/");  // 返回String
// 参数"/"意义：/ 斜杠被服务器解析地址为http://ip:port/工程名/  即映射到DEA代码的web目录
```

#### 2.4、像Map一样存取数据

```java
// 存数据
context.setAttribute(name参数, object参数);  // key-value对

// 取数据
context.getAttribute(name参数);
```


# 三、http协议与请求响应

## （1）谷歌查看HTTP协议报文

![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image006.gif)

 

## （2）请求的HTTP协议格式

### 1、GET请求

- 请求行：

    - 请求的方式 -> GET
    - 请求的资源路径[+?+请求参数]  // 中括号为可选

    - 请求的协议以及版本号 -> HTTP/1.1
- 请求头：

    - key:value组成 -> 不同的键值对，表示不同的含义

- 报文分析：


![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image008.gif)

### 2、POST请求

- 请求行：与GET请求一致，请求方式从GET改为POST

- 请求头：与GET请求一致

- 请求体：发送给服务器的数据

- 注意：请求头和请求体在请求报文中隔了一个空行

- 报文分析：

![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image010.gif)

### 3、GET请求和POST请求体现

- GET请求有哪些：

    - form标签method=get

    - a标签

    - link标签引入css

    - Script标签引入js文件

    - img标签引入图片

    - iframe引入html页面

    - 在浏览器地址栏中输入地址后敲回车

- POST请求有哪些：

    - form标签method=post


## （3）请求的HTTP协议格式

- 响应行

- 响应的协议和版本号
- 响应状态码

    200->表示请求成功
    302->表示请求重定向
    404->表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）
    500->表示服务器已经恢到请求，但是服务器内部错误〈代码错误〉

- 响应状态描述符


- 响应头

    - key : value，不同的响应头，有其不同含义

- 响应体 ： 回传给客户端的数据

    - 响应头和响应体之间有一个空行

- 报文分析：


![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image012.gif)

# 四、HttpServlet请求响应类

## （1）HttpServletRequest类

- 作用：每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中。然后传递到service方法(doGet和doPost)中给我们使用。我们可以通过HttpservietRequest对象，获取到所有请求的信息。

- 生命周期：当有请求进入时创建对象，请求结束对象销毁。

- 对象位置：在继承HttpServlet类后重写doGet和doPost方法中会传入该对象

- 常用方法：

```java
// 获取请求的资源路径
getRequestURI()

// 获取请求的统一资源定位符（绝对路径）
getRequestURL()

// 获取客户端的ip地址
getRemoteHost()

// 获取请求头
getHeader()

// 获取请求的参数
getParameter(name参数)  // name参数为html表单中发送的请求参数

// 获取请求的参数（多个值的时候使用：html表单中多选会传给服务器多个值）
getParameterValues()

// 设置请求体的字符集，解决post请求的中文乱码问题
setCharacterEncoding(字符集参数)  // "UTF-8"

// 获取请求的方式GET或POST
getMethod()

// 设置域数据：两个servlet程序中的参数传递
setAttribute(key,value);

//获取域数据
getAttribute(key);

// 获取请求转发对象
getRequestDispatcher(其他servlet程序路径参数)  // 注意：请求转发的路径必须要以/开头（原理斜杠表映射地址）

// 跳转转发对象：先获取再跳转
requestDispatcher.forword(req,resp)  // req,resp即将当前do方法中的参数传给下一个servlet程序
```

## （1.1）请求转发

- 当客户端发送的请求需要两个或两个以上的Servlet程序才能完成该业务时，两个servlet有一定的执行顺序，当第一个servlet程序执行结束时，需要转发到第二个servlet程序。


![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image014.gif)

- 请求转发特点：

    - 浏览器地址栏没有变化

    - 他们是一次请求

    - 他们共享Request域中的数据

    - 可以转发到WEB-INF目录下（注意WEB-INF目录是保护目录，不可以通过浏览器请求）

    - 不可以访问工程以外的资源

- html标签``<base>``：

    - 页面中默认的地址参照是浏览器中的地址栏

```html
<base href="地址URL" />  // 设置页面下的地址参照
```

## （2）HttpServletResponse类

- 作用：HttpServletResponse类和HttpServletRequest类一样。每次请求进来，Tomcat服务器都会创建一个Response对象传递给Servlet程序去使用。HttpServletResponse表示所有响应的信息，我们如果需要设置返回给客户端的信息，都可以通过HttpServletResponse对象来进行设置。

- Response对象如何响应：（两种响应流不能同时使用，否则将报错）

    - 字节流

```java
getOutputStream()
```
- 字符流

```java
getWriter()

// 给客户端回传字符串数据：一般是给客户端回传htmlcssjs等标签以及数据，这样可以在网页上显示出来
PrintWriter writer = resp.getWriter();
writer.write(String s);
  
// 设置字符集解决中文乱码：注意浏览器和客户端所使用的字符集都需要设置
// 设置服务器字符集
resp.setCharacterEncoding("UTF-8);
    
// 通过响应头设置浏览器字符集
resp.setHeader(name:"Content-Type", value:"text/html; charset=UTF-8");
    
// 同时设置服务器和客户端字符集,还设置了响应头
resp.setContentType("text/html; charset=UTF-8");
```

## （2.1）请求重定向

- 注意请求转发和请求重定向的区别：请求转发是指服务器进行新地址的转移，而请求重定向是指服务器返回信息给客户端进行新地址的转移。


![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image016.gif)

- 请求重定向方法：

```java
// 设置响应状态码302，表示重定向
resp.setStatus(302);

// 设置响应头，说明新地址再哪
resp.setHeader(name:"Location", value:"新地址URL");

// 或者直接告诉浏览器重定向地址：
resp.sendRedirect("新地址URL");
```
- 请求重定向特点：

    - 浏览器地址栏会发生变化

    - 两次请求

    - 不共享Request域中数裾

    - 不能访问WEB-INF下的资源（因为是浏览器进行的请求）

    - 可以访问工程外的资源


## （3）Session会话（HttpSession接口）

### 1、概念

- session就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。

- 每个客户端都有自己的一个session会话。

    - 注意：每个会话都有一个独立的身份证号。也就是ID值。而且这个ID是唯一的。

- Session会话中，我们经常用来保存用户登录之后的信息。


### 2、创建和获取Session

```java
req.getSession()  // 返回HttpSession对象

// 第一次调用是:创建session会话
// 之后调用都是:获取前面创建好的session会话对象。
boolean isNew()  // 判断到底是不是刚创建出来的（新的）
// true表示刚创建
// false表示获取之前创建

String getId()  // 得到唯一标识ID值  
```

### 3、Session域

- 四大域之一。用于保存数据和获取数据。

```java
// 保存数据
Session对象.setAttribute(s:"key值", o:"value值");

// 获取数据
Session对象.getAttribute(s:"key值");
```

### 4、Session生命周期控制（与Cookie一样可以控制生命周期）

- session超时：

    - session的超时指的是，客户端两次请求的最大间隔时长。即第二次请求会重置掉第一次的timeout计时器。

```java
// 设置session的超时时间，超过指定的时长，5ession就会被销毁。
public void setMaxInactiveInterval(int interval);

  // interval为正数时：设定时长； 负数：永不超时
  // 以秒为单位：默认为1800秒（该默认时长是在tomcat下web.xml中session-timeout标签配置）
  // 可以直接通过web.xml添加配置更改：
    // <session-config>
    //   <session-timeout>默认时长（以分钟为单位）</session-timeout>
    // </session-config>

// 获取Session的超时时间
public void getMaxInactivelnterval();

// 使当前Session会话马上超时（删除Session）
public void invalidate();
```

### 5、Session与Cookie底层联系

- Session技术，其底层原理是基于Cookie技术来实现的。


![clipboard.png](Servlet%E7%A8%8B%E5%BA%8F.assets/clip_image018.gif)

 

 

 