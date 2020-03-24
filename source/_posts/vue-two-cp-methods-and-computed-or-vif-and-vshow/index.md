---
title: Vue | 两对cp methods && computed || v-if && v-show
date: 2020-03-04 14:32:04
tags: Vue
order: 4
---

今天来看两对 cp，之所以把他们当 cp，是因为每次写其中之一时，总是要拿另一个做比较，这种关系在前端很常见。

基本上是看起来好像实现的功能点是一样的，但实际上每个都有其特殊的应用场景。彼此互为补充，缺了谁，都有点不大对劲。

今天来介绍的两对是：
- methods && computed
- v-if && v-show

---

先来看第一对，methods && computed 是创建 Vue 实例时可以选择的字段，和 el， data 是同一类的。

两者的关系有一种既生瑜何生亮的感觉。

在比较之前先看一个 computed 的个人问题。

为何计算属性写的时候是函数，用的时候当成属性【不加小括号】来操作？

假设接下来用到的 data 为：
```js
data: {
    firstName: 'cem',
    lastName: 'coe'
}
```

现在要在页面中展示完整的名字，可以使用的方式：
- 双大括号语法 {{firstName + lastName}} 语法过于繁琐
- methods
- computed

使用计算属性：
```js
computed: {
    fullName() {
        return this.firstName + this.lastName
    }
}
```

在 html 中使用时可能是这样的：
```html
<h1>{{fullName}}</h1>
```

为什么不加小括号呢？它不是函数吗？

这里其实是计算属性的简写，完整的写法：
```js
computed: {
    fullName: {
        set: function() {
        },
        get: function() {
            return this.firstName + this.lastName
        }
    }
}
```
计算属性本质上就是一个属性，并非函数，只是开发时一般不实现 set 方法。只是简写以后给人一种函数的感觉。

这也就解释了为何计算属性写的时候是函数，用的时候当成属性来操作。

那么接下进入正题，来看一下这对 cp 的异同。

-----------------------------------

这里做一个对比实验，methods && computed 在页面中均搞 3 次。

```html
<div id="app">
    {{fullName}}
    {{fullName}}
    {{fullName}}
    ---
    {{getFullName()}}
    {{getFullName()}}
    {{getFullName()}}
</div>
```

为了看到结果在它们身上埋点 console
```js
methods: {
    getFullName() {
        console.log("methods")
        return this.firstName + this.lastName
    }
},
computed: {
    fullName() {
        console.log("computed")
        return this.firstName + this.lastName
    }
}
```

![methods && computed](https://upload-images.jianshu.io/upload_images/3750332-16a098d1863f8d87.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从结果来看，同样请求三次数据的情况下， methods 执行3次，而 computed 仅仅执行1次。

在 firstName 和 lastName 不改改变时，computed 会使用缓存的结果，效率更高，更加节省资源。

其实两者之中还有一个第三者，唤为 watch，他们之间错综复杂的关系找时间再写。

---

来看第二对 v-if && v-show,，两者用于条件渲染。

通过一个对比实验来看一看 v-if && v-show 的区别，为了看清结果，给他们埋下 id：
```html
<h1 v-if="isShow" id="if">cemcoe</h1>
<h1 v-show="isShow" id="show">cemcoe</h1>
```

![v-if && v-show](https://upload-images.jianshu.io/upload_images/3750332-3c84bee2f0bb3237.gif?imageMogr2/auto-orient/strip)

当 isShow 的值被改成 false 时，两个都消失。但他们不一样，通过观察埋下的 id 可以发现：v-if 直接从 DOM 中删掉，v-show 增加了行内样式 display:none，仅仅是视觉不可见。

v-show 更像是一件隐身斗篷。元素仍然存在，只是不可见。

在 CSS 中有类似的问题：display:none 和 visibility:hidden 的异同。又给自己挖了一个坑，有空填。

开发中 v-if 使用较多，向服务器请求数据仅仅加载有权限的内容，如果没有权限就不能将数据添加到 DOM 中。

> 这里是连载 [化学小子的前端实验册](https://www.jianshu.com/nb/40206736) 的第 18 篇，欢迎关注。

![pixiv|えすない|78926374](https://upload-images.jianshu.io/upload_images/3750332-ada6474fa1057fdb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[图源](https://www.pixiv.net/artworks/78926374)






