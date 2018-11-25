---
title: Interceptor和Filter
date: 2015-11-13 15:45:46
tags: Struts2
---
#### Filter的生命周期:
1.当服务器启动的时候加载过滤器实例,调用init方法进行初始化
2.当每一次请求的时候都调用doFilter方法进行处理
3.关闭服务器的时候调用Destory进行销毁

#### Filter与Interceptor的区别
1.过滤器依赖于Servlet容器,而拦截器不依赖于Servlet容器
2.过滤器基本对所有请求都进行过滤,而拦截器只对Action拦截
3.过滤器是基于函数回调,拦截器是基于反射机制
4.过滤器不能访问值栈中的数据,而拦截器可以

#### 什么是Listener
1.Listener是Servlet事件监听器，Servlet事件监听器就是一个实现特定接口的Java程序，
2.专门用于监听Web应用程序中ServletContext、HttpSession和ServletRequest等域对象的创建和销毁过程，监听这些域对象属性的修改以及感知绑定到HttpSession域中某个对象的状态。