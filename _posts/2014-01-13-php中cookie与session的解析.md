---
title: php中cookie与session的解析
layout: post
categories:
  - 研磨技术
  - Web相关
---

以前在学java web开发时就大致了解了cookie和session，当时觉得它们的主要差别就是：“cookie存放在客户端，而session存储在服务器端”，其它的区别就记不太清楚了。今天，在学习php时又碰到了这一对兄弟。于是，简单的列一下两者的异同吧。

**cookie**

1，cookie的机制

cookie主要用来识别用户，当浏览器连接到服务器时，服务器通过setcookie( )函数将用户的相关信息（用户名、密码）写入并保存到浏览器中。在设置cookie时，可以通过setcookie( ) 函数指定cookie的有效期。

2，php中cookie的使用

a）服务器向浏览器写cookie

使用方法：setcookie(name, value, expire, path, domain);  setcookie() 函数必须位于 <html> 标签之前

b）读取浏览器中的cookie

使用方法：$_COOKIE['key']

c）服务器删除浏览器中的cookie

使用方法：setcookie()，将超期时间设置为过去的时间。

&nbsp;

**session**

****1，什么是会话，会话的存活周期？

当浏览器访问某个服务器时，浏览器与服务器之间就建立了一个会话。**同一个浏览器（chrome）的不同标签共用一个会话**。在保证chrome不关闭的情况下，重新开一个chrome，新开的chrome与服务器之间的会话还是之前的会话。当所有的chrome都关闭时，chrome与服务器之间的会话就断开了。

2，session的机制？

session就是服务器开辟出来的一个内存空间，该内存空间用来存储服务器与不同浏览器之间的会话信息，如电商网站上，session主要存储用户刚刚浏览过的商品信息等。每一个独立的浏览器都会对应一个session，因而当服务器的访问量很大时，服务器端的session也有成千上万个。session的个数一多，占用的空间就大，于是，合理控制session的存活时间就非常重要了。现在，**每个session的内容在本浏览器关掉之后就被清空了**。

3，php中对session的操作

a）开启session

在html标签之前，通过session_start( )函数开启session。

b）访问session

通过内置的session变量$_SESSION['key'] = value操作

c）清除session

session\_destroy( ) 或者 unset($\_SESSION['key']