## 一、使用JavaScript实现的原生Ajax：

（1）Ajax请求步骤：

- 发送请求到服务器
```javascript
function request() {
  // 1 创建一个请求对象
  let req = new XMLHttpRequest()
  
  // 2 使用请求对象的open方法描述请求
​    // 方法的method参数，指定请求方式
  let method = "GET"

​    // 方法的url参数，指定请求地址
  let url = "http://localhost:8080/TestSpringMVC/hello"

​    // 方法的async参数，指定是同步请求（false），还是异步请求（true）
  let async = true
  req.open(method, url, async)

  // 3 使用请求对象的send方法发送请求到服务器
  req.send()

}
```


（2）Ajax响应步骤：

- 发送请求后接收服务器响应的数据
```javascript
// 使用请求对象的onreadystatechange函数响应
Iable.onreadystatechange = function() {

  // 当状态准备就绪后执行响应的方法
  if(Iable.readyState == 4 && Iable.status == 200) {

    // 获取响应的字符串
    let returnText = Iable.responseText

    // 输出
    alert(returnText)

  }

}
```
-  onreadystatechange函数：
    - 该函数是请求对象的一个存储函数。每当readyState状态发生改变的时候，将会调用一次此函数。


- readyState属性：

    - 存有 XMLHttpRequest 的状态。从0到4发生变化。

        - 0：请求未初始化

        - 1：服务器连接已建立
        - 2：请求已接收
        - 3：请求处理中
        - 4：请求已完成，且响应已就绪
- status属性：

    - 200：“OK”
    - 404：未找到页面
- 响应数据代码解析：本质上当一个请求在发送后，将触发5次onreadystatechange函数。分行别对应readyState属性从0到4的变化。所以在接收服务器返回的数据时，需要使用if判断当前的状态是什么，如果服务器的响应已就绪。即可准备接收数据。




## 二、CORS同源策略：



不同域的请求，客户端不会接收服务器的响应。会在ajax中send方法出现异常。

 

 