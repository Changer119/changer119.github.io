---
layout: post
title: jekyll bootstrap主题安装
---

在安装bootsrap dinky主题后，本地运行jekyll是没问题的，但一上传到github.com后，就会报下面的错误。

>The page build failed with the following error:

>The submodule `_theme_packages/dinky` was not properly initialized with a `.gitmodules` file. For more information, see https://help.github.com/articles/page-build-failed-missing-submodule.

>If you have any questions you can contact us by replying to this email.

笔者也很有耐心的去看了上文中的链接，结果怎么试都不行，后来在网上也找了很多其他的教程，依然解决不了。直到看到下面这两篇文章（[文章1](http://theloverz.me/note/2013/12/06/fix-failure-on-github-pages-and-jekyll/)，[文章2](http://dsimidzija.github.io/programming/2014/02/15/jekyll-bootstrap-themes-and-github-pages/)），才解决了问题。

下面我介绍下我的步骤。

1，进入博客的主目录(我的是changer119.github.io)，在.gitignore文件中添加如下内容。如果主目录下没有.gitignore文件，自己新增一个即可。

	*.swp
	_site/*
	_theme_packages/*

以上内容表示，_site和 _theme_package两个目录都不加到git，也不上传到github。


2，在主目录下删除已有的_theme_packages内容。

	git rm -r --cached _theme_packages

3, 重新安装jekyll bootstrap的主题。[官方链接](http://jekyllbootstrap.com/usage/jekyll-theming.html#toc_3)

我使用的dinky主题，我在主目录下输入如下命令，碰到需要选择的时候，一直yes下去

	rake theme:install git="git://github.com/sodabrew/theme-dinky.git"
	

4，本地run一下jekyll，看看效果。在主目录下执行。

	
	jekyll server


5，查看效果，访问[http://localhost:4000](http://localhost:4000)

6，如果效果正常，就可以提交修改到github啦。

	
	git add *
	git commit -m 'your log'
	git push origin master
	


















