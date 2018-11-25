---
title: Cookie和Session
date: 2014-11-2 00:56:12
tags: javaWEB
---

### 什么是Cookie,Cookie有什么规范

	Cookie是客户端绘画技术,说白了就是将客户数据保存在和客户端(浏览器上).Cookie是由服务器创建的,通过response响应,发送给客户端一个键值对进行保存,当客户端再次请求的时候会携带这个Cookie(HTTP请求行中)到服务器,这样服务器就可以识别客户端了.


### HTTPCookie的规范如下:

	Cookie的上限为4kb,一个服务器最多在客户端浏览器上保存20个Cookie,一个浏览器最多保存300个Cookie


### Session的理解
	
	Session是服务器端的绘画技术,当我们第一次访问JSP或者Servlet动态资源的时候服务器会调用getSession()获取Session对象,访问HTML一些静态资源的时候是不会创建Session对象的.

### Session的失效:

	1.服务器会把长时间没有活动的Session移除掉,此时Session失效
	2.我们手动的调用invalidate()方法使得Session失效

### Session的作用域问题:

![](https://i.imgur.com/IfQVHGt.png)

### 当Cookie被禁用时如何处理Session问题

![](https://i.imgur.com/FPo25cE.png)