---
title: JSP的静态包含与动态包含
date: 2014-4-21 15:32:06
tags: JSP
---
JSP的静态包含:

	<%@ include file="header.jsp"%>
静态包含是把所有的片段都复制在一起,在一起翻译,一起编译成一个.class文件

 JSP的动态包含:
 
	<jsp:include page="header.jsp"></jsp:include>
动态包含是把每一个jsp都单独翻译,单独编译成独自的.class,然后将运行结果复制在一起对外显示。动态包含会随时监测包含的资源是否有变化,如果有变化,则更新,从新编译。