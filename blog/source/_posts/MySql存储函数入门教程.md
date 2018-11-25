---
title: MySql存储函数入门教程
date: 2017-12-19 13:45:37
tags: MySql
---
[TOC]
#### 什么是存储函数
存储函数和存储过程相似，都是用一个事务进行管理的多条SQL语句的集合。
#### 存储函数的定义

	DROP FUNCTION IF EXISTS funName; -- 便于修改存储函数，非必须
	CREATE FUNCTION funName(param 参数类型)
	  RETURNS 返回值类型
	  BEGIN
	    RETURN 1; -- 返回值
	  END;
	
#### 存储函数的调用
	-- 直接调用 SELECT 存储函数名(参数)
	SELECT fun("param")
	
	-- 作为查询条件调用
	SELECT * FROM user WHERE id = findId("param")
#### 存储过程中的语法

- 流控制（Flow-of-control）语句(IF，CASE，WHILE，LOOP，WHILE，REPEAT，LEAVE，ITERATE)也是合法的。
- 变量声明(DECLARE)以及指派(SET)是合法的. 
- 允许条件声明. 
- 异常处理声明也是允许的. 
- 但是在这里要记住函数有受限条件:不能在函数中访问表
#### 存储函数与存储过程的区别

- 存储函数有且只有一个返回值，而存储过程不能有返回值；
- 函数只能有输入参数，而且不能带in, 而存储过程可以有多个in,out,inout参数；
- 存储过程中的语句功能更强大，存储过程可以实现很复杂的业务逻辑，而函数有很多限制，如不能在函数中使用insert,update,delete,create等语句；存储函数只完成查询的工作，可接受输入参数并返回一个结果，也就是函数实现的功能针对性比较强；
- 存储过程可以调用存储函数。但函数不能调用存储过程；
- 存储过程一般是作为一个独立的部分来执行(call调用)。而函数可以作为查询语句的一个部分来调用。