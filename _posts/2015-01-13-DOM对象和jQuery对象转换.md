---
title: DOM对象和jQuery对象转换
layout: post
categories:
  - 研磨技术
---

## DOM对象

    [javascript]
    //DOM对象是JS原生的对象，它具备自己一套方法。
    var aa = document.getElementById("aa");
    

## jQuery对象

    [javascript]
    //jQuery对象通过“选择器”获取，$aa是一个数组对象
    var $aa = $("#aa");
    

## 从jQuery对象 &#8211;> DOM对象

    [javascript]
    //方式1
    var index = 0;
    var bb = $aa[index];
    //方式2
    var bb = $aa.get(index);
    

## 从DOM对象 &#8211;> jQuery对象

    [javascript]
    //通过$()方式来转化
    $cc = $(aa);
    

**this**和**$(this)** 就是上面介绍的DOM和jQuery对象的2种特殊情况。

看看下面的一段代码：

    [html]
    <input type="button" value="点击" onclick="onclickHandler(this)" />
    <input type="button" value="点击2" onclick="onclickHandler2($(this))" />
    <script>
    function onclickHandler(domObj){
        //this作为实参传递进来，代指input的DOM对象，输出"点击"
        console.log(domObj.value);
    }
    function onclickHandler2(jqObj){
        //$(this)作为实参传递进来，代指input的jQuery对象，输出"点击2"
        console.log(jqObj[0].value);
    }
    </script>