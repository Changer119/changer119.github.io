---
title: 如何给wordpress站点添加favicon图标【转载】
author: admin
layout: post
permalink: /?p=32
duoshuo_thread_id:
  - 1159122750187503625
categories:
  - 研磨技术
---
所谓favicon，即Favorites Icon的缩写，顾名思义，便是其可以让浏览器的收藏夹中除显示相应的标题外，还以图标的方式区别不同的网站。而且根据不同版本的浏览器，作用效果也会有差异。

为WordPress添加个性的favicon.ico，可以让网站个性标志显示在浏览器地址栏上和收藏夹中，加深访客对网站的印象。

&nbsp;

favicon.ico添加方法：

先挑一张喜欢的图片，也可自己PS制作。DIY成16×16大小，通过相关图片制作工具，将其转换为扩展名为.ico，文件名为favicon的图片，即favicon.ico

【补充】：现在网上有很多在线生成icon图标的网站，如<http://www.bitbug.net/>。只要你上传图片，并选择相应的icon的大小，就可以生成icon图标了。

方法一

1.把准备好的favicon.ico图标直接上传到WordPress所在的网站空间的根目录

2.保存更新文件文件，清除浏览器缓存即可以地址栏看到favicon图标

方法二

1.把准备好的favicon.ico图标上传到空间的某个目录（如根目录）

2.编辑主题文件header.php，在<head>和</head>之间添加以下代码：

<link rel=”shortcut icon” href=”favicon.ico” type=”image/x-icon” />  
<link rel=”Bookmark” href=”favicon.ico” />

【建议】：href中填绝对路径。如下：

<link rel=”shortcut icon” href=”http://changer119.cn/fcjiang/favicon.ico” type=”image/x-icon” />

<link rel=”bookmark” href=”http://changer119.cn/fcjiang/favicon.ico” />  
保存更新文件文件，清除浏览器缓存即实现效果

添加动态favicon图标的方法：

1.先挑一张喜欢的gif动态图片，调整成16×16大小，重新命名为favicon.gif

2.编辑WordPress主题文件header.php，在<head>和</head>之间添加以下代码：

<link rel=”icon” href=”favicon.gif” type=”image/gif” >  
3.保存更新文件，清除浏览器缓存即可以地址栏看到favicon动态图标效果