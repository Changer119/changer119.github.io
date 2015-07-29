---
layout: post
title: Intellij远程调试Tomcat中的应用
categories: 研磨技术
---

一个web应用部署到tomcat上去了，如何通过Intellij来调试呢？

你只需要两步操作，如下：

## 配置tomcat支持远程调试

tomcat默认是没有开放远程调试端口的。你需要找到%CATALINA_HOME%/bin/catalina.sh文件，在这个文件的最上面加上下面配置：

	export JAVA_OPTS='-agentlib:jdwp=transport=dt_socket,address=53013,suspend=n,server=y'

上面的配置会让tomcat在启动后监听端口**53013**，这个端口你可以自行改动。如果Intellij对这个端口发起请求时，就可以连接上tomcat并进行远程调试。

## 配置Intellij的远程调试

新增remote调试，配置tomcat在的服务器和监听端口，选择要调试的应用。

![](http://changer119.qiniudn.com/QQ20150729-1.png)

![新增remote调试](http://changer119.qiniudn.com/QQ20150729-2.png)

上图中端口应该为53013

---

以上配置完成后，就可以启动远程调试了。

步骤一：正常启动tomcat
	 
	 %CATALINA_HOME%/bin/startup.sh
	 // 或者
	 %CATALINA_HOME%/bin/catalina.sh start

步骤二，启动intellij的远程调试

![启动intellij的远程调试](http://changer119.qiniudn.com/QQ20150729-3.png)
	 
	

        









