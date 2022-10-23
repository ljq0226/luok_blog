# WebPack基础入门

# 一、认识webpack

[webpack官网](https://webpack.js.org/)

**webpack** is a _**static** **module** **bundler**_ for **modern** JavaScript **applications**. When webpack processes your application, it internally builds a [dependency graph](https://webpack.js.org/concepts/dependency-graph/) from one or more _entry points_ and then combines every module your project needs into one or more _bundles_ , which are static assets to serve your content from.

_webpack_ 是一个现代 JavaScript 应用程序的_静态模块打包器(module bundler)_ 。当 webpack 处理应用程序时，它会递归地构建一个_依赖关系图(dependency graph)_ ，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 _bundle_。

---

webpcak官网的介绍图:

![[Pasted image 20220903085758.png]]

Webpack 是一种前端资源构建工具，一个静态模块打包器。

-   前端资源构建工具：主要理解一下这个前端资源是哪些资源。这些前端资源就是浏览器不认识的 web 资源， 比如 sass、less、ts，包括 js 里的高级语法。这些资源要能够在浏览器中正常工作，必须一一经过编译处理。而 webpack 就是可以集成这些编译工具的一个总的构建工具。
-   静态模块打包器：静态模块就是 web 开发过程中的各种资源文件，webpack 根据引用关系，构建一个依赖关系图，然后利用这个关系图将所有静态模块打包成一个或多个 bundle 输出。

## 为什么我们需要 Webpack

在前端开发过程中：

-   我们需要通过模块化的方式来开发；
-   也会使用一些高级的特性来加快我们的开发效率或者安全性，比如通过ES6+、TypeScript开发脚本逻辑，通过sass、less等方式来编写css样式代码；
-   我们还希望实时的监听文件的变化来并且反映到浏览器上，提高开发的效率；
-   开发完成后我们还需要将代码进行压缩、合并以及其他相关的优化；
-   等等...

但是对于很多的前端开发者来说，并不需要思考这些问题，日常的开发中根本就没有面临这些问题：

这是因为目前前端开发我们通常都会直接使用三大框架来开发：Vue、React、Angular；

但是事实上，这三大框架的创建过程我们都是借助于脚手架（CLI）的；

而Vue-CLI、create-react-app、Angular-CLI都是基于webpack来帮助我们支持模块化、less、

TypeScript、打包优化等的；

# 二、使用Webpack

Webpack 5 运行于 Node.js v10.13.0+ 的版本，安装webpack前请先确保node版本更新。

## 局部webpack的安装

第一步：创建package.json文件，用于管理项目的信息、库依赖等

`npm init`

第二步：安装局部的webpack

`npm install webpack webpack-cli -D`

第三步：使用局部的webpack

`npx webpack`

第四步：在package.json中创建scripts脚本，执行脚本打包即可

![[Pasted image 20220903085820.png]]

`npm run build`

## webpack的配置

在通常情况下，webpack需要打包的项目是非常复杂的，并且我们需要一系列的配置来满足要求，默认配置必然是不可以的。

我们可以在根目录下创建一个webpack.config.js文件，来作为webpack的配置文件：

![[Pasted image 20220903085830.png]]

配置entry和output

```jsx
const path = require('path')
module.exports = {
	entry: {"./src/main.js"},//打包的入口文件
	output:{path:path.resolve(__dirname, './build'),//打包路径
		filename:'bundle.js',//打包之后文件夹名
	}
}
```

### 配置常用loader

### 处理css

当我们尝试在main.js中引入css资源，

![[Pasted image 20220903085844.png]]

运行`npm run build` 之后，会显示

![[Pasted image 20220903085854.png]]

上面的错误信息告诉我们需要一个loader来加载这个css文件，但是loader是什么呢？loader 可以用于对模块的源代码进行转换；我们可以将css文件也看成是一个模块，我们是通过import来加载这个模块的;在加载这个模块时，**webpack 默认支持处理 JS 与 JSON 文件，其他类型都处理不了，这里必须借助 Loader 来对不同类型的文件的进行处理。**

所以对于加载css文件来说，我们需要一个可以读取css文件的loader；

这个loader最常用的是css-loader；

css-loader的安装：

`npm install css-loader -D`

安装完css-loder后，我们最常用的loder配置方式是在我们的webpack.config.js中通过module来进行loader的配置。module.rules中允许我们配置多个loader（因为我们也会继续使用其他的loader，来完成其他文件的加载;这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个loader有一个全局的概览；而css-loader只是将源码的.css文件进行解析，并不会将解析之后的css插入到页面中，此时我们还需要另外一个loader就是style-loader

安装style-loader

`npm install style-loader -D`

### 认识postCSS

什么是PostCSS呢？

PostCSS是一个通过JavaScript来转换样式的工具；

这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置；

但是实现这些功能，我们需要借助于PostCSS对应的插件；

如何使用PostCSS呢？主要就是两个步骤：

第一步：查找PostCSS在构建工具中的扩展，比如webpack中的postcss-loader；

第二步：选择可以添加你需要的PostCSS相关的插件；

安装postCSS

`npm install postcss postcss-cli -D`

而webpack中使用postcss是使用postcss-loader来处理的，所以需安装

`npm install postcss-loader -D`

在配置postcss-loader时，通常使用一个插件：postcss-preset-env

postcss-preset-env是一个postcss的插件；它可以帮助我们将一些现代的CSS特性，转成大多数浏览器认识的CSS，并且会根据目标浏览器或者运行时环境添加所需的polyfill；

也包括会自动帮助我们添加autoprefixer（所以相当于已经内置了autoprefixer）；

首先，我们需要安装postcss-preset-env：

`npm install postcss-preset-env -D`

postcss配置通常抽离成postcss.config.js,之后在rules中只需引入postcss-loader
![[Pasted image 20220903085918.png]]

module.rules的配置如下：

```jsx
const path = require('path');

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\\.css$/, //正则表达式匹配相应的loader处理文件
        // 1.loader的写法(语法糖)
        // loader: "css-loader"

        // 2.完整的写法
        use: [
          // {loader: "css-loader"}
          "style-loader",
          "css-loader",
          "postcss-loader"//当有多个loader需要加载时，顺序自下而上
					//当处理loader需要调用插件时，
          // {
          //   loader: "postcss-loader",
          //   options: {
          //     postcssOptions: {
          //       plugins: [
          //         require("autoprefixer")//需提前安装好autoprefix
          //       ]
          //     }
          //   }
          // }
        ]
      },
      {
        test: /\\.less$/,//处理less文件
        use: [
          "style-loader",
          "css-loader",
          "less-loader"
        ]
      }
    ]
  }
}
```

### 打包图片、字体

**认识asset module type**

在webpack5之前，加载这些资源我们需要使用一些loader，比如raw-loader 、url-loader、file- loader；

在webpack5开始，我们可以直接使用资源模块类型（asset module type），来替代上面的这些loader；

资源模块类型(asset module type)，通过添加4 种新的模块类型，来替换所有这些loader：

asset/resource 发送一个单独的文件并导出URL。之前通过使用file-loader 实现；

asset/inline 导出一个资源的data URI。之前通过使用url-loader 实现；

asset/source 导出资源的源代码。之前通过使用raw-loader 实现；

asset 在导出一个data URI 和发送一个单独的文件之间自动选择。之前通过使用url-loader，并且配置资源体积限制实现；

```jsx
//处理图片
{
        test: /\\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]"//处理打包后文件路径和文件名
				
        },
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024//单位byte 限制图片大小100kb以下的图片就打包处理
          }
        }
      },
//处理字体
{
        test: /\\.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]"
        }
      }
```

## Plugin

webpack相较于其它的打包工具强大，正是由于它的生态强大，插件功能丰富

官方有这样一段对Plugin的描述：

While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks like bundle optimization, asset management and injection of environment variables.

上面表达的含义翻译过来就是：

Loader是用于特定的模块类型进行转换；

Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等；

### CleanWebpackPlugin

功能：每次build构建后，先把之前构建的包自动删除，在重新打包生成.

安装:`npm install clean-webpack-plugin -D`

![[Pasted image 20220903085941.png]]

### HtmlWebpackPlugin

功能：对入口html文件进行打包处理

安装：`npm install html-webpack-plugin -D`

![[Pasted image 20220903085955.png]]

通过对入口html 文件打包后，会发现打包后的index.html自动添加来打包好的bundle.js文件

## Mode

[Mode配置选项](https://webpack.docschina.org/configuration/mode)可以告知webpack使用响应模式的内置优化：

默认值是production（什么都不设置的情况下）；

可选值有：'none' | 'development' | 'production'；



![[Pasted image 20220903090008.png]]


![[Pasted image 20220903090030.png]]

## 结合Babel

Babel是一个工具链，主要用于旧浏览器或者环境中将ECMAScript 2015+代码转换为向后兼容版本的JavaScript；

功能包括：语法转换、源代码转换等；

babel本身可以作为一个独立的工具（和postcss一样），不和webpack等构建工具配置来单独使用。

如果我们希望在命令行尝试使用babel，需要安装如下库：

@babel/core：babel的核心代码，必须安装；

@babel/cli：可以让我们在命令行使用babel；

### 命令行使用

`npm install @babel/cli @babel/core -D`

使用Babel通常需要使用很多插件进行转换转换的内容过多，一个个设置是比较麻烦的，我们可以使用预设（preset）：

安装@babel/preset-env预设：

`npm install @babel/preset-env -D`

配置babel.config.js

```jsx
//babel.config.js
module.exports = {
  presets: [
    "@babel/preset-env"
  ]
}
```

```jsx
//webpack.config.js
//其它省略
{
        test: /\\.js$/,
        loader: "babel-loader"
      }
```

## webpack-dev-server

当文件发生变化时，我们希望可以自动的完成 编译 和 展示。

安装webpack-dev-server

`npm install webpack-dev-server -D`

```jsx
module.exports = {
  target: "web",
  mode: "development",
  devtool: "source-map",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "js/bundle.js",
  },
  devServer: {
   
    contentBase: "./public", // 修改配置文件，告知dev server，从什么位置查找文件
    hot: true,//开启模块热替换(HMR)
    host: "0.0.0.0",//默认值为localhost(127.0.0.1)
    port: 8000,//默认8080
    open: true,//是否打开浏览器
    // compress: true,//是否为静态文件开启gzip compression 默认false
    proxy: {
      "/api": {//将服务器地址代理为/api
        target: "<http://localhost:8888>",
        pathRewrite: {
          "^/api": ""
        },//如果没有pathRewrite 将会解析为http://localhost:8888/api/api/**
        secure: false,//默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false
        changeOrigin: true//它表示是否更新代理后请求的headers中host地址；
      }
    }
  },
//其它省略
}
```

## resolve

resolve 用于设置模块如何解析，常用配置如下：

-   alias：配置别名，简化模块引入；
-   extensions：在引入模块时可不带后缀；
-   symlinks：用于配置 npm link 是否生效，禁用可提升编译速度。

配置示例如下：
```jsx
module.exports = {
    resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }，
		symlinks:false
  },
}
```