## 一、helloVue：

（1）导包：

- 在使用vue的时候，js文件是需要执行的文件，所以自己写的js文件必须放在body中。而vue.js文件可以放在在head中。


（2）hello：

- html：

```html
// 被绑定的元素，其Vue对象中绑定过后所有的操作只对该元素内部有效
<div id="app">
  {{ message }}  // 使用vue中定义的值
</div>
```
- js：

```javascript
var app = new Vue({
  // el属性：挂载点，可以指定css选择器，如类或id
  el: '#app',  
  // data属性：绑定的数据，其数据为双向绑定(双方都可以导致对方数据变化) 
  data: {  
    // 数据对象，定义的每个值都可以在html中使用（key-value）
    message: 'Hello Vue!',
    ...
  }
})
```


## 二、vue指令：

- 带有前缀v-开头的vue表达式一般为为vue指令


### （1）v-bind指令：（绑定html属性）

- 效果：可以通过带有v-bind的形式绑定一个html标签中任意一个属性（包括class或者id属性等）。然后此属性值将会和vue实例中的数据模型值保持一致。

- 语法糖：直接使用冒号+属性即可。

```html
<span :title="message">
```
- html：

```html
<div id="app-2">
 // v-bind为一个vue指令，title为span的一个属性，其值message为vue数据对象
 <span v-bind:title="message">
  鼠标悬停几秒钟查看此处动态绑定的提示信息！
 </span>
</div>
```
- js：

```javascript
var app2 = new Vue({
 el: '#app-2',
 data: {
  message: '页面加载于 ' + new Date().toLocaleString()
 }
})
```


### （2）v-if指令：（条件显示隐藏）

- 效果：根据表达式的boolean值切换元素的显示和隐藏。注意：该表达式可以是JavaScript中支持的表达式，如大于小于等于等。

- html：

```html
<div id="app-3">
  // 值也可以是数据对象加表达式：seen（此时seen需要为数字）>30
  <p v-if="seen">现在你看到我了</p>
</div>
```
- js：

```javascript
var app3 = new Vue({
 el: '#app-3',
 data: {
  seen: true  // 当seen数据对象为false时，p标签会被隐藏
 }
})
```


### （2.1）v-else指令和v-if-else指令：

- 效果：嵌套分支

- v-else：

```html
<div v-if="Math.random() > 0.5">
	 Now you see me
</div>

<div v-else>    // 注意：v-else指令必须紧跟在v-if或v-else-if后边
 	Now you don't
</div>
```
- v-if-else：

```html
<div v-if="type === 'A'">
 A
</div>

// 注意：v-else-if指令必须紧跟在v-if或v-else-if后边
<div v-else-if="type === 'B'">
 B
</div>

<div v-else-if="type === 'C'">
 C
</div>

<div v-else>
 Not A/B/C
</div>
```


### （3）v-for指令：（循环）

- 效果：在vue-data中可以定义一个数组类型的数据。然后在html中可以使用该指令遍历这个数组。

- html：

```html
<div id="app-4">
 <ol>
 // v-for中需要使用表达式：元素 in 数据对象
 // 如Java中的 foreach(int a : list)
  <li v-for="todo in todos">  // 整个li标签都会被复制然后遍历
   {{ todo.text }}
  </li>
 </ol>
</div>
```
- js：

```javascript
var app4 = new Vue({
 el: '#app-4',
 data: {
   // 数组对象todos，一共有三个元素。
  todos: [
   { text: '学习 JavaScript' },
   { text: '学习 Vue' },
   { text: '整个牛项目' }
  ]
 }
})
```


### （4）v-on指令：（监听器注册）

- 效果：在Vue对象中可以定义一个methods属性，该属性中又可以指定多个属性（每个属性对应一个监听器函数）。然后可以通过v-on指令调用这些函数，以达到onClick单击事件的效果。

- 语法糖：使用@符号+事件名即可

```html
<button @click="reverseMessage">
```
- html：

```html
<div id="app-5">
  <p>{{ message }}</p>
    
 // 为button注册一个监听器
 // v-on指令需要指定一个事件，如click事件，其值为methods属性中定义的属性。
 <button v-on:click="reverseMessage">反转消息</button>
</div>
```
- js：

```javascript
var app5 = new Vue({
 el: '#app-5',
 data: {
  message: 'Hello Vue.js!'
 },

 // 使用methods属性定义多个监听器
 methods: {
  // 定义一个事件reverseMessage，触发事件时执行function。
    // this代表此vue对象其中的属性。
  reverseMessage: function () {
   this.message = this.message.split('').reverse().join('')
  }
  ...
 }
})
```


### （4.1）v-on指令传递参数到监听器中：

- 效果：监听器定义的函数可以定义形参，然后由html传入实参。

- html：

```html
<div id="example-3">
 // 传入形参hi，因为外部有双引号，所以使用单引号包裹形参。
 <button v-on:click="say('hi')">Say hi</button>
 <button v-on:click="say('what')">Say what</button>
</div>
```
- js：

```javascript
new Vue({
 el: '#example-3',
 methods: {
   // 定义形参message
  say: function (message) {
   alert(message)
  }
 }
})
```


### （5）v-model指令：（表单绑定）

- 效果：使用该指令可以使一个表单元素中的值（如input-text中值）和一个数据对象进行双向绑定，注意是双向绑定。

- html：

```html
<div id="app-6">
  <p>{{ message }}</p>
    
 // v-model属性值为绑定的数据对象
 <input v-model="message">
</div>
```
- js：

```javascript
var app6 = new Vue({
 el: '#app-6',
 data: {
  message: 'Hello Vue!'
 }
})
```



