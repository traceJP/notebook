## 一、概念

### 1、打包工具

- 什么是打包工具：将整个项目打包成一个文件进行输出。

- 打包工具的用处：开发时，我们会使用框架(React、Vue)，ES6模块化语法，Less/Sass等css预处理器等语法进行开发。这样的代码要想在浏览器运行必须经过编译成浏览器能识别的JS、Css等语法，才能运行。所以我们需要打包工具帮我们做完这些事。除此之外，打包工具还能压缩代码、做兼容性处理、提升代码性能等。


### 2、WebPack

- 本质上，webpack是一个用于现代JavaScript应用程序的静态模块打包工具。当webpack处理应用程序时，它会在内部从一个或多个入口点构建一个依赖图（dependency graph），然后将你项目中所需的每一个模块组合成一个或多个bundles，它们均为静态资源，用于展示你的内容。

- 官方文档：https://webpack.docschina.org/concepts/


### 3、HelloWorld

- 使用npm工具初始化前端工程

```shell
npm init -y
```
- 使用npm下载webpack依赖

```shell
npm i webpack webpack-cli -D
```
- 运行webpack指令进行打包

```shell
npx webpack <入口文件> [参数]
// 入口文件：指定webpack从哪个js文件开始打包，还会将其依赖也一起打包进来。
```


## 二、基本配置

### 1、五大核心概念

#### （1）entry (入口)

- 指示Webpack从哪个文件开始打包。该文件就是整个工程的核心文件，其他的文件都需要被该文件引用才可以参与打包。例如vue-cli中的main.js文件。是整个vue的核心。


#### （2）output(输出)

- 指示 Webpack打包完的文件输出到哪里去，如何命名等。

- 默认输出为./dist目录


#### （3）loader (加载器)

- webpack本身只能处理js、json等资源，其他资源（如css，less）需要借助loader，Webpack才能解析。


#### （4）plugins(插件)

- 扩展Webpack的功能


#### （5）mode(模式)

- 主要由两种模式

    - 开发模式：development

    - 生产模式：production


### 2、配置文件

- 配置文件与npm前端工程文件package.json文件写在一起。并且有固定的配置文件名（不建议修改）：webpack.config.js

- 使用配置文件，则无需在npx webpack命令中添加参数和入口文件参数，只需要执行npx webpack命令即可完成打包工作，具体参数则使用配置文件webpack.config.js中的参数。

- 配置文件模板：

```javascript
const path = require('path');  // node.js核心模块，专门用于处理路径问题

// npm模块固定写法
module.exports = { 

  // 入口（相对路径）
  entry: './path/to/my/entry/file.js',

  // 输出
  output: {
    // 所有文件的输出路径（绝对路径）
    path: path.resolve(__dirname, 'dist'),

    // 文件名
    filename: 'my-first-webpack.bundle.js',

    // 自动清空上次打包结果（在打包前将上次目录删除）
    clean: true,
  },

  // 加载器
  module: {
    rules: [
      // loader的配置
    ]
  },
    
  // 插件
  plugins: [
    // plugins的配置
  ],

  // 模式（开发环境 | 生产环境）
  mode: "development | production",

};
```


## 三、其他资源处理

- Webpack本身是不能识别样式资源的，所以我们需要借助Loader来帮助Webpack解析样式资源。可以在对应的官方文档找到相应的Loader文档，进行指令下载和配置，即可进行相应资源的处理。

- 官方Loader目录：https://webpack.docschina.org/loaders/


### 1、处理CSS相关资源（SCSS等CSS框架同理）

- 注意：创建需要使用的css文件需要在入口文件（main.js）中引用。

```javascript
import "<css资源>"
```
- 根据官方文档，通过node.js下载处理css文件需要的loader依赖

```shell
npm install --save-dev css-loader
npm install --save-dev style-loader
```
- 在webpack.config.js文件中新增rules的配置

```javascript
rules: [
   {
    test: /\.css$/i,  // 正则表达式（只检测.css后缀的文件）
    use: [  // 执行顺序是从右到左（从下到上）
      "style-loader",  // 将js代码的css以style的标签插入html中
      "css-loader"  // 将css文件编译成js代码
    ],
   },
   // ...其他loader的rules配置
],
```
- 运行打包命令，即可将css资源打包编译

```shell
npm webpack
```
**2****、处理静态资源（图片等其他格式文件）**

- 在webpack4版本，则使用raw-loader、url-loader、file-loader进行处理。而在webpack5最新版本中，处理静态资源功能已经默认加入了webpack之中，只需要参考官方文档进行rules的相关配置即可实现。

- 官方图片配置文档：https://webpack.docschina.org/guides/asset-modules/

- 相关配置：（通用资源类型webpack.config.js）

```javascript
rules: [
  {
    test: /\.txt/,  // 正则检测资源，自定义要添加资源如png,jpg等
    type: 'asset',
    parser: {
      dataUrlCondition: {
        maxSize: 4 * 1024   // 4kb
      }
    },
      
    generator: {
      // 指定统一静态文件输出的目录和名称
      // [hash:10]取hash值前10位
      filename: "static/images/[hash:10][query]",
    },
  }
]
```
**3****、处理HTML资源**

- 使用处理HTML的Webpack插件可以在自己的html页面中自动引入工程中的js等资源文件。无需手动一个个的添加。

- 详见官网：https://webpack.docschina.org/plugins/html-webpack-plugin/#root


## 四、开发服务器和自动化

- 每次写完代码都需要手动输入指令才能编泽代码，太麻烦了。所以需要npx命令的自动化，即热更新。自动检测代码变化并自动编译。

- 通过npm下载依赖

```shell
npm i webpack-dev-server -D
```
- 在webpack.config.js中配置devServer。

```javascript
module.exports = {
  ...
  // 开发服务器
  devServer: {
    host: "localhost",  //启动服务器域名
    port: "3000",  //启动服务器端口号
    open: true,  //是否自动打开浏览器
  }
}
```
- 使用命令运行服务器

```shell
npx webpack serve
```



