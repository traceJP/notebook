**vue-ajax是一个以vue为基础的第三方js组件，主流的有vue-resource、vue-axios。**

- vue-axios：https://github.com/axios/axios

 

## 一、HelloAxios：

（1）导包：需要使用axios.js包。

（2）hello：

```javascript
// 发送一条get请求。get可以改为post
axios.get('<请求地址>')
  
  // 当请求成功时执行该方法，响应的数据传入response形参
  .then(function (response) {
    ...
  })
  
  // 当请求失败时执行该方法，错误信息传入error形参  
  .catch(function (error) {
    ...
  })

  // 无论如何都会执行此方法  
  .then(function () {
    ...
  });
```
- js代码使用位置：

    - 可以在原生JavaScript代码中使用。
    - 在vue-methods属性中定义一个监听器，在监听器方法中使用此代码。当监听器触发时，就会发送一个请求。

 



