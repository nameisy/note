# react 中的核心概念

### 一. 虚拟 DOM 的了解

1. DOM 的本质是什么？

   `浏览器中的概念，用 js 对象表示页面上的元素，并提供了操作 DOM 的 api`

2. React 中虚拟 DOM

   `用 js 对象模拟页面中的 DOM 和 DOM 嵌套`

3. 为什么要使用虚拟 DOM

   `为了实现页面中 DOM 的高效更新`

### 二. DOM 树的概念（一个网页的呈现过程）

+ 浏览器请求服务器获取页面的 HTML 代码
+ 浏览器在内存中解析 DOM 结构，并在浏览器内存中渲染出一颗 DOM 树
+ 浏览器将 DOM 树呈现到页面上

### 三. 如何虚拟一个 DOM 元素

ps:

```html
<div id="boxwrap" name="boxwrap">
  <span class="box">哈哈</span>
</div>
```

使用 js 对象的形式模拟页面 DOM 结构，通过对比判断实现高效的更新

```javascript
var div = [
	tarName : "div",
	attrs : {
		id : "boxwrap",
		name : "boxwrap"
	},
	childrens : [
		{
			tarName : "span",
			attrs : {
				class : "box"
			},
			childrens : [
				"哈哈"
			]
		}
	]
]
```

本质： 用 js 对象来模拟 DOM 元素和嵌套关系

目的：就是为了实现页面元素的高效更新

总结：虚拟 DOM 就是用 JS 对象的形式来模拟页面上的 DOM 和 DOM 之间的嵌套关系（虚拟 DOM 就是以 JS 对象的形式存在的）

### 四. diff 算法

1. 目的
   计算出新旧两颗 DOM 树之间的变化的部分，并只针对改部分进行原生 DOM 操作而非渲染整个页。

2. 原理
   ![diff算法](https://user-gold-cdn.xitu.io/2018/4/18/162d4772f1f40c49?imageslim 'diff算法')

```
=> tree diff
  将树形结构安装层级分解，逐层比较
=> component diff
  在进行逐层对比（tree diff）时，对于每一层组件级别的对比
=> element diff
  在进行组件级别的对比的时候，如果有两个组件类型相同则需要进行元素级别的对比
```

# 使用 webpack 4.x 创建项目

### 一. 使用 webpack 构建项目

1. 运行 npm init -y 快速初始化项目（生成 package.json）

2. 在项目下创建几个文件夹

```html
dist   // 打包
src 
  |—index.html 
  |—index.js 
package.json
```

3. 安装 webpack webpack-cli (npm i webpack webpack-cli -D)

4. 配置 webpack(新建 webpack.config.js)

```html
module.exports = { 
  mode : "development" // 环境 开发 测试 
}
```

注： 在 webapck 4.x 中约定大于配置 约定默认的打包入口路径是 src 下的 index.js,所以在不规定入口的情况下默认是 index.js.

### 二. webpack-dev-server 的使用

1. 安装 （npm i webpack-dev-server -D）

2. 使用 （在 package.json 中）

```json
"scripts" : {
  "dev" : "webpack-dev-server"
}
```

3. 运行 （npm run dev）

```html
扩展： "dev" : "webpack-dev-server --open --port --host" --open //
运行时打开浏览器 默认打开默认浏览器 --port // 端口号 默认8080 --host // 域名
默认localhost
```

### 三. html-webpack-plugin 插件

作用: 运行时打开指定的入口文件

- 安装 （npm i html-webpack-plugin -D）
- 使用 （在 webpack.config.js 中配置）

```javascript
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
const htmlPlugin = new htmlWebpackPlugin({
  template: path.join(__dirname, './src/index.html'), // 路径
  filename: 'index.html'
})
module.exports = {
  mode: 'development',
  entry: path.join(__dirname, "./src/main.jsx"), // 入口
  output: {
    path: path.resolve(__dirname, "./dist"), // 出口
    filename: "js/bundle.js"
  },
  plugins: [htmlPlugin]
}
```

- 运行 （npm run dev）

```
  打开本地是 index.html
  html-webpack-plugin 自动引入 .js 文件
``` 
### 四. 其他插件
+ extract-text-webpack-plugin         `抽取CSS样式文件`
+ optimize-css-assets-webpack-plugin    `压缩CSS文件`
+ clean-webpack-plugin   `打包前删除以前打包的内容`

###  五. 抽离第三方包
```javascript
optimization: { //第三方库抽离
  splitChunks: {
	cacheGroups: {
	  commons: {
		test: /[\\/]node_modules[\\/]/,
		name: 'vendors',
		chunks: 'all'
	  }
	}
  }
},
```
### 六. webpack 基本配置
```javascript
const path = require("path");
const htmlWebpackPlugin = require("html-webpack-plugin"); //创建html入口文件  压缩html代码
const ExtractTextPliugin = require("extract-text-webpack-plugin") // 抽取CSS样式文件
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin"); // 压缩CSS文件
const TerserPlugin = require('terser-webpack-plugin'); // 代码压缩
const { CleanWebpackPlugin } = require('clean-webpack-plugin'); // 打包前删除以前打包的内容

module.exports = {
    mode: "development",
    entry: path.join(__dirname, "./src/main.jsx"), // 入口
    output: {
        path: path.resolve(__dirname, "./dist"), // 出口
        filename: "js/bundle.js"
    },
    optimization: { //第三方库抽离
        splitChunks: {
            cacheGroups: {
                commons: {
                    test: /[\\/]node_modules[\\/]/,
                    name: 'vendors',
                    chunks: 'all'
                }
            }
        },
        minimizer : [new TerserPlugin({ /* additional options here */ })],
    },
    plugins: [
        new htmlWebpackPlugin({ //创建html入口文件  压缩html代码
            template: path.join(__dirname, "./src/index.html"),
            filename: "index.html",
            minify : {
                collapseWhitespace : "true" 
            }
        }),
        new CleanWebpackPlugin(), // 打包前删除第三方包
        new ExtractTextPliugin("css/style.css"), // 抽离CSS
        new OptimizeCSSAssetsPlugin() // 压缩CSS
    ],
    module: {
        rules: [{ // 启用sass jsx
                test: /\.css$/,
                use: ExtractTextPliugin.extract({
                    fallback : "style-loader",
                    use : "css-loader",
                    publicPath : "../"
                })
            },
            {
                test: /\.scss$/,
                use: ExtractTextPliugin.extract({
                    fallback : "style-loader",
                    use : ["css-loader", "sass-loader"],
                    publicPath : "../"
                })
            },
            {
                test: /\.(png|gif|bmp|jpg)$/,
                use: "url-loader?limit=5000"
            },
            {
                test: /\.js|jsx$/,
                use: 'babel-loader',
                exclude: /node_modules/
            }
        ]
    }
}
```
# react基础

### 在项目中使用react
+ 安装react react-dom （yarn add react react-dom -D）
	- react : 专门用来创建组件和虚拟DOM
	- react-dom : 专门进行DOM操作，最主要的应用场景 reactDOM.reander()
+ 在index.html 中创建容器
	- `<div id="root"></div>`
+ 使用react创建
```javascript
import React from "react"
import ReactDOM from 'react-dom'
/*
  参数1：创建元素的类型
  参数2:  一个对象，表示当前DOM的属性
  参数3：子节点
*/
const str = React.createElement("h1",{name:"header"},"哈哈")
// 使用ReactDOM将虚拟DOM渲染到页面
/*
  参数1 ： 需要渲染的虚拟DOM
  参数2 ： 指定的页面容器
*/
ReactDOM.reander(str,document.getElementById("root))
```
# JSX语法的学习
###  react中使用JSX
- 安装babel插件
```
  运行 yarn add babel-core babel-loader babel-plugin-transform-runtime -D
  运行 yarn add babel-preset-env babel-preset-stage-0 -D
  运行 yarn add babel-preset-react -D
```
- 添加 .babelrc 配置文件
```
  {
    "presets": ["env", "stage-0", "react"],
    "plugins": ["transform-runtime"]
  }
```
- 在webpack.config.js中添加babel-loader配置项：
```
  module: { //要打包的第三方模块
    rules: [
      { test: /\.js|jsx$/, use: 'babel-loader', exclude: /node_modules/ }
    ]
  }
```
### JSX语法
+ jsx语法本质上还是以React.createElements的形式实现的，并没有把用户写的HTML渲染到页面上
+ jsx中书写JS代码使用 {}
+ 在编译的时候遇到 `"<"` 开头的就会把它当成HTML来编译，遇到`"{}"`就会当成JS代码来编译
+ {} 中可以写任何JS规范代码
```javascript
ReactDOM.render(
  <div>
    {isShow ? "显示" ： "隐藏"}
  </div>,document.getElementById("root")
)
```
+ jsx中 
```
  class ---> className
  for ---->htmlFor
```
+ jsx 最外层有一个跟元素包含
```javascript
  ReactDOM.reander(
    <div>
      {/* 必须有一个根元素 */}
    </div>,document.getElementById("root")
  )
```
+ jsx 中的注释放在{}
```javascript
  {
    // 这是注释
  }
```
+ jsx 中只用循环（map 方法）
```javascript
  var arr = ['1','2','3','4']
  ReactDOM.reander(
      <div>
          {
              arr.map((item,index) => {
                  return <p key={index}>{item}</p>
              })
          }
      </div>,document.getElementById("root")
  )
```

# 创建最基本的组件
+ 创建组件的两中方式
```javascript
// 使用function创建一个组件
function Header() {
	retrun <div>头部组件<div>
}
// 使用class创建一个组件
import react form "react"
class Header extends React.Component {
	render() {
		retrun <div>头部组件<div>
	}
}
```
+ 组件传值（`通过属性的方式传值 props`）
父组件
```javascript
	import React from "react"
	import ReactDOM from 'react-dom'
	import "./css/index.scss"
	// header 组件
	import Header from "./components/Header.js"


	ReactDOM.render(
		<div className="demo-wrap">
			<Header className="demo-header" />
		</div>
	,document.getElementById("root"))
```
子组件
```javascript
import React from "react"
// 使用classs的方式
class Header extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return <h1 className={this.props.className}>this is my react-demo header</h1>;
  }
}
export default Header

// 使用function的方式
function Header(props) {
	return <h1 className={props.className}>this is my react-demo header</h1>
}
export default Header
```


