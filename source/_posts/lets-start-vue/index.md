---
title: lets-start-vue
date: 2020-03-27 18:12:37
tags: Vue
order: 1
---

<!-- more -->
Vue 的介绍，优缺点等概念这里就不瞎掰了。若想了解请直达[官网](https://cn.vuejs.org/index.html)。

在 [CSS 效果 | 轮播图](https://www.jianshu.com/p/9817643f8a6b) 和 [CSS 效果 | tab 选项卡](https://www.jianshu.com/p/fdd58caea73f) 中两个效果中可以提取出一个常见的问题，那就是如何进行元素的双向绑定，原文并没有实现，这是一个普遍的需求点。

tab 变换时内容区域要跟着变，那么内容如果发生了切换而 tab 却还停留在原来的位置，肯定是不符合需求的。同样的轮播图和小白点也是要双向绑定的，那么这里其实就存在着重复的需求。Vue 其实是支持元素的双向绑定的。

原生的 js 有点难搞，这时可以捋一捋 Vue 的知识点了。

国际传统 Hello World 开始。

在 Vue 中写一个Hello World 是要这样的：
- 引入 Vue.js || 本质上就是引入一个外部的 js 文件
- html 中定义要挂载的元素 || 看一看哪个幸运儿可以被选中，独得皇上恩宠
- 使用 new Vue() 创建实例 || 该实例接受一个对象作为参数
- 填充字段，接收的字段包括：el，data
- 使用双大括号语法 || 显示


在 HTML 中引入 Vue
```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

HTML 文档结构
```html
<div id="app">
    {{message}}
</div>
```
HTML 文档结构很简单，一个 id 名为 app 的 div 包着一个双大括号。
这里的双大括号是个什么鬼东西嘞？挖了一个坑，有空填。。


下面是 js  示例，同时也是核心代码：
```js
// function Vue() {

// }
// new Vue 做了什么？这个要看一下源码。
// 接受一个对象参数
const app = new Vue({
    el: '#app', // 用于挂载要管理的元素
    data: {
        // 定义数据
        message: "Hello cemcoe"
    }
})
```
效果就是页面上会展示 “Hello cemcoe”。

上面的编程范式是声明式编程，至于什么是声明式编程，简单说就是：点外卖，自己做饭可能要去买菜，接着洗菜，下一步切菜。。。点外卖就不需要考虑各个步骤了，直接点击要的下单，我要这个，还有这个，至于怎么做完全不用考虑。


---


想一下，如果不使用框架要实现上面的 Hello World 是一个什么流程。

与声明式编程相对，这里其实是命令式编程，一步一步怎么做，每一步都要精确。
1. 首先肯定要使用诸如 querySelector() 等方法选择节点
2. 而后创建文本节点，并为其赋值
3. 将创建的文本节点挂载到选择的节点


----

下面来看一下如何展示一个列表。

省略固定不变的代码，在 data 中定义一个书籍列表
```js
books: [
        "数学",
        "物理",
        "生物",
        "化学"
]
```

下面是展示，有了 Hello World 的经验，对于列表比较容易想到的方法是：
```html
<ul>
    <li>books[0]</li>
    <li>books[1]</li>
    <li>books[2]</li>
</ul>
```

上面的方法可以吗，当然可以，就是有点憨憨，就好像老师让算从1到100的整数和时，真就从1开始加到100。



比较简洁的是下面的方式，Vue 已经帮我们搞好了，真贴心。

---

```html
<ul>
    <li v-for="item in books">{{item}}</li>
</ul>
```

不出意外会展示一个列表 。

技术点在于 `v-for="item in books"`，可以大致猜一下，这是遍历了一下数组并将值赋给每个 item，并将 item 搞到页面中
```js
// 仅供理解
for(let i = 0; i < books.length; i++) {
    console.log(books[i])
}
```
在 Vue 中类似于 `v-for` 这种命名规则的还有 `v-if` 等，他们的术语是指令。

到此就是 Vue 的入手笔记了。


