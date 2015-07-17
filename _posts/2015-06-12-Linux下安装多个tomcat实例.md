---
layout: post
title: Linux下安装多个tomcat实例
---

## 拷贝tomcat目录
```
$ cp /..../tomcat  /..../tomcat_new
```

## 修改配置

### 修改`/etc/profile`文件
```
# 在文件的末尾加上下面这一段
# added by fcjiang @2015-06-12 for tomcat-7-9999-test
CATALINA_3_BASE=/enniu/tomcat-7-9999-test
CATALINA_3_HOME=/enniu/tomcat-7-9999-test
TOMCAT_3_HOME=/enniu/tomcat-7-9999-test
export CATALINA_3_BASE CATALINA_3_HOME TOMCAT_3_HOME
```

### 激活`/etc/profile`修改内容
```
$ source /etc/profile
```

### 修改`.../tomcat_new/bin/catalina.sh`文件
```
# 在该文件的最前面添加一下内容
# added by fcjiang @2015-06-12 for tomcat-7-9999-test
export JAVA_HOME=$JAVA_HOME
export PATH=$PATH
export CLASSPATH=$CLASSPATH
export CATALINA_BASE=$CATALINA_3_BASE
export CATALINA_HOME=$CATALINA_3_HOME
```

### 修改tomcat_new中server.xml文件
文件路径`.../tomcat_new/conf/server.xml`

需要修改三个地方：

1，修改应用访问端口

```
<Connector port="9999（修改）" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"/>
```

2，修改AJP端口

```
<Connector port="9009（修改）" protocol="AJP/1.3" redirectPort="8443" />
```

3，修改tomcat关闭端口

```
<Server port="9005（修改）" shutdown="SHUTDOWN">
```

**至此，新加的tomcat的配置已经完成**


## 启动新的tomcat

```
$ .../tomcat_new/bin/catalina.sh start
```


