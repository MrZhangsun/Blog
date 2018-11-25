---
title: BigDecimal与Double
date: 2014-03-23 23:01:29
tags: JavaSE
---
### 大数据类型BigDecimal与Double
	
	我们知道在浮点类型做运算的时候存在损失精度的问题,这是因为计算机底层是以二进制的方式存储数据的,所以我们在和金钱打交道的时候就应该避免采用Double,而应该用大数据类型BigDecimal.
	BigDecimal与Double的实验分别采用两种方式计算两个小数的加法运算:
  
```java 
public static void testNaN() {
	int num1 = 10;
	float num2 = 10.1F;
	double num3 = 10.2;
	
	BigDecimal add1 = BigDecimal.valueOf(10.1).add(BigDecimal.valueOf(10.2));
	BigDecimal add2 = new BigDecimal("10.1").add(new BigDecimal("10.2"));
	BigDecimal add3 = BigDecimal.valueOf(num2).add(BigDecimal.valueOf(num3));
	Double add4 = num3 + num2;
	System.out.println(add1 + "****" + add2 + "****" + add3 + "****" + add4);
}```

执行结果
20.3****20.3****20.300000381469727****20.300000381469726

### 得出结论
	浮点型的运算会存在误差问题,所以我们需要进行小数精确运算的时候应该采用BigDecimal,BigDecimal实现了任意精度的浮点型运算,保证了运算的正确性
### 使用心得
	我们在使用BigDecimal的时候应该采用构造方法的形式,传入字符串的形式进行加减乘除计算
### API的使用
	add(other) — 和
	subtract(other) — 差
	multiply(other) — 积
	divid(other) — 商
	mod(oher) — 余
	int compareTo(other) — 比较两个数,如果相等返回0,如果小于other返回负数,如多大于other返回正数
	valueOf(other) — 将基数值转换为大数据类型
