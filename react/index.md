# react 中的核心概念

### 一. 虚拟 DOM 的了解

1. DOM 的本质是什么？
   浏览器中的概念，用 js 对象表示页面上的元素，并提供了操作 DOM 的 api

2. React 中虚拟 DOM
   用 js 对象模拟页面中的 DOM 和 DOM 嵌套

3. 为什么要使用虚拟 DOM
   为了实现页面中 DOM 的高效更新

### 二. DOM 树的概念（一个网页的呈现过程）

1. 浏览器请求服务器获取页面的 HTML 代码
2. 浏览器在内存中解析 DOM 结构，并在浏览器内存中渲染出一颗 DOM 树
3. 浏览器将 DOM 树呈现到页面上

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
=>element diff
	在进行组件级别的对比的时候，如果有两个组件类型相同则需要进行元素级别的对比
```

# 使用 webpack 4.x 创建项目

### 一. 使用 webpack 构建项目

1. 运行 npm init -y 快速初始化项目（生成 package.json）

2. 在项目下创建几个文件夹

```
dist   // 打包
src
    |—index.html
		|—index.js
package.json
```

3. 安装 webpack webpack-cli (npm i webpack webpack-cli -D)

4. 配置 webpack(新建 webpack.config.js)

```html
module.exports = { mode : "development" // 环境 开发 测试 }
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

1. 安装 （npm i html-webpack-plugin -D）
2. 使用 （在 webpack.config.js 中配置）

```javascript
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
const htmlPlugin = new htmlWebpackPlugin({
  template: path.join(__dirname, './src/index.html'), // 路径
  filename: 'index.html'
})
module.exports = {
  mode: 'development',
  plugins: [htmlPlugin]
}
```

3. 运行 （npm run dev）
   <1> 打开本地是 index.html
   <2> html-webpack-plugin 自动引入 .js 文件
