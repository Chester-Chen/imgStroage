---
date: 2019/1/14 13:11:06 
title: vue.js devtools安装成功后，仍不可用
categories:
 - vue
 - js
tags: 
 - vue
 - js
 - 随笔
---

# 安装vue.js devtools有2种方法

### 1.在谷歌的应用商店里下载

搜索vue.js devtools就会看到了，，安装就不必多说了，，在座的各位都会了
![](https://raw.githubusercontent.com/Chester-Chen/imgStroage/master/images/devtools01.png)

<!--more-->
### 2.用npm拉取到本地
clone链接[https://github.com/vuejs/vue-devtools](https://github.com/vuejs/vue-devtools)

`git clone https://github.com/vuejs/vue-devtools`

步骤：

2.1在vue-devtools下安装依赖
```
cd vue-devtools
npm install
```
**静静的等待一会**

2.2修改manifest.json文件

将**persistent:false**改成**true**

其目录为：`vue-devtools\shells\chrome`

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/devtools02.png?raw=true)

2.3然后编译

`npm run bulid`

2.4载入插件

chrome浏览器>更多工具>扩展程序

首先**右上角打开开发者模式**

然后点击加载已解压的扩展程序

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/devtools03.png?raw=true)

选中vue-devtools\shells下的chrome文件夹

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/devtools04.png?raw=true)

以上方法2种方法任君选择

## 下载完成后注意
在浏览器设置里，，把vue该项设置为允许
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/devtools05.png?raw=true)

**特别注意**：在项目里引用的vue.js必须是，，开发版本的，而不是生产版本的(不然你打开调试模式是没有vue的)

生产版本会出现以下信息：

`Vue.js is detected on this page. Devtools inspection is not available because it‘s in production mode or explicitly disabled by the author`