## 一、setup钩子

- 在Vue3中，所有的生命周期、变量、函数都将写在setup钩子中。

```vue
<template>
  ...
</template>

<script>
  export default {
    name: 'App',
      
    // setup钩子
    setup() {

      // 定义一个普通变量
      let name = '张三';

      // 定义一个普通方法
      funcation sayHello() {
        alter('hello');
      }

      // 需要交出去对template暴露的对象
      // 只有返回的变量，才能在template中使用
      return {}

    }
  }
</script>
```
### 1、setup返回渲染函数

- 定义setup的返回值直接渲染视图，则此时自动忽略```<template>```块代码

```vue
import {h} from 'vue';

export default {
  ...
  setup() {
    // 返回一个渲染函数
    return () => h('h1', 'JP233');
  }
}
```
### 2、使用```<script setup>```快速暴露返回的变量

- 在 setup() 函数中手动暴露大量的状态和方法非常繁琐。我们可以通过使用构建工具来简化该操作。当使用单文件组件（SFC）时，我们可以使用``` <script setup> ```来大幅度地简化代码。

```vue
// 在script标签中增加setup属性
<script setup>
  // 导包、定义变量、方法、或者钩子函数等操作都将自动暴露给<template>中使用
  // 无需使用setup()函数包裹
  import { reactive } from 'vue' 
    
  const state = reactive({ count: 0 })
  function increment() {
   state.count++
  }
</script>
```


## 二、ref函数

- 在vue3中，如果需要标识一个变量为响应式数据，需要使用ref函数进行定义。

```javascript
// 导入ref变量
import {ref} from 'vue';

export default {
  ...
  setup() {
    // 定义一个响应式数据变量
    let name = ref('张三');
    
    // 暴露该变量
    return {
      name,
    }
  }
}
```
### 1、深入理解ref函数

- ref函数会将你的变量包装成RefImpl对象（引用对象）来实现响应式。

- 所以在```<script>```中需要使用 变量.value 的形式进行引用和赋值。

- 而在```<template>```中直接使用变量即可。

```javascript
let number = ref(1);

// 修改（不能使用number = 2来修改）
number.value = 2;
```
### 2、使用ref函数对基本类型和对象类型进行定义

- 使用ref函数可以定义基本类型和对象类型。

    - 基本类型：响应式原理依然采用vue2中的Object.defineProperty()的get和set方法实现。

    - 对象类型：响应式原理则采用vue3中的reactive函数（即JS-ES6规范中的代理器对象原理）。

- 基本类型和对象类型对比：

```javascript
let number = ref(1);
number.value = 2;

let job = ref({
  type: '工程师',
  salary: 30,
})

// 赋值和引用响应式依旧是在.value后
job.value.type = '经理';
```

## 三、reactive函数

- 与ref对象基本类似，但是只能定义对象类型。

- reactive和ref区别：

    - reactive：定义深层次的响应式，```<script>```中直接通过对象进行引用和赋值即可，无需引用.value属性。

    - ref：可以处理基本类型，但是```<script>```需要通过.value进行引用

```javascript
import {reactive} from 'vue';

// setup
let job = reactive({
  type: '工程师',
  salary: 30,
})

job.type = '经理';  // 赋值和引用响应式
```
**1****、多角度对比reactive和ref：**

- 从定义数据角度对比：

    - ref用来定义：基本类型数据。

    - reactlve用来定义：对象（或数组）类型数据。

    - 备注：ref也可以用来定义对象（或数组）类型数据，它内部会自动通过reactive转为代理对象

- 从原理角度对比：

    - ref通过object.defineProperty()的get与set来实现响应式（数据劫持）。

    - reactive通过使用Proxy来实现响应式(数据劫持)，并通过Reflect操作源对象内部的数据。

- 从使用角度对比：

    - ref定义的数据：操作数据需要.value，读取数据时模板中直接读取不需要.value

    - reactive定义的数据：操作数据与读取数据均不需要.value 


## 四、组件props与setup

- 在vue3中组件传递props参数，依旧需要使用props进行声明，并且需要在setup中形参进行声明。

- 并且，在vue3中组件接收到的props参数都是响应式的数据。

```javascript
// 父组件传递foo属性
export default {

 // vue3中需要
 props: ['foo'],
 setup(props, context) {
  // 接收props作为第一个参数
  console.log(props.foo)

  // context（上下文）对象：
  // 包含父组件传递过来的
  // attrs（未接收属性）、emit（事件）、slots（插槽）对象
     
 }
}
```
### 1、```<script setup>```下的props接收

- 使用defineProps进行接收

```vue
<script setup>
    import {defineProps} from 'vue';

    // 定义接收
    const props = defineProps(['foo'])
    console.log(props.foo)
</script>
```
### 2、配合TypeScript使用接收props

- 参考TypeScript与组合式API文档https://cn.vuejs.org/guide/typescript/composition-api.html#typing-component-props

```vue
<script setup lang="ts">
    defineProps<{
     title?: string
     likes?: number
    }>()
</script>
```

## 五、Computed计算属性

- 在setup中定义computed函数即可。

```javascript
import {computed} from 'vue';

setup() {
  // computed函数传入回调函数，返回计算属性的值
  const a = computed(() => {
    return "<计算出的值>";
  })

  // 完整的计算属性写法
  const b = computed({
    get() {
      // 获取回调
      return "";
    },
    set(value) {
      // 修改回调
    }
  })
}
```


## 六、Watch监视属性

- 在setup中定义watch函数即可。

```javascript
import {watch} from 'vue';

setup() {

  // 1 监视ref数据
  let sum = ref(0)
  watch(sum, (newValue, oldValue) => {
    // 监视回调
  }, {immediate: true,...})  // 第三个参数为监视配置

  // 2 监视多个响应式数据
  // 直接传入数组参数即可，此时newValue和oldValue也为数组
  watch([sum, ...], (newValue, oldValue) => {
    // 监视回调
  })

  // 3 监视reactive数据
  let person = reactive({
    name: '张三',
    age: 18,
  })

  // 3.1 此处强制开启深度监视，deep配置无效
  watch(person, (newValue, oldValue) => {
    // 监视回调
  })

  // 3.2 监视reactive其中一个属性（需要在回调函数中返回对象属性）
  watch(
  () => {
    // 指定监视属性 简写 () => person.name 
    return person.name;
  } , 
  (newValue, oldValue) => {
    // 监视回调
  })

  // 3.3 监视reactive的某些属性（依旧使用数组，内部使用回调返回对象属性）
  watch([() => person.name, () => person.age], 
  (newValue, oldValue) => {// 监视回调})

}
```

## 七、WatchEffect函数（新增）

- 类似watch和computed函数结合，但是不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

    - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。

    - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

```javascript
import {watchEffect} from 'vue';

setup() {
  // 若sum.value和sum.age变化，则回调执行
  // 并且自动开启immediate: true
  watchEffect(() => {
    const x1 = sum.value;
    const x2 = sum.age;
    // 无需返回值
  })
}
```


## 八、自定义hook函数（重点）

- 类似与vue2中的mixin的用途，是对vue2中mixin的增强。

- 本质是一个函数，把setup函数中使用的组合式API进行了封装。

```vue
// 使用单独的js文件  hooks/useXXX.js
export default funcation() {
  // 变量、方法、钩子等<script>代码
  ...
  // 交出hook对外暴露的元素
  return {}
}

// 使用hook
import useXXX from '../hooks/useXXX';
setup() {
  // 使用js文件，接收暴露的值
  const res = useXXX();
}
```

## 九、toRef和toRefs函数

- 创建一个ref对象，使其value值指向另一个对象中的某个属性，即把一个不是ref的东西，变成是ref的东西。

```javascript
import {toRef} from 'vue';

setup() {
  let person = reactive({
    name: '张三',
    age: 18,
  })

  // name1为非响应式数据（单独的一个值）
  const name1 = person.name;

  // 为name2绑上响应式（类似修改指针，为person的一个属性别名引用）
  // 此时name2为响应式数据
  // ***并且与person.name指向同一个地址***
  const name2 = toRef(person, 'name');

  // 也可以在返回值中使用toRef
  return {
    name: toRef(person, 'name'),
  }

}
```
- 使用toRefs批量创建ref对象

```javascript
import {toRefs} from 'vue';

setup() {
  let person = reactive({
    name: '张三',
    age: 18,
  })

  // 为person取个别名x
  const x = toRefs(person);

  // 在返回值中借助ES6解构语法和toRefs一次解构
  return {
    ...toRefs(person)
  }

}
```


## 十、其他函数

- 参考官方文档响应式进阶https://cn.vuejs.org/api/reactivity-advanced.html


 



 