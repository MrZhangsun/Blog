---
title: DOS批处理入门
date: 2017-12-29 15:42:51
tags: WINDOWS
---

### echo命令
	echo hello: 向控制台输出hello
	echo hello > file.txt: 将hello输出到文件file.txt中
	echo world >> file.txt: 将world追加到文件file.txt中

### md命令
	md file1, file2, file3 ... 在当前路径下创建多个文件夹

### dir命令
	dir /b: 显示当前路径下所有文件及文件夹名
	dir /s: 显示当前路径下的所有文件及子文件

### > 和 >>
	echo hello > file1.txt: 将hello写入到文件中并覆盖原来的内容
	echo world >> file.txt: 将world追加到文件file.txt中
### type命令
	filename.ext: 查下看文件中的内容
### [通配符](https://baike.baidu.com/item/%E9%80%9A%E9%85%8D%E7%AC%A6/92991?fr=aladdin) * 和 ?
	
	*: 匹配所有的内容
	?: 仅代表单个字
	dir /b /s > file.txt: 将文件及文件夹的名称信息输出到file文件中
	
### ren = rename命令
	ren old.txt new.txt： 重命名文件