date：2020.01.13

构造函数内含有`prototype`属性，该属性指向构造函数的原型对象。所有原型对象都有一个`constructor`，该属性指向`prototype`所在的构造函数。构造函数的实例，其`__proto__`属性指向构造函数的原型对象。

# 原型链
我们让原型对象等于另一个对象的实例，此时原型对象包含一个指向另一个原型的指针，相应的，另一个原型中也包含着一个指向另一个构造函数的指针，层层递进，构成的实例与原型的链条，就叫做原型链。
```javaScript
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function () {
    return this.property;
}

function SubType() {
    this.subProperty = false;
}

// 继承SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function() {
    return this.subProperty;
}

var instance = new SubType();
console.log(instance.getSuperValue());   // true
```
通过创建`SuperType`的实例，并将实例赋给`SubType.prototype`,本质上是重写了原型对象。`SubType`继承`SuperType`的属性和方法，同时也在自己的原型中添加新的方法`getSubValue()`。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.13/01.png?raw=true)

上面的图中其实还缺少了一条链。all we know，js中皆为对象，顶层继承至`Object`，该继承也是通过原型链实现的。
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.13/02.png?raw=true)

## 谨慎定义方法
- 子类需要覆盖父类中的方法时，或者添加父类中不存在的方法时，给原型添加方法的代码一定要放在**替换原型的语句之后。**
- 不能使用对象字面量创建原型方法，因为会重写原型链。
```javaScript
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function () {
    return this.property;
}

function SubType() {
    this.subProperty = false;
}

// 继承SuperType
SubType.prototype = new SuperType();

// 字面量添加新方法，会导致上一行的继承失效
SubType.prototype = {
    getSubValue: function() {
        return this.subProperty;
    },
    someOtherMethod: function() {
        return false;
    }
};

var instance = new SubType();
console.log(instance.getSuperValue());   // error
```
上面的代码中，`SuperType`把实例赋值给了`SubType`的原型，接着后面又把原型替换为一个对象字面量。所以现在的原型包含的是一个`Object`的实例，而非`SuperType`的实例，所以`SubType`和`SuperType`之间的链子已经断了。

## 原型链的问题
- 引用类型值的原型属性会被所有实例共享
- 创建子类型的实例时，不能向超类型的构造函数中传递参数。
```javaScript
function SuperType() {
    this.colors = ['red', 'blue'];
}

function SubType() {

}

// 继承SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push('yellow');
console.log(instance1.colors);       // [ 'red', 'blue', 'yellow' ]

var instance2 = new SubType();
console.log(instance2.colors);       // [ 'red', 'blue', 'yellow' ]
```
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.13/03.jpg?raw=true)

该例子中，`SuperType`构造函数定义了一个引用类型的`colors`属性，`SubType`继承父类后，其原型对象中的`colors`会共享给所有实例对象。

# 借用构造函数
为解决引用类型值带来的问题，可以采用**借用构造函数**。即在子类构造函数的内部调用父类构造函数，通过使用`apply()`和`call()`。
```javaScript
function SuperType(name) {
    this.name = name;
    this.colors = ['red', 'blue'];
}

function SubType() {
    SuperType.call(this);
}

// 继承SuperTyope
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push('yellow');
console.log(instance1.colors);      // [ 'red', 'blue', 'yellow' ]

var instance2 = new SubType();
console.log(instance2.colors);      // [ 'red', 'blue' ]
```
可以看到instance1和instance2的各自colors互不影响了。

## 组合继承
组合继承是指，将原型链和借用构造函数组合。使用原型链实现对原型属性和方法的继承，借用构造函数来实现对实例属性的继承。
```javaScript
function SuperType(name) {
    this.name = name;
    this.colors = ['red', 'blue'];
}

function SubType(name) {
    SuperType.call(this, name);
}

// 继承SuperTyope
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayName = function () {
    console.log(this.name);
}

var instance1 = new SubType('rose');
instance1.colors.push('yellow');
console.log(instance1.colors);      // [ 'red', 'blue', 'yellow' ]
instance1.sayName();                // 'rose'

var instance2 = new SubType('chester');
console.log(instance2.colors);      // [ 'red', 'blue' ]
instance2.sayName();                // 'chester'
```