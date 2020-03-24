---
title: Hexo | 如何搞一个专属的 Hexo 主题 | 文章篇
date: 2020-03-23 
tags: Hexo
order: 0
---
如何使用 Hexo 搭建博客不是本文重点，本文主要搞一个专属的个人博客主题。
<!-- more -->
点我去 [预览](https://cemcoe.com/blog/chemer-fe-test/index.html) 主题。

今天来看一看文章详情的模板如何写？

先进行功能点拆分：
- 文章元信息如标题，时间，tags 等的展示问题
- 如何拿到文章内容的主体数据
- 如何处理文章里出现的代码，图片等特殊资源

---

现在看一下使用 `forEach` 遍历 tags，来看一下 tag 的数据结构。
```md
_Document {
  name: 'Hexo',
  _id: 'ck82sbm4z000fkoie38def1hh',
  slug: [Getter],
  path: [Getter],
  permalink: [Getter],
  posts: [Getter],
  length: [Getter]
}
```

嗯，我们需要的数据是 `tag.name`，开始遍历。
```html
<% page.tags.data.forEach(function (tag) { %>
<span class="tags"><%- tag.name %></span>
<% }) %>
```

拿到文章主体数据：
```html
<%- page.content %>
```

右侧目录排序功能点
```html
<ul>
    <% site.posts.data.forEach( function(post){ %>
    <li style="order: <%- post.order %>;"><a
      href="<%- url_for(post.path) %>"><%- post.order %>---<%- post.title %></a></li>
    <% }) %>
</ul>
```
在这里使用 flex 布局中的 order 进行正序排列。

在这里有个坑，在 `site.posts.data` 中的数组是按照文件在系统中的排序，而不是创建时间，就很迷，看了好久不知道排序的依据是啥。

---


接下来看一看如何处理文章里出现的代码，图片等特殊资源。
被 ` 包裹会被渲染成 code 标签，双大括号会报错。


下面列举一些 md 搭配 html 标签的例子用于添加样式：
```md
``     ===> code
>      ===> blockquote
`````` ===> figure.[hihlight plain]

```

<!-- 小小目录功能涉及的点
数组排序
遮罩问题
滚动条的使用 -->
```css
pre {
  overflow-x: auto;
}
```
防止代码撑开页面

代码高亮 https://prismjs.com/download.html

整体上这个 Hexo 主题大致就搞好了，还剩一些边边角角。