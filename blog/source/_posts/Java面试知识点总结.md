---
title: Java面试知识点总结
date: 2018-02-01 14:22:30
tags: 面试
---
[TOC]
### JavaSE
#### 多线程

#### IO流
#### 反射
#### 集合

[java基础面试][5]

### 前台框架
#### JavaScript
1. **闭包**

	闭包是JavaScript中的一种语法现象，指的是在函数的内部定义了子函数，并且子函数访问了父函数中的成员变量。定义一个闭包现象如下：
	
		fout = function(){
			var v1 = 10;
			fin = function(){
				var v2 = v1;
				return v2;
			}
			return fin;
		}
		
		var out = fout() //  fin()
		var in = out() //  10
	通过闭包中的子函数可以访问到父函数中的成员变量但是外部的父函数却不能访问子函数中的成员变量，这就是Javascript语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

	既然fin可以读取fout中的局部变量，那么只要把fin作为返回值，我们不就可以在fout外部读取它的内部变量了吗！



2.  **JavaScript 中 undefined 和 not defined 的区别:**
	undefined是javaScript中的一种原始数据类型，表示没有初始化的变量或者函数的初始化值。如果一个变量或者函数只定义了，而没有初始化，则用typeof会返回一个undefined。

		var val;
		alert(typeof val) // undefined
		
	not defined是一种错误提示消息，表示变量没有定义过就使用了，如下：
		
		var val = x; // x会报错: Unresolved variable or type x

3. **全局对象**
	JavaScript的全局对象是指，该语言中的内置对象，这些对象可以在javaScript代码的任何位置不用声明就可以直接使用，类似Java中的基本类型数据。
	常用到的全局对象有：
	
		eval(String)：接受一个字符串参数，用于执行字符串代码。
		typeof： 返回一个变量的数据类型。
		Number() ：把对象的值转换为数字。
		String()： 把对象的值转换为字串。
		parseInt(String)：解析一个字符串并返回一个整数。
		parseFloat(String)：解析一个字符串并返回一个浮点数。
		decodeURI()：解码某个编码的 URI。
		decodeURIComponent()：解码一个编码的 URI 组件。
		encodeURI()：把字符串编码为 URI。
	下面面试题输出的结果是甚末？
	
		var y = 1;
		if (function f(){}) {
		    y += typeof f;
		}
		console.log(y);
	输出结果：1undefined
	
4. **怎么判断一个object是否是数组(array)？**

		object.__proto__ === Array.prototype; // 利用原型链
		
		$.isArray(object) // 利用jQuery

5. **JavaScript怎么清空数组？**

		var arr = [1, 2, 3, 4, 5];
		arr = []; // 方法一
		arr.length = 0; // 方法二 
6. **JavaScript中自执行匿名函数**	

	自执行匿名函数定义：
	常见格式：(function() { /* code */ })();
	解释：包围函数（function(){})的第一对括号向脚本返回未命名的函数，随后一对空括号立即执行返回的未命名函数，括号内为匿名函数的参数。
	作用：可以用它创建命名空间，只要把自己所有的代码都写在这个特殊的函数包装内，那么外部就不能访问，除非你允许(变量前加上window，这样该函数或变量就成为全局)。各JavaScript库的代码也基本是这种组织形式。
	
	总结一下，执行函数的作用主要为 匿名 和 自动执行,代码在被解释时就已经在运行了。
	
	下面代码输出的结果是甚末？
	
		var output = (function(x){
		    delete x;
		    return x;
		})(0);
		console.log(output);
输出是 0。 [delete][2] 操作符是将object的属性删去的操作。但是这里的 x 是并不是对象的属性， delete 操作符并不能作用。

7. **什么是 undefined x 1 ？**
	在chrome下执行如下代码，我们就可以看到undefined x 1的身影。
	
		var trees = ["redwood","bay","cedar","oak","maple"];
		delete trees[3];
		console.log(trees);
		
	当我们使用 delete 操作符删除一个数组中的元素，这个元素的位置就会变成一个占位符。打印出来就是undefined x 1。
	注意如果我们使用trees\[3] === 'undefined × 1'返回的是 false。因为它仅仅是一种打印表示，并不是值变为undefined x 1。
	
8. **下面代码输出什么？**

		var bar = true;
		console.log(bar + 0);   
		console.log(bar + "xyz");  
		console.log(bar + true);  
		console.log(bar + false);   
	输出是:
	1
	truexyz
	2
	1
	
	下面给出一个加法操作表:
	Number + Number -> 加法
	Boolean + Number -> 加法
	Boolean + Boolean -> 加法
	Number + String -> 连接
	String + Boolean -> 连接
	String + String -> 连接

9. **两种函数声明有什么区别？**

		var foo = function(){ 
		    // Some code
		}; 
		function bar(){ 
		    // Some code
		}; 
	第一种函数定义是在运行时；第二种定义实在解析时，存在变量提升。看看如下代码的运行结果：
	
		console.log(foo)
		console.log(bar)

		var foo = function(){ 
		    // Some code
		}; 
		function bar(){ 
		    // Some code
		}; 
	结果是：
		
		undefined
		function bar(){ 
		    // Some code
		};
	第二种方式JavaScript在执行时，会将变量提升。
	
10. [**JavaScript 变量提升**][3]
	JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。
	JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。
	
11. **内存泄漏**
	内存泄露是指变量使用完之后没有被销毁，一直常驻内存，无法被垃圾回收器回收导致的内存浪费，严重的时候会导致内存溢出。引发内存泄露的原因通常是因为不规范编码造成的。一般有一下几种现象：
	全局变量引起的内存泄漏：
	
		function leaks(){  
		    leak = 11; // leak 成为一个全局变量，方法执行完成之后，不会被回收
		}
	闭包引起的内存泄漏：
	
		function leaks(){
			let leak = 11; // 被闭包所引用，不会被回收
			function fin() {
				console.log(leak); 
			}
			return fin();
		}
	事件未清除导致的内存泄漏
	
#### jQuery
1. **Ajax**
	简述ajax 的过程。
	
	- 创建XMLHttpRequest对象,也就是创建一个异步调用对象
	- 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
	- 设置响应HTTP请求状态变化的函数
	- 发送HTTP请求
	- 获取异步调用返回的数据
	- 使用JavaScript和DOM实现局部刷新
	
同步和异步的区别?
syn
如何解决跨域问题?

JavaScript 的同源策略

	同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。所谓同源指的是：协议，域名，端口相同，同源策略是一种安全协议，指一段脚本只能读取来自同一来源的窗口和文档的属性。
解释jsonp的原理，以及为什么不是真正的ajax

	Jsonp并不是一种数据格式，而json是一种数据格式，jsonp是用来解决跨域获取数据的一种解决方案，具体是通过动态创建script标签，然后通过标签的src属性获取js文件中的js脚本，该脚本的内容是一个函数调用，参数就是服务器返回的数据，为了处理这些返回的数据，需要事先在页面定义好回调函数，本质上使用的并不是ajax技术
#### React
1. **简介**

	React是Facebook2013年开源的一个JavaScript项目，是用来构建UI界面的，相当于MVC模型中View层。React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。

2. **React主要的原理**

	传统的web应用，操作DOM一般是直接更新操作的，但是我们知道DOM更新通常是比较昂贵的。而React为了尽可能减少对DOM的操作，提供了一种不同的而又强大的方式来更新DOM，代替直接的DOM操作。就是Virtual DOM,一个轻量级的虚拟的DOM，就是React抽象出来的一个对象，描述dom应该什么样子的，应该如何呈现。通过这个Virtual DOM去更新真实的DOM，由这个Virtual DOM管理真实DOM的更新。

	为什么通过这多一层的Virtual DOM操作就能更快呢？ 这是因为React有个diff算法，更新Virtual DOM并不保证马上影响真实的DOM，React会等到事件循环结束，然后利用这个diff算法，通过当前新的dom表述与之前的作比较，计算出最小的步骤更新真实的DOM。

3. **特点：**
	- 不直接操作DOM对象，而是通过虚拟DOM通过diff算法以最小的步骤作用到真实的DOM上；
	- 组件化开发，代码的复用性高；
	- 异步更新页面，不用刷新整个页面；
4. **state, props, refs, keys的作用：**
	- state: 是React中组件的一个对象。React把用户界面当做是状态机，想象它有不同的状态然后渲染这些状态,可以轻松让用户界面与数据保持一致。 React中,更新组件的state,会导致重新渲染用户界面(不要操作DOM).简单来说,就是用户界面会随着state变化而变化。调用setState(data,callback)这个方法会合并、更新data到this.state，并重新渲染组件。渲染完成后，调用可选的callback回调。大部分情况不需要提供callback，因为React会负责吧界面更新到最新状态。
	
	- props: 用来从父级组件向子集组件传递参数的容器。React 里，数据通过props 从拥有者流向归属者。

	- ref: 是React中的一个特殊属性，这个属性用在render()方法返回的组件上，用来标记组件，以便于获取到该组件的实例。
	ref的形式如下：
	```
	<input ref="myInput" />
	```
		
		要想访问这个实例，可以通过this.refs来访问：

			this.refs.myInput
 
 	- key: 它是一个特殊的属性，它是出现不是给开发者用的（例如你为一个组件设置key之后不能获取组件的这个key props），而是给react自己用的。react利用key来识别组件，它是一种身份标识标识，就像我们的身份证用来辨识一个人一样。每个key对应一个组件，相同的key react认为是同一个组件，这样后续相同的key对应组件都不会被创建。
 	key值的唯一是有范围的，即在数组生成的同级同类型的组件上要保持唯一，而不是所有组件的key都要保持唯一。不仅仅在数组生成组件上，其他地方也可以使用key，主要是react利用key来区分组件的，相同的key表示同一个组件，react不会重新销毁创建组件实例，只可能更新；key不同，react会销毁已有的组件实例，重新创建组件新的实例。
 	
5. **定义一个组件：**

		import React from "react";
		import {Row,Col} from "antd";
		import "antd/dist/antd.css";

		export default class PCFooter extends React.Component{
		    render(){
		    
				return(
				    <footer className={"footer"}>
					<Row>
					    <Col span={2}></Col>
					    <Col span={20}>
						<p>&copy;&nbsp;2017-2050 ReactNews CopyRight Reserved.</p>
					    </Col>
					    <Col span={2}></Col>
					</Row>
				    </footer>
				);
		    };
		}

6. **AntD**
	Ant Design是蚂蚁金服开发的一款前端UI框架，可以帮助开发者快速搭建漂亮的页面，即使你是一个不懂设计的开发者，也能快速地写出比较好看的前端页面。从而提高了页面的开发效率。与React结合开发前端页面效果不错。
7. **开发流程**
	- 导入需要的模块；
	- 定义一个组件，继承React.Component，用到关键字export, default, class, extends
	- 编写构造器（可选，用于初始化参数）;
	- 编写render()方法，用于渲染组件；
	- 编写render()方法中的return()方法，返回组件；
	- 在其他组件中导入编写的组件，用props进行参数传递。
	
8. **NodeJS**

	简单的说 Node.js 就是运行在服务端的 JavaScript。自从前后端开发分离之后，前端代码从后段代码中剥离出来了，搭建独立的前端服务器，Node.JS就是前端项目服务器, 类似Tomcat、Apache、Nginx。
	Node.Js采用NPM对前端项目中的依赖JS包进行管理，在新版本中NPM已经包含在了Node.js中，用户只需要安装Node即可，不需要额外安装NPM。
	
	接下来我们使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8888 端口。 函数通过 request, response 参数来接收和响应数据。

实例如下，在你项目的根目录下创建一个叫 server.js 的文件，并写入以下代码：

	var http = require('http');

	http.createServer(function (request, response) {

	    // 发送 HTTP 头部 
	    // HTTP 状态值: 200 : OK
	    // 内容类型: text/plain
	    response.writeHead(200, {'Content-Type': 'text/plain'});

	    // 发送响应数据 "Hello World"
	    response.end('Hello World\n');
	}).listen(8888);

	// 终端打印如下信息
	console.log('Server running at http://127.0.0.1:8888/');
	以上代码我们完成了一个可以工作的 HTTP 服务器。

	使用 node 命令执行以上的代码：

	node server.js
	Server running at http://127.0.0.1:8888/
	
	
9. **NPM**
	
	- npm init：会引导你创建一个package.json文件，包括名称、版本、作者这些信息等
	- npm install moduleNames：安装Node模块
	- npm install <name> -g： 将包安装到全局环境中
	- npm install <name> --save： 安装的同时，将信息写入package.json中项目路径中（如果有package.json文件时，直接使用npm install方法就可以根据dependencies配置安装所有的依赖包，这样代码提交到github时，就不用提交node_modules这个文件夹了）。
	- npm uninstall moudleName：卸载node模块
	- npm update moduleName：更新node模块
	- npm list：查看当前目录下已安装的node包
	- npm -v：查看npm安装的版本
	
### 后台框架
#### SpringMvc
1. SpringMVC的工作流程？

	- 客户端发起请求到DispatcherServlet(前端控制器)；
	- 调用HandlerMapping(处理器映射器)，根据请求中的url到xml配置或注解(@RequestMapping)中去进行查找对应的Handler(处理器)；
	```
	<bean name="/url" class="com.example.controller.Controller"></bean> 
	```
	- HandlerMapping 生成HandlerIntercepter(处理器拦截器)，将其和Handler一并返回给前端控制器；
	
2. SpringMVC和Struts2的区别：
3. 
#### Spring
#### Mybatis
#### Struts2
#### Hibernate
#### SpringBoot

### 服务器
#### Tomcat
#### Nginx
1. 简介
2. Http服务器
3. 负载均衡服务器
[Nginx负载均衡的详细配置及使用案例详解.][4]
4. 反向代理服务器


### 数据库
#### MySQL
1. **Sql优化**
2. **锁**
	死锁
	锁策略
	锁粒度
		表级锁
		行级锁
3. **事务**
	原子性
	一致性
	持久性
	隔离性
		脏读
		不可重复读
		虚度/幻读
		序列化
#### Redis
1. **数据类型**
Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。
2. **Jedis**

3. **Redis与**

4. **Redis的应用场景**
	- 缓存
		Redis读写速度每秒在10万次左右，可以对查询频繁的数据进行缓存，能实现高效的查询。
	- 技术器
		使用INCREABY和DECREABY可以实现计数器
	- 分布式锁与单线程 
		Redis是单线程的所以可以控制多线程并发访问资源的问题
	- 计时器
		Redis新版本推出了key过期事件，可以通过订阅指定的key事件来实现定时任务，但这是新版本的特性。
#### MongoDB
1. **数据类型**
2. **JavaAPI**
### 技术点
#### Dubbo
Dubbo是阿里开源的一款微服务框架，可以与Spring整合实现分布式应用之间服务的管理。
Dubbo四个组件组成：
- 生产者：服务的提供者；
- 消费者：服务的订阅者；
- 注册中心：用于服务的发布和订阅；
	通常使用Zookeeper作为注册中心，因为Zookeeper可以通过节点进行订阅和发布数据。
- 监控中心：用于服务的监控和管理。
#### ActiveMQ

#### Zookeeper
	
	- Zookeeper最初时应用在大数据中的，被用于分布式系统中用于维护分布式应用配置的一致性。
	- 作为Dubbo的注册中心
	- 作为项目的配置中心
	首先动态加载相关bean，如果配置中心发生改变，对应的watcher监听到事件后，客户端则对相关bean进行重新注册，并且从配置中心获取到了最新数据，然后客户端直接调用getBean()方法获取相关bean实例，确保不再是之前引用。
#### Freemarker
#### SwaggerAPI
#### Solr
#### FastDFS
#### 定时任务框架
1. **JDK**
2. **Spring-Task**
3. **Quartz**
4. [**Elestic Job**][1]
5. **Zookeeper**
	
#### WebService（CXF）
#### Shiro

### 开发工具
#### IDEA
#### PostMan
#### Charles
#### Datagrid
#### Ubuntu
#### GIT

### 加分项
#### Kotlin

- 防止空指针
- Kotlin文件类型有class, object, menu, interface
- 伴生对象对应java中的static
- 但例采用object 
- 高阶函数
- Lambda表达式
- 详见博客《Kotlin基础语法》
#### Vertx
- EvenBus异步执行，提高执行效率
- 使用多层嵌套解决同步问题
- 使用标签跳出指定层
#### Python





























[1]: http://blog.csdn.net/fanfan_v5/article/details/61310045
[2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete
[3]: http://www.runoob.com/js/js-hoisting.html
[4]: http://www.cnblogs.com/wang-meng/p/5861174.html#3896182
[5]: http://www.cnblogs.com/wang-meng/p/5701918.html
[6]: http://www.cnblogs.com/wang-meng/




