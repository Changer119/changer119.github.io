---
layout: page
title: 欢迎进入烂笔头的世界
tagline: 轻轻地来，也轻松地走
---
{% include JB/setup %}

## 个人简介

这能说些啥呢？悲剧的80后，无房无车，纯屌丝。

这个博客是烂笔头尝试的第N个博客了，依旧不知道自己能够坚持多久。

## 文章列表

下面是以往的文章列表：

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

我kao，还有ToDo...




