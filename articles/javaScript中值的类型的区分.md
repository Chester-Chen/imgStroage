date: 2020.01.06

# 1、基本类型和引用类型

ECMAScript中包含两种不同类型的值：基本类型和引用类型的值。基本类型指简单的数据段，引用类型指可能由多个值构成的对象。

基本数据类型：Undefined、Null、Boolean、Number、String。这5种基本数据类型是按值访问的，因为可以操作保存在变量中的实际的值。

而引用类型的值是保存在内存中的对象。在JavaScript中不允许直接访问内存中的位置，也就是不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。

## 1.2、复制变量值

**复制基本类型的值**
```javaScript

var num1 = 3;

var num2 = num1;    // 3
```
当使用num1来给num2初始化时，num2中的值只是num1中3的一个**副本**，两个变量的3彼此完全独立。这两个变量参与任何操作都不会互相影响。


**复制引用类型的值**
```javaScript

var obj1 = new Object();
var obj2 = obj1;
obj1.name = 'chester';
console.log(obj2.name); // 'chester'

```
变量obj1保存了一个对象的实例，然后这个值被复制到了obj2中。其实，obj1和obj2都是指向同一个对象。这里复制的值是一个**指针**，这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量都指向堆中的同一个对象。所以当改变其中的一个变量时，就会影响另一个变量。

### 1.2.1、传递参数

在向参数传递基本类型值时，被传递的值会被复制给一个局部变量(arguments对象中的一个元素)。在向参数传递引用类型的值时，会把这个值在内存中的地址复制给局部变量。请看下面几个例子。
```javaScript

function addNum(num) {
    num += 10;
    return num;
}
var count = 20;
var result = addNum(count);
console.log(count);         // 20  没有发生变化
console.log(result);        // 30
```

上面这个基本类型好理解。下面看看对象传参。
```javaScript

function setName(obj) {
    obj.name = 'chester';
}
var person = new Object();
setName(person);
console.log(person.name);   // 'chester'

```
参数obj和变量person指向堆中的同一对象，所以当添加obj.name的属性后，person的也随之改变。
很多开发人员错误的认为：在局部作用域中修改的对象会在全局作用域中反映出来，就说明参数是按引用传递的。为了证明对象是按值传递的，请看下面。

```javaScript

function setName(obj) {
    obj.name = 'chester';
    obj = new Object();
    obj.name = 'Rose';
}
var person = new Object();
setName(person);
console.log(person.name);   // 'chester'

```
`person.name`显示的值仍然是`chester`, 即便在函数内部修改了参数的值，但原始的引用保持不变。实际上， 当在函数内部重写obj时，这个变量的引用就是一个局部对象了，这个局部对象会在函数执行完毕后立即销毁。

