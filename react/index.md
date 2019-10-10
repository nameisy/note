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

```
<div id="boxwrap" name="boxwrap">
	<span class="box">哈哈</span>
</div>
```

使用 js 对象的形式模拟页面 DOM 结构，通过对比判断实现高效的更新

```
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
