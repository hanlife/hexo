---
title: Ajax
comments: true
date: 2018-07-16 11:31:34
tags: [JavaScript,ajax]
categories: JavaScript
---

# Ajax实现代码

```
//创建一个XMLHttpRequest对象
var xhr = new XMLHttpRequest();

//监听statechange事件
xhr.onreadystatechange = function() {
  /**
	 * XMLHttpRequest的readystate有五个状态
	 * 0 还没有调用open方法
	 * 1 已调用open方法，尚未调用send方法
	 * 2 已调用send，但尚未接收到响应
	 * 3 已接收到部分响应数据
	 * 4 已经接收到到全部数据，而且可以在客户端使用
	 */
  if (xhr.readystate == 4) {
    //状态码在200 到 300表示请求成功，状态码304表示资源没有被修改，可以直接使用缓存中的版本
    if ((xhr.status >=200 && xhr.status < 300) || xhr.status == 304)) {
      alert(xhr.responseText);
    } else {
      //发生错误打印状态码，
      alert("Request was unsuccessful: " + xhr.status);
    }
  }
}
//打开请求以备发送
xhr.open('post', 'http://example.com', false);
//设置请求头
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
//序列化表单
var form = document.getElementById('test-form');
//发送请求
xhr.send(serialize(form));
```
<!--more-->
## XMLHttpRequest对象
所有现代浏览器 (IE7+、Firefox、Chrome、Safari 以及 Opera) 都内建了 XMLHttpRequest 对象。
> 创建 XMLHttpRequest 对象的语法：
```
xmlhttp=new XMLHttpRequest();
```
> 老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：

```
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
```
> 因此

在创建XMLHttpRequest对象时需要做兼容：

```
if (window.XMLHttpRequest)
  {// code for all new browsers
  xmlhttp=new XMLHttpRequest();
  }
else if (window.ActiveXObject)
  {// code for IE5 and IE6
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
```

## onreadystatechange

onreadystatechange 是一个事件句柄。它的值 (state_Change) 是一个函数的名称，当 XMLHttpRequest 对象的状态发生改变时，会触发此函数。状态从 0 (uninitialized) 到 4 (complete) 进行变化。仅在状态为 4 时，我们才执行代码。

##  readyState


状态码 | 含义
---|---
0 | 还未调用
1 | 当open方法被成功调用
2 | 当send方法被调用，成功接收到HTTP响应头(HEADERS_RECEIVED)
3 | 一旦HTTP响应内容开始加载(LOADING)
4 | 一旦HTTP响应内容结束加载(DONE)

## setRequestHeader

设置请求头
```
setRequestHeader( Name, Value )
```

第一个参数是header的字符串名称，第二个参数是字符串值。如果请求需要多个header，这个方法就要被调用多次。这个方法附加的请求头，在下次open方法调用时会被清空。

## send方法


```
send(Data)
```


如果不需要发送内容，这个参数可以省略。W3C草案声明这个参数可以是任意类型的值只要能被js转成字符串，除了DOM对象。如果用户代理无法序列化这个参数，这个参数会被忽略。Firefox3.0.x以及之前版本在send方法没传参数时会抛出异常。

如果参数是DOM对象，用户代理应该确保文档已经被转成XML格式，通过文档对象的inputEncoding属性编码。如果请求头的Content-Type还没有通过setRequestHeader方法设置，用户代理应该自动的增加一个值"application/xml;charset=charset"，其中的charset应该是用来编码文档的编码格式。

如果用户代理被配置成使用代理服务器，XMLHttpRequest对象应该适当修改请求连接代理而不是源服务器，发送Proxy-Authorization头配置。

## abort方法

如果XMLHttpRequest对象的readyState属性还没有变成4，这个方法可以终止请求。这个方法可以确保异步请求中的回调不被执行。

```
abort()
```

## responseText

当成功调用XMLHttpRequest对象的send方法之后，如果服务端响应式格式正确的XML，并且已经把Content-Type头设置成用户代理支持的XML类型，XMLHttpRequest对象的responseXML属性将会包含一个DOM文档对象。另外一个属性responseText将会包含服务端返回的文本类型，而不管它是否被理解为XML。

 