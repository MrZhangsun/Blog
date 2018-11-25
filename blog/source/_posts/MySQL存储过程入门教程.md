---
title: MySQL存储过程入门教程
date: 2017-09-08 09:06:21
tags: MySql
---
[TOC]

#### 什么是存储过程？
存储过程是存储在MySQL数据库中的已经经过编译好的SQL指令集。

#### 存储过程的特点：

- 因此执行时不需要数据库进行编译、优化就可以直接运行，相比直接执行SQL语句而言省去了编译和优化的环节，从而节省了系统的消耗。
- 触发器是在对数据库中数据进行增删改的时候触发执行的，是一个自动的过程。
- 存储过程能将对多个表的复杂操作封装到一起，结合MySQL事务进行执行。如果用程序来执行，那就变成了多次执行单条SQL语句，执行
- 过程中不仅每次需要数据库进行编译、优化，而且需要多次连接数据库，十分消耗性能。
- 存储过程能对数据库的访问权限进行细化。
#### 什么情况下应该使用存储过程？

- 存储过程的使用要根据项目实际情况来决定。并非将所有的业务逻辑都封装成存储过程就好。因为存储过程的移植性差，不同的数据库对于存储过程的实现语法都存在差异。
- 不要将大量的运算放在存储过程中去，因为高并发下，多个用户访问，数据库的运算量会激增，导致数据库压力过大，不如将所有的计算放在web端。
- 针对并发量大的web项目，不建议大量使用存储过程，原因如上。如果要是用，请控制存储过程的复杂程度，逻辑越简单越好。

#### 定义一个存储过程

	DELIMITER $$
	DROP PROCEDURE IF EXISTS countRow;
	CREATE PROCEDURE countRow(OUT num INT)
	    BEGIN
	      SELECT COUNT(*) INTO num FROM mysql.user;
	    END $$

	DELIMITER ;
	-- 调用存储过程
	CALL countRow(@s);
	SELECT @s as num;

#### 存储过程参数：IN 参数

	-- IN 类型的参数表示在调用的时候需要传入实际值，该值的作用域仅限于存储过程内部，不会对外输出该值
	DELIMITER $$
	DROP PROCEDURE IF EXISTS paramIn;
	CREATE PROCEDURE paramIn(IN p_in INT)
	    BEGIN
	      SELECT p_in AS init_val;
	      SET p_in = 100;
	      SELECT p_in AS update_val;
	    END $$

	DELIMITER ;

	-- 定义一个用户变量
	SET @p_in = 11;
	-- 查询变量的初始值
	SELECT @p_in;
	-- 调用存储过程
	CALL paramIn(@p_in);
	-- 在此查看用户变量的值是否仍为11
	SELECT @p_in;


#### 存储过程参数：OUT 参数

	-- OUT 类型的参数表示在调用的时候不能传入外部值，该值由存储过程在内部赋值之后返回给外部，可以在外部访问
	DELIMITER $$
	DROP PROCEDURE IF EXISTS paramOut;
	CREATE PROCEDURE paramOut(OUT p_out INT)
	    BEGIN
		SELECT p_out AS init_val;
	#         SET p_out = 200;
	#         SELECT p_out AS update_val;
	    END $$
	DELIMITER ;

	-- 定义一个用户变量
	SET @p_out = 22;
	-- 查询变量的初始值
	SELECT @p_out;
	-- 调用存储过程
	CALL paramOut(@p_out);
	-- 在此查看用户变量的值是否已经变成200
	SELECT @p_out;


#### 存储过程参数：INOUT 参数

	-- NOUT 类型的参数表示在调用存储过程的时候可以接收外部传入的参数值，执行完后也能将新值传出
	DELIMITER $$
	DROP PROCEDURE IF EXISTS paramInout;
	CREATE PROCEDURE paramInout(INOUT p_inout INT)
	    BEGIN
	      SELECT p_inout AS init_val;
	      SET p_inout = 300;
	      SELECT p_inout AS update_val;
	    END $$

	DELIMITER ;

	-- 定义一个用户变量
	SET @p_inout = 33;
	-- 查询变量的初始值
	SELECT @p_inout;
	-- 调用存储过程
	CALL paramInout(@p_inout);
	-- 在此查看用户变量的值是否已经变成200
	SELECT @p_inout;


#### 存储过程参数：不指定参数类型

	DELIMITER $$
	DROP PROCEDURE IF EXISTS paramDefault;
	CREATE PROCEDURE paramDefault(p_default INT)
	    BEGIN
		SELECT p_default AS init_val;
		SET p_default = 400;
		SELECT p_default AS update_val;
	    END $$

	DELIMITER ;

	-- 定义一个用户变量
	SET @p_default = 44;
	-- 查询变量的初始值
	SELECT @p_default;
	-- 调用存储过程
	CALL paramDefault(@p_default);
	-- 在此查看用户变量的值是否仍是44
	SELECT @p_default;

#### 总结：
以存储过程为中心，IN 表示能将值传入到存储过程中，但是不能传出去； OUT 表示不能将值传入到存储过程中，但是能将值传出到外部；INOUT 表示能将值传入到存储过程中，执行完成后也能将值传出到外部；如果不指定参数类型，默认为 IN。


#### 存储过程中变量的定义：使用 DECLARE 关键字

	DELIMITER $$
	DROP PROCEDURE IF EXISTS variableDefine;
	CREATE PROCEDURE variableDefine()
	    BEGIN
		-- 定义变量的格式：DECLARE 变量名 数据类型 （默认值可选）
		DECLARE variableName VARCHAR(20) DEFAULT "defaultName";

		-- 变量赋值格式：SET 变量名 = 变量值
		SET variableName = "signedValue";

		SELECT CONCAT("the new value is:", variableName) AS name;
	    END $$

	DELIMITER ;

	CALL variableDefine();

#### 存储过程中的变量作用域：详见IN、 OUT、 INOUT

#### 存储过程中的条件语句：IF -- ELSEIF -- ELSE -- END IF

	DELIMITER $$
	DROP PROCEDURE IF EXISTS conditional1;
	CREATE PROCEDURE conditional1(IN input INT)
	    BEGIN
		IF input BETWEEN 0 AND 60 THEN
		    SELECT "L" AS GRADE;
		ELSEIF input BETWEEN 60 AND 80 THEN
		    SELECT "M" AS GRADE;
		ELSEIF input BETWEEN 80 AND 100 THEN
		    SELECT "H" AS GRADE;
		ELSE
		    SELECT "Error" AS Warning;
		END IF;
	    END $$
	DELIMITER ;

	SET @input = 90;
	CALL conditional(@input);

#### 存储过程中的条件语句：CASE -- WHEN THEN

	DELIMITER $$
	DROP PROCEDURE IF EXISTS conditional2;
	CREATE PROCEDURE conditional2(IN case_val INT)
	    BEGIN
		CASE case_val
		    WHEN 20 THEN
		        SELECT "L" AS GRADE;
		    WHEN 30 THEN
		        SELECT "M" AS GRADE;
		    WHEN 40 THEN
		        SELECT "H" AS GRADE;
		    ELSE
		        SELECT "ERROR" AS Warning;
		END CASE ;
	    END $$

	DELIMITER ;

	SET @case_val = 30;
	CALL conditional2(@case_val);

#### 存储过程中的循环语句(先判断后执行)：WHILE DO -- END WHILE
	
	DELIMITER $$
	DROP PROCEDURE IF EXISTS loop1;
	CREATE PROCEDURE loop1(IN loop_val INT)
	    BEGIN
		WHILE loop_val > 10 DO
		    -- doing something
		END WHILE;
	    END $$
	DELIMITER ;

#### 存储过程中的循环语句(先执行后判断)：REPEAT -- UNTIL -- END REPEAT

	DELIMITER $$
	DROP PROCEDURE IF EXISTS loop1;
	CREATE PROCEDURE loop1(IN loop_val INT)
	    BEGIN
		REPEAT
		    -- doing something
		    UNTIL  loop_val > 10
		END REPEAT;
	    END $$
	DELIMITER ;


#### 存储过程中的循环语句：LOOP -- LEAVE -- END LOOP
	
	DELIMITER $$
	DROP PROCEDURE IF EXISTS loop1;
	CREATE PROCEDURE loop1(IN loop_val INT)
	    BEGIN
		label:LOOP
		    if loop_val > 10 THEN
		        -- 跳出循环
		        LEAVE label;
		    ELSEIF loop_val > 20 THEN
		        -- 跳出本次循环执行下一次循环
		        ITERATE label;
		    END IF;
		END LOOP;
	    END $$
	DELIMITER ;

#### 查看存储过程的状态：

	SHOW PROCEDURE STATUS LIKE 'conditional1';

	SHOW PROCEDURE STATUS LIKE 'conditional%';
#### 查看存储过程定义相关信息

	SHOW CREATE PROCEDURE conditional1;

#### 修改存储过程的特征参数（MySQL中不支持修改存储过程的内容）：

	ALTER PROCEDURE conditional1 SQL SECURITY DEFINER ; -- 修改存储过程的调用者（INVOKER \ DEFINER）

#### 删除存储过程

	DROP PROCEDURE conditional1;