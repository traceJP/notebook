## 一、2.0时代

### 1、最初的VUE

- 在vue2中，前端项目框架选型通常如下，并且采用JavaScript开发。

    - webpack+vue-cli

    - vue2

    - vue-route

    - axios

    - vuex

    - UI框架

    - ....

- 其中Vue书写风格则采用最传统的选项式API（Options API）书写风格。

- 选项式API风格如下所示。

```javascript
export default {
  ...
  data: () => {
    ...
  },
  methods() { ... }
}
```

### 2、更加面向对象的JavaScript风格的VUE

- 显然，对于上面选项式API的书写风格，不符合面向对象编程的原则，因Vue2与TypeScript语言的不完美兼容，和设计不完善。所以提供了带有class模式的vue组件代码风格（旨在让js代码更加扁平化）。只需要引入相应的包，即可以更加符合面向对象的方法书写代码。

- class模式

    - vue-class-component

    - vue-property-decorator（是vue-class-component项目的超集，完全继承于该项目，拥有更多的包装API）

- class模式风格如下所示。

```javascript
// class模式风格（更加简洁高效和面向对象）
@Component
export default class Name extends Vue {
  testData = "";
  testfun() { ... }
}

// 上述代码相当于
export default {
  name: "Name",
  data: () => {
    testData: "",
  },
  methods() {
    testfun() {
      ...
    },
  },
}
```


## 二、3.0时代

- 在Vue3中，前端项目框架选型如下，并且采用TypeScript开发。

    - vue-cli改为vite

    - vuex改为pinia

- 其中Vue代码风格应抛弃传统的书写风格（选项式API和class模式组件），而采用vue3中推荐的以TypeScript为中心的组件式API（Composition API）书写风格。并且应当使用TypeScript编写更加容易维护的代码。

    - 组合式API概念官网：https://vuejs.org/guide/extras/composition-api-faq.html#more-flexible-code-organization

- 对比两种风格，选项式API的业务逻辑分散，而组件式API的业务逻辑紧密，易于维护编码。


![clipboard.png](Vue%E7%89%88%E6%9C%AC%E5%8F%98%E8%BF%81.assets/clip_image002.gif)

 