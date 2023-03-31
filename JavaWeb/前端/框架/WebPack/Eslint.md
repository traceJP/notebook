## 一、Eslint

- ESLint是在ECMAScript/JavaScript代码中识别和报告模式匹配的工具，它的目标是保证代码的一致性和避免错误。即用来检测js和jsx语法的工具，通过配置Eslint配置文件，可以配置各项功能。将来运行Eslint时就会以写的规则对代码进行检查。


### 1、配置文件

- 配置文件与webpack配置文件写在同一目录，并且有多种文件类型选择，区别在于具体的配置格式不一样。

    - .eslintrc.*（新建文件，位于项目根目录。注意命名前面有个点。）

        - .eslintrc

        - .eslintrc.js

        - .eslintrc.json

- 常见配置（以.eslintrc.js为例）

    - 具体配置详见官方文档：https://eslint.bootcss.com/docs/user-guide/configuring


```javascript
// ESlint 检查配置
module.exports = {
  // 解析选项
  parserOptions: {
      
    // ES 语法版本
    ecmaversion: 6,

    // ES模块化
    sourceType:"module",

    //ES其他特性
    ecmaFeatures: {

      // 如果是React项目，就需要开启jsx语法
      jsx: true,
    }
  },

  // 具体检查规则
  // "off"或0 关闭规则
  // "warn"或1 开启规则，使用警告级别的错误:warn(不会导致程序退出)
  // "error"或2 开启规则，使用错误级别的错误:error(当被触发的时候，程序会退出)
  rules: {
      
    // 禁止使用分号
    semi: "error",
      
    // 强制数组方法的回调函数中有return 语句，否则警告
    'array-callback-return':'warn',
    "default-case':[
      
      // 要求 switch 语句中有 default 分支，否则警告
      'warn',

      // 允许在最后注释 no default,就不会有警告了
      { commentPattern: '^no default$'},
    ],

    eqeqeq: [
      // 强制使用--=和!==,否则警告
      'warn',
      // https://eslint.bootcss.com/docs/rules/eqeqeq#smart除了少数情况下不会有警告
      'smart'
    ],
  },

  // 直接继承已经有的其他规则（详见官方文档）
    // Eslint官方的规则：eslint:recommended
    // Vue Cli官方的规则︰plugin:vue/essential
    // React Cli官方的规则：react-app
  extends: [
    // 若需要覆盖继承的规则，则直接在rules中写自己的规则即可覆盖
  ],
}
```

### 2、Webpack使用Eslint

- 详细步骤参考官方文档：https://webpack.docschina.org/plugins/eslint-webpack-plugin/#root

- 通过npm下载eslint的依赖

```shell
npm install eslint-webpack-plugin --save-dev

npm install eslint --save-dev
```
- 将插件添加到webpack的plugins配置项中

```javascript
module.exports = {
 // ...
 plugins: [
     
   // new一个eslint对象
   new ESLintPlugin({
     // eslint配置选项
   })
 ],
 // ...
};
```



