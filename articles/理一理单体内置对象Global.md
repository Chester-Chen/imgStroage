date: 2020.01.04

对于Global和window看到实在摸不清两者的区别。网上查阅资料后，稍有理解，此处记下自己的见解备忘。

**什么是内置对象：** 由ECMAScript实现提供的、不依赖于宿主环境的对象，就是说这些对象在ECMAScript程序执行前就已经存在了。*开发人员不必显式的实例化内置对象*，因为它们已经实例化了。下面主要分析Global。

# Global对象
> 《JavaScript高级程序设计》第三版中写道，ECMAScript 中的Global 对象在某种意义上是作为一个终极的“兜底儿对象”来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。事实上，没有全局变量或全局函数；所有在全局作用域中定义的属性和函数，都是Global 对象的属性。本书前面介绍过的那些函数，诸如isNaN()、isFinite()、parseInt()以及parseFloat()等看起来都像独立的函数，实际上全都是Global对象的方法。

# window对象
> window对象是BOM的核心，它表示浏览器的实例。在浏览器中， window对象扮演着双重角色，它既是通过JavaScript访问浏览器窗口的一个接口 ，又是ECMAScript规定的Global对象。意味着在网页中定义的任何一个对象、变量和函数，都以window作为其Global对象。

```JavaScript
var person = "chester";
function showPrn() {
    console.log(window.person);  // "chester"
}
window.showPrn();  // chester
```
可以看到全局变量和全局函数是window对象的属性和方法。

我都懵币了。Global和window是一个东西吗？

当我`console.log(Global);`的时候，提示`Uncaught ReferenceError: Global is not defined`

---
后来查阅一些资料后，我理解为，Global对象是" 老祖宗 "，它不属于任何其他对象的属性和方法，子子孙孙都可以对它认祖归宗，都能在它这找到归属，所以我理解为Global是一个抽象的概念，不能直接访问。

```JavaScript
function show() {
	var  a = this;
	console.log(typeof(a));     // Object
	console.log(a);             // Window{...}
 }
 ```
不知道这样能不能解释，在浏览器中顶层对象是window。在一定程度上，window是Global的子对象。在浏览器中，window就是Global，是Global的更具体的实现。

> Global是js语言核心的东西，而window只是浏览器引擎上实现的一个对Global对象的封装。JS不仅仅局限于浏览器，还有其他地方。window对象是宿主对象也就是在一定的环境中才会生成的对象，这个环境就是浏览器。而global对象是在任何环境中都存在的。