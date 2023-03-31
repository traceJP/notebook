# 一、SpringMVC概念

## （1）概述

- Spring为展示层提供基于MVC设计理念的web框架。是封装servlet的一种框架。

- SpringMVC通过一套MVC注解，让POJO成为处理请求的控制器，而无需实现任何接口。

- 支持REST（restful）风格的URL请求。

- 采用松散耦合可插拔组件结构。

- 是Spring框架的后续产品。属于Spring框架的一部分。

- SpringMVC最大的好处就是使得接收请求的类无需继承任何类（如HttpServlet）。


## （2）HelloWorld

1、在web.xml中配置springMVC和核心（前端）控制器DispatcherServlet

- 作用：加载SpringMVC的配置文件。
    - web.xml：

```xml
<servlet>
  <servlet-name>springmvc</servlet-name>

  // 类路径为SpringMVC官方写好的核心servlet
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

  // (可选)指定初始化springmvc-xml配置文件的路径
  <init-param>

      // 属性：上下文配置路径
     <param-name>contextConfigLocation</param-name>

     // 配置路径：可以使用classpath:作为前缀然后使用包类路径
     // 一般maven目录是指定映射在resources目录下
     <param-value>...</param-value>

  </init-param>

  // (可选)设置servlet的加载时间
     // 其servlet默认是在第一次接收到请求时加载
     // 若设置此标签，则加载时间提前到项目启动时
     // value:(整数)/负数和0为无任何效果，正整数即值越小优先级越高
  <load-on-startup>1</load-on-startup>

</servlet>

<servlet-mapping>
  <servlet-name>springmvc</servlet-name>

   // 设置web.xml-Servlet的请求拦截路径
  <url-pattern>/hello</url-pattern>
    
</servlet-mapping>
```
2、编写springmvc-xml配置文件：

- 注意：如果未指定初始化参数配置文件的位置，则该xml配置文件默认需要在WEB-INF目录下， 且文件默认名为``<servlet-name>-servlet.xml``。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  // 开启包注解扫描（也可以直接使用<bean>标签注册bean）
  <context:component-scan base-package="com.TraceJP.SpringMVC"/>

   // 配置视图解析器
  <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">

     // 设置视图的头
     <property name="prefix" value="/WEB-INF/views/"/>

     // 设置视图的尾
     <property name="suffix" value=".jsp"/>

  </bean>

</beans>
```
3、编写映射处理类：

```java
@Controller  // 控制注解：指明这是一个视图层的bean
public class HelloSpringServlet {

  // 设置映射方法，value值指定映射的路径
  @RequestMapping("/hello")
  public String helloWorld() {
      
     System.out.println("SpringMVC已接收到请求！！！");
    
     // 返回视图解析器：视图解析器将根据此字符串拼接页面
     // 视图解析器头 + return值 + 视图解析器尾
     // 如此例子中页面将跳转到/WEB-INF/views/rep.jsp页面
     return "rep";
  }
}
```

# 二、SpringMVC

## 1、概念

- @RequestMapping可以用于设置请求映射，把请求和控制层中的方法设置映射关系。

- 当请求路径和@RequestMapping的value属性值一致时，则该注解所标注的方法即为处理请求的方法。


## 2、属性

### （1）String name

- 给这个映射分配一个名称。


### （2）String[] value和path

- 都是用于设置请求的映射URL。

- value属性本质为value属性的别名，使用两者都可以设置请求路径，当注解只有一个属性的时候也可以忽略属性而直接使用双引号设置请求路径即可。

- 都支持Ant风格的请求路径（Ant风格匹配符）

    - *：表示路径中可以有任意字符，类似mysql中like查询的%通配符

    - **：表示路径中可以有任意多层目录

    - ?：表示路径中可以有一个任意字符，类似mysql中like查询的下划线通配符


```java
value = "/hel*lo"  // 可以使用/helabclo
value = "/hello/**/a"  // 可以使用/hello/a/b/.../a
value = "/hel?lo"  // 可以使用/helalo 
```
- 都支持占位符方式的请求路径：

    - 通过占位符方式的请求路径，可以不必使用请求路径?key=value的形式进行数据的发送。可以直接通过路径绑定参数即可。

    - 注意：使用了占位符时，传入的请求路径必须要包含这些路径。

```java
// 设置占位符，且该占位符名为id
@RequestMapping("/hello/{id}")

// 使用@PathVariable注解设置接收占位符的形参
// 当不使用value属性时，形参名必须要与占位符名一致。
// 可以使用required属性使没有value的注解进行抛出异常的检查，参考spring源码
public String hello(@PathVariable(value = "<占位符名>") String id) {
  System.out.println(id);
  return "rep";
}

// 此时可以通过.../hello/admin进行前端参数的传递，admin即为id的值
```
- 都支持嵌入式占位符：

    - 嵌入式占位符是针对本地的配置文件进行解析。如Spring中通过此占位符解析.properties文件。

    - 注意占位符和嵌入式占位符的区别：占位符是指接收请求时，参数的占位符，只有大括号；嵌入式占位符指启动时通过PropertyPlaceHolderConfigurer对文件内容的解析，有$符号。

```java
@RequestMapping("/hello/${key}")
...
```


### （3）RequestMethod[] method

- 用来设置请求方式约束，只有客户端发送请求的方式和method值一致时，才能处理请求。

- 主要有RequestMethod枚举类中的GET，POST，HEAD，OPTIONS，PUT，PATCH，DELETE，TRACE值。

```java
// 此注解所对应的方法只能处理/hello请求路径的get请求
@RequestMapping(value = "/hello", method = RequestMethod.GET)
```
- 也可以直接使用请求方式注解代替@RequestMapping：

    - 源码分析：在源码中，该注解的类上@RequestMapping(method = RequestMethod.GET)注解。即原理还是使用了method属性。

```java
@GetMapping
...
```
### （4）String[] params

- 用于设置客户端传到服务器的数据约束，从而缩小请求映射的范围，支持表达式。

```java
@RequestMapping(value = "/hello", params = "<表达式>")
// 假设此时表达式为 "username"：表示传入的请求必须要有username参数
// "!username"：表示传入的请求不能有username参数
// "username=2"：表示传入的请求必须要有username参数且值必须为2
// ...
```
### （5）String[] headers

- 用于设置客户端传到服务器的请求头约束，只有当客户端发送的请求的请求头与headers值一致时才能处理该请求。

    - consumes()

    - produces()


## 3、作用位置

### （1）作用在方法上

- 当该注解作用在方法上时，指定该方法先对注解所设置的属性进行一遍请求的排除，最终选择其中能正确映射的方法执行。


### （2）作用在类上

- 当该注解作用在类上时，会按类上的注解属性进行一遍请求的排除，排除完毕后，再进入其方法，对其方法上的注解属性进行请求的排除，最终选择能正确的方法执行。


### （3）拦截路径：请求 -> web.xml拦截配置 -> 类 -> 方法 - > 接收请求

- web.xml：

```xml
<servlet-mapping>
  <servlet-name>springmvc</servlet-name>
    
   // 1 核心控制器拦截请求：/表示拦截所有请求 
  <url-pattern>/</url-pattern>
</servlet-mapping>
```
- 请求处理类：

```java
@Controller
@RequestMapping("/hello")  // 2 作用在类上，判断是否是/hello请求
public class HelloSpringServlet {

  // 3 作用在方法上，判断是否是get请求
  @RequestMapping(method = RequestMethod.GET)
  public String helloWorld() {
     System.out.println("GET方法执行");
     return "rep";
  }

  // 4 作用在方法上，判断是否是post请求
  @RequestMapping(method = RequestMethod.POST)
  public String helloWorld1() {
     System.out.println("POST方法执行");
     return "rep";
  }
}
```


## 4、获取客户端传入的数据

### （1）通过占位符和@PathVariable注解获取数据（上述笔记）

### （1.1）拓展：矩阵变量

- 注意：使用矩阵变量前需要在springmvc-xml文件中开启：

```xml
<mvc:annotation-driven enable-matrix-variables="true"/>
```
- 矩阵变量可以出现在任意占位符的后方，是占位符附带的key-value对。


- 请求：GET /hello/admin;password=123，其中admin为name占位符所取值；password即为矩阵变量，使用分号进行分割。

- 处理类：

```java
@GetMapping("/hello/{name}")
public String helloWorld2(

       // 使用@PathVariable注解将占位符映射到形参name上
       @PathVariable() String name,
    
       // 使用@MatrixVariable注解将矩阵变量映射到password形参上
       @MatrixVariable String password
    
       ...
       ) {

  System.out.println(name+password);
  return "rep";
}
```
- 矩阵变量的请求格式：
    - 可以使用多个矩阵变量，使用分号隔开。

```http
/hello/admin;key1=value1;key2=value2;...
```
- 可以使用一个矩阵变量对应多个值，每个值用逗号隔开。并使用数组或list形参接收。

```http
/hello/admin;key1=value1,value2,..valueN;key2=...
```
- 矩阵变量的@MatrixVariable映射属性：

    - value和name：映射请求传入时的参数。

    - required和defaultValue：是否是必要参数和参数的默认值。详细参考底下@RequestParam注解。

    - pathVar属性：区分该矩阵变量属于哪个占位符的。


- 请求：GET /owners/42;q=11/pets/21;q=22
- 处理类：

```java
@GetMapping("/owners/{ownerId}/pets/{petId}")
public void findPet(

     // 此时两个矩阵变量名都为q，就无法区分是哪个占位符附带的矩阵变量
     // 那么需要通过pathVar指定是哪个矩阵变量
     @MatrixVariable(name="q", pathVar="ownerId") int q1,
     @MatrixVariable(name="q", pathVar="petId") int q2) {
  // q1 == 11
  // q2 == 22
}
```

### （2）通过直接书写形参获取数据

- 在SpringMVC中，可以在映射处理请求的方法中直接通过形参获取


- 请求：/hello?username=xxx&password=xxx
- 请求接收方法：

```java
@RequestMapping(value = "/hello")
// 直接写入形参，并且形参名等于发送过来的参数名，SpringMVC将自动完成映射
// 若形参中没用对应的映射，则为null
public String hello(String username, String password) {
  ...
  return "rep";
}
```
- 使用POJO获取客户端数据：

- 请求：/hello?name=xxx&user.city=xxx

```java
// 在SpringMVC中，可以直接使用映射形参的POJO进行获取数据
@RequestMapping(value = "/hello")
// 方法中形参为一个JavaBean
// 其中客户端传入的数据会按照该JavaBean的属性进行对应的赋值，并且支持级联赋值
public String hello(UserJavaBean user) {
  ...
  return "rep";
}
```
### （3）通过@RequestParam注解映射请求数据

- 此注解可以用于形参中，用于将客户端传入的参数和形参进行匹配

- 请求：/hello?user=xxx

```java
@RequestMapping(value = "/hello")
public String helloWorld3(@RequestParam(value = "user") String username) {
  ...
  return "rep";
}
```
- @RequestParam属性：

    - String value和name：

        - 客户端传入的参数名，以映射相对应的形参。value属性是name属性的别名。

    - n boolean required：

        - 是否是必要参数，默认为true。

        - true：这是一个必要参数，如果客户端没有传入此参数则会抛出异常

        - false：这不是一个必要参数，如果客户端没有传入此参数，则映射会将形参设置为null

    - String defaultValue：

        - 当没有提供相应的请求参数或值为空时，则使用该默认值赋值给形参。提供默认值会隐式将required属性设置为false。


### （4）获取请求头信息

- 使用@RequestHeader注解可以获取HTTP请求的请求头信息。其使用用法与@RequestParam注解一致，name或value属性值为请求头的参数名。


### （5）获取客户端Cookie信息

- 使用@CookieValue注解可以获取客户端的Cookie信息。其使用用法与@RequestParam注解一致，name或value属性值为请求头的参数名。


### （6）直接获取servletAPI对象
```java
@RequestMapping(value = "/hello")
// 只需要直接要用的对象放到形参位置即可使用，如直接使用request对象
public String hello(HttpServletRequest req) {
  ...
  return "rep";
}
```


## 5、向作用域中放值

### （1）使用ModlAndView类向request作用域中放值并设置视图返回

- 对象使用：

```java
@RequestMapping(value = "/hello")
// 需要将整个ModAndView对象返回给视图解析器。
public ModelAndView hello() {

  // 创建一个ModelAndView对象
  ModelAndView mav = new ModelAndView();

  // 使用该对象方法进行放值
  mav.addObject("key","value");

  ...

  // 使用该对象的方法设置视图（可选）
  mav.setViewName("视图名");

  // 返回对象到解析器
  return mav;
}
```
- 深入理解ModelAndView：

    - ModelAndView类本质上是在自身中创建了一个ModelMap类和定义了一个view的Object对象。而在返回此对象的时候，本质上是返回了ModelMap对象和viewName。这样SpringMVC会通过解析这个对象的viewName，从而调用视图解析器进行解析。

    - 而其中ModelMap类其本质是一个SpringMVC的隐含对象。当只使用了ModelMap对象添加key-value时，他会默认的调用request对象的setAttribute将数据存入request域中。所以本质上ModelMap对象是不需要返回的。需要返回的只是视图的名称，以供视图解析器使用。


### （2）在形参中声明ModelMap对象或者ModelAndView对象

- 原理：SpringMVC也会在形参中建立这两个对象的隐含模型，最终还是被封装成ModelAndView对象，然后其过程与直接第一种方法一致。

```java
@RequestMapping(value = "/hello")
// 直接使用mav对象并返回该对象
public ModelAndView hello(ModelAndView mav) {
  ...
  return mav;
}

@RequestMapping(value = "/hello")
// 直接使用ModelMap对象
public String hello(ModelMap map) {  // 也可使用Model接口作为形参
  ...
  return "success";
}
```
### （3）在形参中直接获取Map对象向request作用域中放值

- SpringMVC对Java原生的Map进行了一个调用，最终该Map对象还是被封装到ModelMap中，并被自动保存在request域中。

```java
@RequestMapping(value = "/hello")
public String hello(Map<Object, Object> map) {
  map.put(.., ..);
  ...
  return "success";
}
```


## 6、响应主体

### （1）概念

- 在上述中，请求处理的方法返回的基本为视图名称（即页面跳转功能），所以，可以在方法上面使用@ResponseBody注解以通过HttpMessageConverter将返回值序列化为响应主体。

- 即直接响应一段数据到客户端。类似原生servlet中的HttpServletResponse类功能。


### （2）示例

```java
@RequestMapping(value = "/hello")
@ResponseBody
public String hello() {
  ...
  return "<响应字符串>";
}
```

### （3）响应返回Json数据

- 在原生servlet中一般使用第三方的jar包对json数据进行封装，主流的框架有Gson、fastJson、JsonLib、jackson。而在SpringMVC中，官方默认使用了jackson的json库支持进行json的封装。

- 使用JackSon处理json：

    - 需要导入jackson.jar及相关依赖，并需要在config中开启springmvc驱动。

```java
@RequestMapping(value = "/hello")
@ResponseBody
public <响应对象> hello() {
   ...
       
  // 直接返回响应的对象，springmvc会自动将其转化为json格式发给客户端。
  return <响应对象>;
}
```

# 三、SpringMVC过滤器

## 1、REST风格

### （1）概念

- REST即表述性状态传递（英文：Representational State Transfer，简称REST）是Roy Fielding博士在2000年他的博士论文中提出来的一种软件架构风格。它是一种针对网络应用的设计和开发方式，可以降低开发的复杂性，提高系统的可伸缩性。

- REST风格在SpringMVC中的体现：

    - 原本的HTTP请求发送：

        - 增：insertXXX post请求

        - 删：deleteXXX get请求

        - 改：updateXXX post请求

        - 查：selectXXX get请求

    - REST风格HTTP请求发送：

        - 增：/XXX post请求

        - 删：/XXX delete请求

        - 改：/XXX put请求

        - 查：/XXX get请求


### （2）SpringMVC与REST

- 问题引入：在SpringMVC中一共可以接收八种请求的方式（GET，POST，HEAD，OPTIONS，PUT，PATCH，DELETE，TRACE），而客户端只能发送两种请求的方式（GET，POST）。所以当要使用REST风格的HTTP请求时，只能将客户端所发送的请求进行一种转化，最终在服务端被相同的请求处理器接收。


### （3）使用SpringMVC中Filter过滤器进行转化

- 在SpringMVC中为我们写好了一个完整的Filter过滤器，通过这个过滤器可以实现每次发送到服务器的请求都被过滤后转化为REST风格的请求。只需要在web.xml中配置这个过滤器即可。

- 配置过滤器：

```xml
<filter>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  // 配置SpringMVC中写好的过滤器
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>

<filter-mapping>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  // 所有请求都被拦截
  <url-pattern>/*</url-pattern>
</filter-mapping>
```
- 过滤器原理：

    - 参考SpringMVC源码HiddenHttpMethodFilter类：

        - 其中doFilterInternal方法中有一个嵌套if语句：

        - 在第一层会判断你的请求是否是“POST”请求。

        -  在第二层会判断中会判断你的请求发送是否有“_method”参数。

        - 如果都满足则开始进行请求的转化：

        - 首先会将_method参数的值转化成大写。

        - 然后通过重写getMethod()方法，将该值返回。

- 效果：

```html
// 通过转化之后，真正的请求方式其实就是_method的值
<form action="..." method="POST">  // 满足条件1 POST请求
  // 满足条件2 请求带有_method参数。值为真正的请求方式。
  <input type="hidden" name="_method" value="[PUT | DELETE]"/>
  ...
</form>  
```


### （2）通过AJAX直接发送REST请求

- 在AJAX的同步或异步请求中，都可以直接通过type属性指明要发送的八种请求。


## 2、编码过滤器

### （1）概念

- 在JavaWeb中使用servlet接收客户端发送的请求带有中文参数时，一般需要使用request.setCharacterEncoding("utf-8”);方法进行指定utf-8转码。

- 在SpringMVC中，官方已经配置好了一套过滤器以供转码使用，直接在web.xml中配置使用即可。（注意：该过滤器必须配置在所有过滤器的第一个（缓存问题））


### （2）使用
```xml
<filter>
  <filter-name>CharacterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    
  // 为encoding属性进行编码赋值
  <init-param>
     <param-name>encoding</param-name>
     <param-value>UTF-8</param-value>
  </init-param>

  // 在新版本的CharacterEncodingFilter类中需要额外配置打开
  // private boolean forceRequestEncoding = false;
  // private boolean forceResponseEncoding = false;

</filter>

<filter-mapping>
  <filter-name>CharacterEncodingFilter</filter-name>
  // 对所有请求都进行过滤
  <url-pattern>/*</url-pattern>
</filter-mapping>
```


# 四、视图解析

## 1、概念

- SpringMVC解析视图的过程：

    - 一般处理请求的方法可能会返回null，String，ModelAndView，ModelMap等相关对象。而无论返回什么，他最终都会被封装成一个ModelAndView对象，然后视图解析器使用此对象解析视图并进行页面的跳转。

- 回顾HelloWorld中配置的视图解析器bean：

```xml
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="prefix" value="/WEB-INF/views/"/>
  <property name="suffix" value=".jsp"/>
</bean>
```


## 2、View接口的三个实现类

- 在SpringMVC中， 使用view接口的实现类进行处理模型数据和页面跳转，该接口主要有InternalResourceView（转发视图）、JstlView（转发视图）、RedirectView（重定向视图）三个实现类。以及一个增强实现类InternalResourceViewResolver（视图解析器）类


## 3、转发与重定向

- 在返回的view字符串中使用前缀即可指定使用转发视图还是重定向视图：

- 转发：

```java
public String hello() {
  return "redirect:<视图名>";
}
```
- 重定向：

```java
public String hello() {
  return "forward:<视图名>";
}
```


## 4、视图解析类的选择

- 源码分析：UrlBasedViewResolver类createView方法。

```java
// 传入的视图名前缀是否带有"redirect:"
if (viewName.startsWith(REDIRECT_URL_PREFIX)) {

  ...  // 按转发操作new相应的类并返回
  return applyLifecycleMethods(REDIRECT_URL_PREFIX, view);
}

// 传入的视图名前缀是否带有"forward:"
if (viewName.startsWith(FORWARD_URL_PREFIX)) {
    
  ...  // 按重定向操作new相应的类并返回
  return applyLifecycleMethods(FORWARD_URL_PREFIX, view);
}

// 都不是，则调用父类的createView new相应的视图
// 该方法最终会回调UrlBasedViewResolver类的buildView方法进行构造
// 其中第二行view.setUrl(getPrefix() + viewName + getSuffix());
// 即最终采用视图解析器中的头+视图名+尾进行view的创建。
return super.createView(viewName, locale);
```


# 五、异常处理

（1）SimpleMappingExceptionResolver类

# 六、springmvc-config的其他标签

## 1、静态资源处理

### （1）概念

- 在SpringMVC中配置的核心控制器，其本质是将发送到服务器的所有请求都看作是一个对处理器映射的请求。所以，当浏览器只想获取一个静态资源的时候（如css,js等），也会发送一个请求，但这个请求没有映射，只有资源路径。所以此时springMVC将会出现错误。


### （2）解决

- 使用此标签在spirngMVCconfig.xml中配置后，他会在WEB容器启动的时候会在上下文中定义一个DefaultServletHttpRequestHandler，它会对DispatcherServlet的请求进行处理，如果该请求已经作了映射，那么会接着交给后台对应的处理程序，如果没有作映射，就交给WEB应用服务器默认的Servlet处理，从而找到对应的静态资源，只有再找不到资源时才会报错。

```xml
<mvc:default-servlet-handler/>
```
## 2、SpringMVC驱动

### （1）概念


```xml
<mvc:annotation-driven/>
```





