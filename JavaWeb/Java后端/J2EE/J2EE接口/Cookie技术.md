# 一、概念

- Cookie翻译过来是饼干的意思。

- Cookie是服务器通知客户端保存建值对的一种技术。

- 客户端有了Cookie后，每次请求都发送给服务器。

- 每个Cookie的大小不能超过4kb，且cookie中的value值不能为空格、 各种括号、逗号、中文等。


# 二、如何创建Cookie

- 在servlet中创建：

    - 可以同时创建多个Cookie对象。

```java
// 1 创建Cookie对象
Cookie cookie = new Cookie(name:"key值", value:"value值");

// 2 通知客户端保存Cookie
resp.addCookie(cookie对象);
```
- Cookie创建和保存原理：


![clipboard.png](Cookie%E6%8A%80%E6%9C%AF.assets/clip_image002.gif)

 

# 三、获取Cookie

- 服务器获取客户端的Cookie：

```java
// 返回Cookie[]：因为可以有多个Cookie，所以返回数组
req.getCookies();

// 通过遍历Cookie[]可以使用以下方法：
cookie.getName();  // 返回Cookie的key值
cookie.getValue();  // 返回Cookie的value值
```

# 四、修改Cookie的值

- 方法一：重新创建一个要修改的Cookie的同名（key值相同）对象。并同值客户端保存Cookie。

- 方法二：

```java
// 1 找到需要修改的Cookie对象：通过遍历getCookies()的集合
Cookie cookie = ...

// 2 调用setValue()方法赋新的Cookie值
cookie.setValue(新的value值);

// 3 调用同值客户端保存修改方法
resp.addCookie(cookie对象);
```

# 五、Cookie生命控制

- Cookie的生命控制指的是如何管理Cookie什么时候被销毁（删除）。

```java
cookie.setMaxAge(数值);  // 设置存活时间
// 该数值可以为：
//  正数：表示在指定的秒数后删除
//  负数：表示浏览器一关，Cookie就会被删除（默认值是-1） Session级别
//  0：表示马上删除Cookie，注意需要调用addCookie方法通知客户端修改
```

# 六、Cookie技术用户名免登录原理

![clipboard.png](Cookie%E6%8A%80%E6%9C%AF.assets/clip_image004.gif)

 

 



 