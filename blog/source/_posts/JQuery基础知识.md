---
title: JQuery基础知识
date: 2015-05-31 09:19:52
tags:  JQuery
---
#### JQuery遍历Json数据

	var json = [
	    {"id":"1","tagName":"apple"},
	    {"id":"2","tagName":"orange"},
	    {"id":"3","tagName":"banana"},
	    {"id":"4","tagName":"watermelon"},
	    {"id":"5","tagName":"pineapple"}
	];

	$.each(json, function(idx, obj) {
	    alert(obj.tagName);
	});

#### JQuery对象和Dom对象的相互转换
          
Jquery对象是一个数组对象,可以通过[index]的方法得到相应的DOM对象:

	var $userId = $(“#userId”); //Jquery对象
	var userId = $userId[0]; //DOM对象
	
另一个方法是Jquery本身提供的,通过get(index)方法得到相应的DOM对象:
	
	var $userId = $(“#userId”); //Jquery对象
	var userId = $userId.get(0); //DOM对象
对于DOM对象,使用$()把DOM对象包装起来,就可以获得一个Jquery对象:

	var userId = document.getElementById(“userId”); //Jquery对象 var 		$userId =$(userId); //DOM对象

$(function(){})---$(document).ready(function)和window.onload的区别:

	1.两个方法的执行时机不一样,$(function(){})在文档的DOM树形成之后就开始执行,window.onload()是在所有资源加载完毕之后才开始执行
	2.$(function(){})比window.onload()先执行
	3.$(function(){})可以重复执行,window.onload()只能执行一次
