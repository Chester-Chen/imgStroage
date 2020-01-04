---
date: 2019/1/9 17:51:32 
title: MYSQL安装问题(the service already exists)
categories: 安装问题
tags: 
 - MySQL
 - 数据库
---

&emsp;将mysql 8.x，，删除后，，安装mysql 5.x时，执行`mysqld install`后，总是提示8.x版本已存在，，导致出错。
\7

# 第一步
1.以管理员身份运行cmd&emsp;( 这就不用多说了把 )<br/><br/>
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/mysql01.png?raw=true)

<!--more-->

# 第二步
2.输入`sc query mysql`,<br/><br/>
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/mysql02.png?raw=true)
<br/><br/>
查看mysql服务，发现之前确实安装过



# 最后一步
3.输入 `sc delete mysql`<br/><br/>
提示：<br/>
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/mysql03.png?raw=true)<br/><br/>
之后再安装就没有问题了


