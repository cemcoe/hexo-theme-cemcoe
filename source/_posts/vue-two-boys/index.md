---
title: Vue | 事件监听的两个小伙伴
date: 2020-03-05 14:32:04
tags: Vue
order: 5
---

介绍完 [两对cp](https://www.jianshu.com/p/12b8728d19cf) 后，今天来看看一对基友的故事。
<!-- more -->

在 Vue 中有个名为 methods 的字段，这货通常是要搭配 v-on 来一起使用。

两个搞到一块可以实现术语唤为 事件监听 的东西。

---

来波回忆杀，看看原生的 JavaScript 是如何进行事件监听的？
```
const demo = document.querySelector('#demo')
demo.onclick = () => {
    console.log("mama，我被人点了。嘤嘤嘤。")
}
```

看上去不大高端，唤 addEventListener 来看看。
```
const demo = document.querySelector('#demo')
addEventListener('click', () => {
    console.log("mama，我被人点了。嘤嘤嘤。")
})
```
emm，有点样子了。

通过上面的一波回忆，要来监听事件可以提取下列步骤：
- 找到要监听谁？ || 监听对象
- 对监听到的事件做何反应？ || 响应

简言之，事件监听就是：某元素发生了什么事情，我要做出相应的反应证明我不是吃干饭的。


---

来看看在 Vue 的地盘事件监听是怎么的一个玩法？
```html
<h1>{{cemcoe}}</h1>
<button v-on:click="qinqin">qinqin</button>
<button v-on:click="baobao">baobao</button>
```
v-on 监听事件，起到的是找到监听元素的作用；methods 提供具体的应对策略，被打要不要被打回去，还是以理服人。
```
const demo = document.querySelector('#demo')
// 老兄，找元素的事情交给我 v-on
addEventListener('click', () => {
// 回调函数就交给 methods 好了
    console.log("mama，我被人点了。嘤嘤嘤。")
})
```
这个东西还有语法糖 v-on:click === @click

用上语法糖，有点简洁的写法：
```html
<h1>{{cemcoe}}</h1>
<button @click="qinqin">qinqin</button>
<button @click="baobao">baobao</button>
```
具体的 qinqin 和 baobao 函数这里就不实现了，当伪代码看。

----

通过上面的一番瞎掰，可以知道 methods 中其实是原先 addEventListener 的回调函数，函数肯定要有参数才能发挥它的魅力。

关于参数的问题：
- 如果方法(函数)不需要参数，那方法后的 () 可以不写
        · 若方法本身有参数，会默认将原生事件 event 传进去
- 同时传某个参数和 event 时，通过 $event 传进去

这个关于 event 的一些东西有空再写。 

---

下面来看一些例子，正常的逻辑
```html
<button @click="fn(‘cemcoe’)">点我</button>

// ---methods---
fn (a) { console.log(a) }
```
这是乖孩子，方法有一个函数，那么在使用时就规规矩矩地传给它一个参数，合作愉快。

---

那遇到就是叛逆，就是要彰显与众不同，就是不传参数且不加括号，会怎么样？下面是点三次按钮的结果。
```html
<button @click="fn">点我</button>

// ---methods---
fn (a) { console.log(a) }
```
![MouseEvent](https://upload-images.jianshu.io/upload_images/3750332-3fa3a06c1f2232bb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 在事件定义时，写方法时省略小括号，但是方法本身需要参数，这时Vue会默认将浏览器生产的 event 事件对象作为参数传入方法中。



下面来看一个方法定义时，需要 event 对象，同时需要其他参数的例子：

```
<button @click="fn('cemcoe', $event)">点我</button>
//---methods---
 fn (name, event) {
        console.log(name, event)
}
// cemcoe MouseEvent
```
大概就是这个样子，传参时使用 $event 传递事件本身。

--------------------------------------------

原生的 事件监听 会有事件冒泡等问题，Vue 为这些问题提供了解决方案，术语叫做修饰符。换成大白话就是加点加状态。

```html
<div @click="divClick">
    <button @click="btnClick">
        button
    </button>
</div>

//---methods---
divClick() {
    console.log("div")
},
btnClick() {
    console.log("btn")
}
```
当点击 button 时会发现控制台会打印 div 和 btn，也就是说，两个点击都被激活，这就是事件冒泡。

阻止冒泡，Vue 可以使用修饰符 `.stop`
```html
<div @click.stop="divClick">
    <button @click="btnClick">
        button
    </button>
</div>

//---methods---
divClick() {
    console.log("div")
},
btnClick() {
    console.log("btn")
}
```
修饰符的使用方法如上例 ，直接在事件监听后加修饰符。

---

这里有一些其他修饰符：
- stop 阻止冒泡 event.stopPropagation()
- prevent 阻止默认事件 event.preventDefault()
- keyCode 特定按键触发
- once 执行一次回调

关于 v-on 和 methods 的故事告一段落。

> 这里是连载 [化学小子的前端实验册](https://www.jianshu.com/nb/40206736) 的第 19 篇，欢迎关注。










