---
title: Vue | 如何将数据插入到属性中
date: 2020-03-03 14:32:04
tags: Vue
order: 3
---

上一篇 [Vue | 插值操作 | 如何将数据弄到DOM中](https://www.jianshu.com/p/86a4b7033567) 讲的是网页上看得见的内容的插入操作。这篇就看一看网页中那些看不见的地方如何进行插入。

这里看不见的地方的术语名为属性。

涉及的内容
- src alt class
- class 对象绑定写法


在真实的开发中 src 什么的肯定都是不能写死的，都是从服务器上请求来的，可能是一个 json 数据，包含很多的 url 。

这是要对它们进行绑定就要用到 `v-bind` 指令了。

几个例子：

> v-bind:href
v-binf:src

这里假设从服务器请求到的数据如下：
```
data: {
        link: "https://www.jianshu.com/u/e20f22d3e8d3"
    }
```

来尝试绑定一下，是这样的：
```html
<a v-bind:href="link">cemcoe</a>
```

经过上面的一番操作，代码会被解析成如下的 html，即一个 a 标签
> <a href="https://www.jianshu.com/u/e20f22d3e8d3">cemcoe</a>


---

鉴于上边的绑定语法在开发中经常使用，Vue 为其定义了语法糖：直接冒号，开发使用
> v-bind:href === :href
v-bind:src === :src


----------

class 同 src alt 一样，本身也是一个属性，那么如果要对其进行绑定的话，上面的方法自然也是可以勉强试一试的。

仿照一般属性的绑定方法，class 绑定如下：
```html
<h1 v-bind:class="myClass">{{name}}</h1>

// ---data---
data: {
        name: "cemcoe",
        myClass: "green"
    }
```
这样当然可以将 green 绑定到 class 上，但如果这样干，为啥要多此一举，直接写在 class 里岂不是更香。基于以上认知，工作中并不常用这种方式来绑定 class，class 是一个特殊的属性，这里会对它特殊照顾。

与其它属性不同的是，class 还有对象写法和数组写法【也不常用】。
下面来看一看对象的写法。


---


动态绑定 class 的对象写法
```
<h1 v-bind:class="{green: isGreen, border: isBorder}">{{name}}</h1>
// ----data----
data: {
        name: "cemcoe",
        isGreen: true,
        isBorder: false
    }
```
通过布尔值来决定是否添加对应的 class，与一般属性的绑定相比，这种绑定方式可以动态地进行调整，这非常关键，因为在开发中不断地变换 class 是一个经常要用到的操作。

----------


虽然上面的对象写法和一般的绑定方式相比增加了动态性，但 `{green: isGreen, border: isBorder}` 这种格式放在 html 中，从视觉上有点长；从可维护方面上看，也没有将逻辑和 html 分离。

前端强调结构，样式和行为彼此分离，彼此不要有太多的杂糅。

这时就可以考虑更加可维护的方式：
```
<h1 v-bind:class="getClasses()">{{name}}</h1>
// ---data---
data: {
        name: "cemcoe",
        isGreen: true,
        isBorder: false
    },
methods: {
    getClasses() {
        return {green: this.isGreen, border: this.isBorder}
    }
}
```
如上，用了一波 methods 字段，这个字段是干嘛的，有空填。

这里将 class 切换的逻辑从 html 中抽离到了 js 中的 methods 中的字段，这样就做到了逻辑和结构相分离的效果。

在接到需求变更时，找到 js 代码进行修改就好，可以快速进行定位。

---
数组语法绑定 class 用的较少，不展开，感兴趣的可直达官网。

来个总结：
绑定属性采用 `v-bind` 指令，有颗语法糖是 `:`，Vue 对 class 的绑定开了一个门，可以使用对象写法，借助布尔值来判断。为了可维护性可以将判断逻辑抽离到 js 中，尽量保持结构，样式和行为相分离的效果。
  

> 这里是连载 [化学小子的前端实验册](https://www.jianshu.com/nb/40206736) 的第 17 篇，欢迎关注。




