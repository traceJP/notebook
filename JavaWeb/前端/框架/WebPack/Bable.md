## 一、Bable

- Bable是JavaScript编译器。主要用于将ES6语法编写的代码转换为向后兼容的JavaScript语法，以便能够运行在当前和旧版本的浏览器或其他环境中。主要是通过配置文件来指示编译。

- 官方文档：https://www.babeljs.cn/docs/


### 1、配置文件

- 配置文件与webpack配置文件写在同一目录，并且有多种文件类型选择，区别在于具体的配置格式不一样。
    - babel.config.*（新建文件，位于项目根目录）
        - babel.config.js

        - babel.config.json

    - .babelrc.* （新建文件，位于项目根目录，注意文件名前面有个点）
        - .babelrc

        - .babelrc.js

        - .babelrc.json

    - package.json中babel（不需要创建文件，在原有文件基础上写）



### 2、常见配置

- 具体配置参考官方文档

```javascript
module.exports = {
  // 预设（简单理解：就是一组Babel插件扩展Babel功能）
    // @babel/preset-env：智能预设，允许您使用最新的JavaScript。
    // @babel/preset-react：用来编译React jsx语法的预设
    // @babel/preset-typescript:用来编译Typescript语法的预设
  presets: [],
}
```


### 3、在Webpack中使用Bable

- 具体可以参考官方文档：https://webpack.docschina.org/loaders/babel-loader/

- 使用npm下载bable的loader相关依赖
```shell
npm install -D babel-loader @babel/core @babel/preset-env webpack
```
- 在webpack的rules中添加配置

```json
rules: [
  {
    test: /\.m?js$/,
    // 排除node_modules中的js文件
    exclude: /(node_modules|bower_components)/,
    use: {
      loader: 'babel-loader',
      // 选项（建议在bable.config.*文件中写，会自动检测到options中）
      options: {
        presets: ['@babel/preset-env']
      }
    }
  }
]
```



