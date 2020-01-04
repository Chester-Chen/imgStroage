---
date: 2019/2/10 16:24:40 
title: babel 中去除严格模式
categories:
 - webpack
 - vue
 - js
tags:
 - js
---


# babel 中去除严格模式
<!--more-->
**doc:**[https://github.com/genify/babel-plugin-transform-remove-strict-mode](https://github.com/genify/babel-plugin-transform-remove-strict-mode)

## install
`npm install babel-plugin-transform-remove-strict-mode`

## usage
在.bagelrc中添加字段
```
{
  "plugins": ["transform-remove-strict-mode"]
}
```