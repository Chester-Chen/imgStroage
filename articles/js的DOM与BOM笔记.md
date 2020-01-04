date: 2020.01.02

# 一、js中的DOM与BOM

JavaScript是一种专为与网页交互设计的脚本语言，由三部分组成：
- ECMAScript ,提供核心语言功能
- DOM(文档对象模型)， 提供访问和操作网页内容的方法和接口
- BOM(浏览器对象模型)， 提供与浏览器交互的方法与接口

## 1.1 DOM
DOM把整个页面映射为一个多层次节点结构。通过DOM创建的树形图，开发人员可以轻松的删除、添加、替换和修改任何节点。

### 1.1.1 选择符API
Selectors API是由W3C制定的标准，让浏览器原生支持css查询。Selectors APILevel1的核心两个方法：`querySelector()`和`querySelectorAll()`，在兼容的浏览器中可以通过Document和Element类型的实例调用它们。

**querySelector()**     
querySelector()方法接受一个css选择符，返回与该模式匹配的第一个元素，如果没有找到匹配元素，则返回null。
```javaScript
// 取body元素
var body  = document.querySelector("body"); 

// 取id为test的元素
var test = document.querySelector("#test");

// 取class为test的元素
var test = document.querySelector(".test");

// 取class为test的第一个图像元素
var test = document.body.querySelector("img.test");
```


通过Document类型调用的querySelector()时，会在文档元素的范围内查找匹配的元素

通过Element类型调用的querySelector()时，会在该元素后代元素的范围内查找匹配的元素

**querySelectorAll()**  
querySelectorAll与querySelector接受的参数一样，都是一个css选择符，但返回的匹配元素不仅仅是一个元素。该方法返回的是一个NodeList的实例。如果没有找到匹配元素，NodeList就是空的。其具体使用与上面雷同。

看到这里就有些困扰了，平时接触的都是getElementsByxxx，这个与qurySelector()有什么区别呢。

### 1.1.2 getElementsByxxx与qurySelector()区别

getElementsByxxx获取的是`动态集合`，qurySelector()获取的是`静态集合`。看下面代码具体分析。

** 先来看看getElementsByxxx **
```JavaScript
<body>
<ul id="box">
    <li class="a">测试1</li>
    <li class="a">测试2</li>
    <li class="a">测试3</li>
</ul>
</body>
<script type="text/javascript">
    var ul = document.getElementById('box');
    var list = ul.getElementsByTagName('li');
     for(var i =0;i<list.length;i++){
        ul.appendChild(document.createElement('li')); //动态追加li
    }
</script>

```
上述代码会陷入死循环，这是因为在第一次获取到3个li后，每插入一个子元素，list的值就会更新。所以getElementsByxxx是获取的动态集合，它会随着DOM的变化而变化。

> 为了证明它是动态变化的请看下面这个修改

```JavaScript
<body>
<ul id="box">
    <li class="a">测试1</li>
    <li class="a">测试2</li>
    <li class="a">测试3</li>
</ul>
</body>
    <script type="text/javascript">
        var ul = document.getElementById('box');
        var list = ul.getElementsByTagName('li');
        console.log('初始list的值为：' + list.length); //3
            for(var i =0;i<4;i++){
            ul.appendChild(document.createElement('li')); //动态追加li
        }
        console.log('循环后list的值为：' + list.length); //7
    </script>
```
for循环结束后，ul里新添加了4个li，list的长度也编程7了。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.02/01.png?raw=true)

**再来看看qurySelector()**

```JavaScript
<body>
    <ul id="box">
        <li class="a">测试1</li>
        <li class="a">测试2</li>
        <li class="a">测试3</li>
    </ul>
</body>
<script type="text/javascript">
    var ul = document.querySelector('#box');
    var list = ul.querySelectorAll('li');
    console.log('初始list的值:' + list.length); // 3
    for(var i = 0;i<list.length;i++){
        ul.appendChild(document.createElement('li'));//动态追加li
    }
    console.log('循环后list的值:' + list.length); // 3
</script>
```
可以看到使用querySelectorAll是静态获取的，第一次获取后，dom发生变化，其值也不会改变。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.02/02.png?raw=true)


## 1.2 BOM

记到这里实在有太多细致的东西没有理清，暂时放下记录，后续补写。