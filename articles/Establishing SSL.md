---
date: 2019/1/10 20:22:53
title: Establishing SSL connection without server's identity verification is not recommended.
categories: 
 - 随笔
 - jdbc
tags: 
 - jdbc
---

<!--more-->
在使用jdbc连接mysql的时候，出现以下警告：

	WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

因为高版本的需要ssl连接,在客户端连接数据库时，进行了加密。   

**解决办法**：在请求url里加上`useSSL=true`

![](https://i.imgur.com/FVItYNG.png)