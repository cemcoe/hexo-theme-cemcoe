---
title: Vue | 组件化 | 组件个性化定制
date: 2020-03-09
tags: Vue
order: 9
---
组件也要非主流，现在提倡个性化定制。
<!-- more -->
生活中个性化需要两步，第一步客户挑选要个性化的物件，如T恤，U盘。
第二步商家对物件上预留的可以个性化的地方进行定制。

组件个性化如何进行，首先找到要个性化的物件，这里要拿到整个的组件，而后对组件进行个性化设置

---

先来看关键词
- 父子 `$children` || `$refs`【常用】
- 子父 `$parent`

`$children` 得到数组，通过索引得到特定组件。

`$refs` 得到对象 配合  `ref` 使用，通过 `ref` 的值得到组件。


```html
<cpn ref="demo"></cpn>
//---
this.$refs.demo
// 得到cpn组件
```

大家都是有身份的人，`$refs` 使用的较多，直接指定要定制的部位，而不是按索引。

生活中的个性化一般是对要定制的地方进行留白，在 Vue 中有个关键词 `slot` 名为插槽。

---
个性化，非主流
slot 插槽
- 具名插槽
- 编译作用域

自定义组件，暴漏一定的接口，组件可以动态化。

求同存异 函数的参数 而不仅仅是表达式。
```html
<div id="app">
    <cpn>
        <button>函数实参，替换函数形参</button>
    </cpn>
    <cpn></cpn>
</div>
<template id="cpn">
    <div>
        <h1>cemcoe</h1>
        <slot></slot>
        <!-- 函数() 形参 -->  
    </div>
</template>
<script>
    const cpn = {
        template: "#cpn"
    }
    const app = new Vue({
        el: "#app",
        components: {
            cpn
        }
    })
</script>
```

还可以默认值
```html
<slot><button>我是默认值</button></slot>
```


------
给插槽名字 对象的属性名 数组 `refs` 是不是相通的？？
多个插槽 插哪个？ `name + slot`
```html
<div id="app">
    <cpn>
        <span slot="sa">我把老大绑了</span>
    </cpn>
</div>
<template id="cpn">
    <div>
        <slot name="sa"><button>我是老大</button></slot>
        <slot name="xi"><button>我是老二</button></slot>
        <slot name="si"><button>我是老三</button></slot>
    </div>
</template>
```

来波总结：

拿特定的组件和定义插槽都借助了 `key-value` 的数据结构。
- 拿组件是 `refs: ref`
- 配置组件插槽是`slot: name`

