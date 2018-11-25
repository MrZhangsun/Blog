---
title: JS基础知识点
date: 2018-01-31 09:05:37
tags: JS
---
#### 什么是JS中的闭包
闭包就是能够读取其他函数内部变量的函数

	function foo(x) {
	    var tmp = 3;
	    return function (y) {
		alert(x + y + (++tmp));
	    }
	}
	var bar = foo(2); // bar 现在是一个闭包
	bar(10);
&emsp;&emsp;由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数

#### JS的数据类型
	1.基本数据类型:String,Boolean,Number,Undefined, NULL
	2.引用数据类型:Object(Array,Date,RegExp,Function)

#### 获取页面中所有的input
	var domList = document.getElementsByTagName(‘input’)

#### 什么是Ajax和JSON，它们的优缺点
	Ajax是异步的Javascript和XML,用于WEB页面中异步数据交互
	优点:
		1.使得用户在不刷新页面的情况下加载数据,提高用户的体验性
		2.在不重载页面的情况下加载局部数据,减少数据的交互量  
	缺点:
		1.存在安全问题
		2.对搜索引擎支持不好
		3.一些手持设备目前还不支持  
 Json是一个轻量级的数据交互格式,ECMA的一个子集,所以js支持性好

	优点:
		1.轻量级,易于人编写和阅读
		2.是ECMA的一个子集,和js结合好,便于解析

#### 什么是eval()函数
此函数是将json格式的字符串转换成JavaScript对象

#### JS遍历Json数据

	var data=[{name:"a",age:12},{name:"b",age:11},{name:"c",age:13},{name:"d",age:14}];
	for(var o in data){
		alert(o);
		alert(data[o]);
		alert("text:"+data[o].name+" value:"+data[o].age );
	}

	<script type="text/javascript">
		function text(){
			var json = {"options":"[{/"text/":/"王家湾/",/"value/":/"9/"},{/"text/":/"李家湾/",/"value/":/"10/"},{/"text/":/"邵家湾/",/"value/":/"13/"}]"}
			json = eval(json.options)
			for(var i=0; i<json.length; i++){
				alert(json[i].text+" " + json[i].value)
			}
		}
	</script>

#### Ajax技术原理
&emsp;&emsp;Ajax是通过XMLHTTPRequest对象异步向服务器端发送请求,服务器端接收请求处理,返回XML或者JSON数据

#### Ajax的执行步骤
1.创建XMLHTTPRequest对象
2.监听 onreadystatechange事件
3.打开连接
4.发送请求

	function Ustbwuyi() {
	    var data = document.getElementById("username").value;
	    CreateXmlHttp();
	    if (!xmlhttp) {
		alert("创建xmlhttp对象异常！");
		return false;
	    }

	    xmlhttp.open("POST", url, false);

	    xmlhttp.onreadystatechange = function () {
		if (xmlhttp.readyState == 4) {
		    document.getElementById("user1").innerHTML = "数据正在加载...";
		    if (xmlhttp.status == 200) {
		        document.write(xmlhttp.responseText);
		    }
		}
	    }
	    xmlhttp.send();
	}

XmlHttpRequest对象解析:
　　   它的属性有：
  　　  onreadystatechange  每次状态改变所触发事件的事件处理程序。
  　　  responseText     从服务器进程返回数据的字符串形式。
  　　  responseXML    从服务器进程返回的DOM兼容的文档数据对象。
  　　  status           从服务器返回的数字代码，比如常见的404（未找到）和200（已就绪）
  　　  status Text       伴随状态码的字符串信息
  　　  readyState       对象状态值
　　　　0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）
　　　　1 (初始化) 对象已建立，尚未调用send方法
　　　　2 (发送数据) send方法已调用，但是当前的状态及http头未知
　　　　3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误，
　　　　4 (完成) 数据接收完毕,此时可以通过通过responseXml和responseText获取完整的回应数据

定义js对象和属性

	var person = function(){};
	var p = persopn.prototype;
	p.name='张三';
	page=18;
	p.add = function() {
		alert("HELLO");
	}
