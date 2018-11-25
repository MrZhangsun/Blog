---
title: maven常见问题
date: 2017-01-01 16:23:27
tags: Maven
---
**原因分析：**
出现.lastUpdated结尾的文件的原因：由于网络原因没有将Maven的依赖下载完整，导致。
**解决方案：**
删除所有以.lastUpdate结尾的文件
1、切换到maven的本地仓库
2、在当前目录打开cmd命令行
3、执行命令：for /r %i in (*.lastUpdated) do del %i
在项目上执行 Maven Update （Alt + F5）

**Maven跳过测试的命令:**

	-Dmaven.test.skip=true(=号两端不能存在空格)