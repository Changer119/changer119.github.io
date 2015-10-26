---
layout: post
title: java getResourceAsStream用法
---

通过getResourceAsStream方法可以获取到配置文件。

主要有两种形式：

### 通过Class类调用 

```
this.getClass.getResourceAsStream(filePath)
// 若是静态类，可以直接通过类名，如
Person.class.getResourceAsStream(filePath)
```

**filePath建议以“/”开头，表示从classpath目录下搜索**

filePath不以“/”开头，则表示从Person.class文件所在目录开始搜索

### 通过ClassLoader调用

```
this.getClass.getClassLoader.getResourceAsStream(filePath)
// 静态类的方式如下
Person.class.getClassLoader.getResourceAsStream(filePath)
```

**filePath不能以“/”开头，默认从classpath目录下搜索**


下面给一些实例

```
第一： 要加载的文件和.class文件在同一目录下，例如：com.x.y 下有类me.class ,同时有资源文件myfile.xml 

那么，应该有如下代码： 

me.class.getResourceAsStream("myfile.xml"); 

第二：在me.class目录的子目录下，例如：com.x.y 下有类me.class ,同时在 com.x.y.file 目录下有资源文件myfile.xml 

那么，应该有如下代码： 

me.class.getResourceAsStream("file/myfile.xml"); 

第三：不在me.class目录下，也不在子目录下，例如：com.x.y 下有类me.class ,同时在 com.x.file 目录下有资源文件myfile.xml 

那么，应该有如下代码： 

me.class.getResourceAsStream("/com/x/file/myfile.xml"); 

总结一下，可能只是两种写法 

第一：前面有 “   / ” 

“ / ”代表了工程的根目录，例如工程名叫做myproject，“ / ”代表了myproject 

me.class.getResourceAsStream("/com/x/file/myfile.xml"); 

第二：前面没有 “   / ” 

代表当前类的目录 

me.class.getResourceAsStream("myfile.xml"); 

me.class.getResourceAsStream("file/myfile.xml"); 

```


---

下面是从网络上转载过来的内容：

1，首先，Java中的getResourceAsStream有以下几种： 
1. Class.getResourceAsStream(String path) ： path 不以’/'开头时默认是从此类所在的包下取资源，以’/'开头则是从ClassPath根下获取。其只是通过path构造一个绝对路径，最终还是由ClassLoader获取资源。 

2. Class.getClassLoader.getResourceAsStream(String path) ：默认则是从ClassPath根下获取，path不能以’/'开头，最终是由ClassLoader获取资源。 

3. ServletContext. getResourceAsStream(String path)：默认从WebAPP根目录下取资源，Tomcat下path是否以’/'开头无所谓，当然这和具体的容器实现有关。 

4. Jsp下的application内置对象就是上面的ServletContext的一种实现。 



