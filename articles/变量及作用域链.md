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

# 2、执行环境及作用域

执行环境(execution context即'环境')定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量环境，环境中定义的所有变量和函数都保存在这个对象中。虽然我们无法访问该对象，但是解析器在处理数据时会在后台使用它。

全局执行环境是最外围的一个执行环境。根据宿主环境的不同，表示执行环境的对象也不一样。在web浏览器中，全局执行环境被认为是window对象，因此所有全局变量和函数都是作为window对象的属性和方法创建的。

当代码在一个环境中执行时，会创建变量对象的一个**作用域链**。作用域链是为了保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象。活动对象最开始只有一个变量，那就是arguments对象(这个对象在全局环境中是不存在的)。作用域链中的下一个变量对象来自于它的包含环境，而再下一个变量对象则来自下一个包含环境。这样一直延续到全局执行环境。全局执行环境的变量对象始终都是作用域链中的最后一个对象。

上面的描述可能比较长，简单来说，作用域链就是像剥洋葱一样，最里层的变量能访问外层环境的变量，而外层的环境不能访问里层的变量。下面举个例子。

```javaScript
var color = 'blue';

function changeColor() {

    var anotherColor = 'red';
    function swapColor() {
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;

        // 在swapColor（）执行环境里能访问到color、anotherColor、tempColor
    }
        // 在这里能访问到color、anotherColor
}

// 在最外头，即全局环境下只能访问到color
changeColor();
```
```
用这个图来展示上面例子的作用域链

window 
    |
    |----color
    |
    |----changeColor()
              |
              |----anotherColor
              |
              |----swapColor()
                        |
                        |----tempColor
```

用书上的说法就是，内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。这些环境之间的联系是线性、有次序的。每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能向下搜索作用域链而进入另一个执行环境。

例如在，swapColor()中，访问变量color，在swapColor()局部环境中搜索不到该变量，就向上一级作用域链搜索。

## 2.1、延长作用域链
执行环境总共只有2种：全局和局部。有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。就是当执行流进入下列任何一个语句是，作用域链就会得到加长。
- try-catch语句的catch块
- with语句
```javaScript

function test() {

    var para = 'abc';
    with(location) {

        var url = href + para;
    }
    return url;
}

```

上面的代码块中，with语句接受的是location对象，这个变量对象被添加到了作用域链的前端。当在引用变量href时(实际上时引用的location.href)，可以在当前执行环境的变量对象中找到。

