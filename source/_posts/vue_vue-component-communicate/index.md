---
title: Vue | 组件化 | 组件通信
date: 2020-03-08 14:32:04
tags: Vue
order: 8
---
通信问题就是如何沟通，如何唠嗑的问题。
<!-- more -->

在前后端分离时数据传输的流向是这样子的：
- 父组件向服务器接口请求数据
- 子组件向父组件请求本组件数据
- 父组件监听子组件发生的事情并做响应的处理



不如来看以下细胞信号传输：细胞如何相互沟通。
> 传送和接收这些信息所需的网络很复杂。这是由一支信使分子军队组成，跨细胞和在细胞之间传输信号（传讯分子）。它们寻找接收信号的目标（受体），最后，信使和受体相互作用，使细胞产生了最终的结果（细胞对原始信号做出回应）。
https://askthescientists.com/zh-hans/qa/what-is-cell-signaling/

也就是说，通信是需要两方共同参与，谁发出？传给谁？

---


瞎掰结束，来看 Vue 中的组件通信。

最上层的组件从服务器请求数据，每个子组件向父组件请求自己所需的数据。

不要跨级请求数据，浪费数据请求。


关键指令：
- 父传子 `props`
- 子传父 自定义事件 `$emit`


来个 DNA 检测确认父子关系，传信息前肯定是要确定对方身份的，将信息传给敌方就尴尬了。

> 陈佩斯：（从台侧上）队长——别开抢！是我！（然后像电线竿立着）
朱时茂：哦——你小子。说话！是你把敌人引到这儿来的？（陈佩斯不做声）
朱时茂：你说话呀！
陈佩斯：你们家电线杆子能说话吗！
朱时茂：你这个……台词还是要说的嘛！
陈佩斯：让说就说呗。
朱时茂：你千万记得啊！只要我掏出枪来一抬手——
陈佩斯：怎么着？
朱时茂：你就倒下。
陈佩斯：为什么？
朱时茂：这表示我的枪法准啊！


来理一理下面的组件关系:
```html
<template id="cpn">
    <div>
        {{goods}}
    </div>
</template>
//----data----
const cpn = {
    template: "#cpn"
}
const app = new Vue({
    el: "#app",
    data: {
        goods: ['腊肉', "肥肠", "咸鸭蛋"]
    },
    components: {
        cpn
    }
})
```
app 是 cpn 的父组件，现在的需求是儿子 cpn 向爸爸 app 要物资，即 goods 列表。

怎么要？打电话还是发短信。

怎么传，电报？

传递信息需要双发的共同努力，父组件发送，子组件接收。

```html
<div id="app">
    <cpn v-bind:cgoods="goods"></cpn>
</div>
<template id="cpn">
    <div>
        <h1>{{cgoods}}</h1>
    </div>
</template>
<script>
    const cpn = {
        template: "#cpn",
        props: ["cgoods"],
    }
    const app = new Vue({
        el: "#app",
        data: {
            goods: ['腊肉', "肥肠", "咸鸭蛋"]
        },
        components: {
            cpn
        }
    })
</script>
```

通过 `<cpn v-bind:cgoods="goods"></cpn>` 将父子组件连接，子组件通过 props 接收父组件传来的数据。


下面来理智分析（瞎掰）一波。

过年回家，家里 准备了很多的过年的物资` data·`，儿子回到了老父亲的家里 `app` 过年，
临走时父亲从自己的背包 `goods` 中拿出很多儿子想吃的塞到了儿子的背包 `cgoods `里。

父子分别，儿子到自己的家 `cpn` 后，从背包里拿出老父亲给自己塞的腊肉咸鸭蛋，开始准备今天的晚饭 `{{cgoods}}`。

这真是一个温情的故事。

父子传递信息大概就是这么个意思。

至于如何处理数据（腊肉啥的）这里就不搞了。

----------

逻辑理通后来看一波零碎的知识。

这里有点 TypeScript 的影子，类型限制和默认值，来波 TypeScript 代码，TypeScript 搞起来。

```js
props: {
    cgoods: {
        type: String,
        default: "烤肠",
        required: true
        // 必须传值
    }
}
```
也就是说可以对 props 的接收数据可以进行类型限制，确定默认值等。

-----------------------
反过来看一下子传父的过程。


子组件发生事件，父组件要知道，比如人生大事结婚订婚等。

家人是坚强后盾。

先让自己知道自己发生了什么事情？？？
```html
<div id="app">
    <cpn></cpn>
</div>

<template id="cpn">
    <div>
        <button 
        v-for="item in categories"
        @click="btnClick(item)">{{item.name}}</button> 
    </div>
</template>

<script>
  const cpn = {
      template: "#cpn",
      data() {
          return {
              categories: [
                  {id: 001, name: "评论"},
                  {id: 002, name: "简信"},
                  {id: 003, name: "投稿请求"},
                  {id: 004, name: "喜欢和赞"}
              ]
          }
      },
      methods: {
          btnClick(item) {
              console.log(item)
          }
      }
  }
  const app = new Vue({
      el: "#app",
      data: {
          
      },
      components: {
          cpn
      }
  })
</script>
```

首先儿子要先知道自己目前的状况，发生了什么事情。这里就要用到事件监听。要用到 [@click && method](https://www.jianshu.com/p/b2aa1eeb30c7) 一对基友。


问题来了，如何将自己的近况及时告诉自己的父母，父母知道儿子的近况以后做出相应的回应，或放心，或担心，或嘱咐添加衣物按时吃饭，不要累着。


```html
<div id="app">
    <cpn v-on:item-click="cpnClick"></cpn>
    <!-- 4. 得到孩子的近况 -->
</div>

<template id="cpn">
    <div>
        <button 
        v-for="item in categories"
        @click="btnClick(item)">{{item.name}}</button> 
        <!-- 1. 时刻关注自己的状况 -->
    </div>
</template>

<script>
    const cpn = {
        template: "#cpn",
        data() {
            return {
                categories: [
                    {id: 001, name: "饿了"},
                    {id: 002, name: "没钱了"},
                    {id: 003, name: "有女朋友了"},
                    {id: 004, name: "结婚了"}
                ]
            }
        },
        methods: {
            btnClick(item) {
                console.log(item)
                // 2. 我发生了什么事情
                this.$emit('item-click', item)
                // 3. 告诉父母我发生了什么事情
            }
        }
    }
    const app = new Vue({
        el: "#app",
        methods: {
            cpnClick(item) {
                console.log("老子知道你小子的事情了 | ", item.name)
                // 5. 对孩子的近况做出响应
            }
        },
        components: {
            cpn
        }
    })
</script>
```

将在子组件监听到的事件通过 `$emit` 传出去，通过 `<cpn v-on:item-click="cpnClick"></cpn>` 将两者连接，父组件在自己的 `methods` 中实现对应的响应逻辑。


捋一捋大概是这样：
- 时刻关注自己的状况
- 我发生了什么事情
- 告诉父母我发生了什么事情
- 得到孩子的近况
- 对孩子的近况做出相应


来一波总结：

- 父传子 `props`
- 子传父 自定义事件 `$emit`


