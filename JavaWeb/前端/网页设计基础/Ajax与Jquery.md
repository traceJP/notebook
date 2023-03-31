## 一、使用JQuery实现Ajax：

（1）$.ajax方法：

- 属性：

    - url：表示请求的地址

    - type：表示请求的类型GET或POST请求

    - data：表示发送给服务器的数据：name=value&name=value或者{key:value}

    - dataType：预计服务器响应的数据类型（text\xml\json）

    - success：请求成功后响应的回调函数

- 方法示例：
```javascript
$("#test").click(
  function() {  
    $.ajax({
       url: "请求路径",
       data:"username=admin&password=123",
       type: "GET",
       dataTyep:"test",
       // 注意：必须在回调函数中加一个响应的形参
       success: function(data) {
         alter(data)
       }
     })
  }
)
```





