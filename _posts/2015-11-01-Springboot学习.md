---
layout: post
title: Springboot学习
---

## springboot打包的jar文件远程调试

```
java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=55555,suspend=n -jar target/myproject-0.0.1-SNAPSHOT.jar
```

对于几个参数做一些说明：

- address=55555 表示服务端开启了55555的调试端口，等待IDE连接到该端口进行远程调试。这个端口与tomcat应用的端口8080是不同的。
- suspend=n/y  如果是y，启动服务端应用时，应用会停在那，知道IDE连接上后才开始执行。如果n，则启动后服务端可以正常运行，直到有IDE连接上后才会进行调试状态。


