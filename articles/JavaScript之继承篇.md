date：2020.01.13

构造函数内含有`prototype`属性，该属性指向构造函数的原型对象。所有原型对象都有一个`constructor`，该属性指向`prototype`所在的构造函数。构造函数的实例，其`__proto__`属性指向构造函数的原型对象。

补充（ 2020.05.12）：
在之前的笔记中（也就是下文中所提及的），由于后面的学习中，发现关于继承代码部分，存在一些差异和不足，这里放在开头提示自己。

```js
// 下文中均是通过new实现继承，如下行代码
SubType.prototype = new SuperType();	// 继承SuperType

```
上面采用`new SuperType()`，会创建一个`SuperType`的新实例，该实例会指向`SuperType`的原型对象。通过重写`SubType`的原型对象并抛弃原有的原型对象（抛弃的对象会被GC回收）来实现继承。但是今日看书得知，这种实现方法，能基本满足需求，但会有副作用，具体的副作用，老夫暂时冇得头绪。


接下来介绍ES6新方法
```js
// a
Bar.ptototype = Object.create( Foo.prototype );

// b、 开始可以直接修改现有的 Bar.prototype
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```
在ES6中，a 方法会抛弃原本的`Bar.prototype`，会带来轻微的性能损耗，个人感觉无伤大雅。

在 b 方法中，可以使用`Object.setPrototypeOf(obj, prototype)`直接替换原型对象



# 1、原型链
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

## 1.1 谨慎定义方法

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


## 1.2 原型链的问题
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

# 2. 借用构造函数
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

# 3. 组合继承
组合继承是指，将原型链和借用构造函数组合。使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承。
```javaScript
function SuperType(name) {
    this.name = name;
    this.colors = ['red', 'blue'];
}

function SubType(name) {
    SuperType.call(this, name);     // 第二次调用SuperType
}

SuperType.prototype.sayColors = function() {
    console.log(this.colors);
}

// 继承SuperTyope
SubType.prototype = new SuperType();    // 第一次调用SuperType
SubType.prototype.constructor = SubType;
SubType.prototype.sayName = function () {
    console.log(this.name);
}

var instance1 = new SubType('rose');
instance1.colors.push('yellow');
console.log(instance1.colors);      // [ 'red', 'blue', 'yellow' ]
instance1.sayName();                // 'rose'
instance1.sayColors();              // [ 'red', 'blue', 'yellow' ]

var instance2 = new SubType('chester');
console.log(instance2.colors);      // [ 'red', 'blue' ]
instance2.sayName();                // 'chester'
```

第一次调用SuperType构造函数时，SubType.prototype会得到两个属性：name和colors，它们都是SuperType的实例属性，只不过现在位于SubType的原型中。当调用SubType构造函数时，又会调用一次SuperType的构造函数，这一次又在新实例上创建了属性name和colors。所以，这两个属性屏蔽了原型中的同名属性。如图。

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.13/04.png?raw=true)


# 4. 原型式继承

```javaScript
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}

var person = {
    name: 'xxx',
    friends: ['Rose', 'Bob']
};

var p1 = object(person);
p1.name = 'Chester';
p1.friends.push('Aron');

var p2 = object(person);
p1.name = 'Jack';
p1.friends.push('Toast');

console.log(person.friends);        // [ 'Rose', 'Bob', 'Aron', 'Toast' ]
```

原型式继承要求必须有一个对象可以作为另一个对象的基础。在有这么一个对象的基础上，把该对象`person`传入`object()`函数中，并返回新对象。这个新对象将`person`作为原型，所以这个新对象的原型中就包含一个基本类型和引用类型值的属性。所以`friends`会被所有实例共享。

在ES5中，新增的`Object.create()`方法规范化了原型式继承。该方法接受两个参数：
- 一个用做新对象原型的对象
- 一个为新对象定义额外属性的对象(会覆盖原型上的同名属性)

**第二个参数与`Object.defineProperties()`方法的第二个参数格式相同**


所以上面的例子可以改为

```javaScript
var person = {
    name: 'xxx',
    friends: ['Rose', 'Bob']
};

var p1 = Object.create(person);
p1.name = 'Chester';
p1.friends.push('Aron');

var p2 = Object.create(person);
p1.name = 'Jack';
p1.friends.push('Toast');

var p3 = Object.create(person, {name: {value: 'BOT'}});
console.log(p3.name);               // 'BOT'

console.log(person.friends);        // [ 'Rose', 'Bob', 'Aron', 'Toast' ]

```

# 5. 寄生式继承

与原型继承相似，即创建一个仅用于封装继承过程的函数，该函数内部以某种方式来增强对象，然后返回对象。

```javaScript
function createAnother(original) {
    var clone = object(original);    // 原型式继承里的object()方法
    clone.sayHi = function () {     // 增强对象
        console.log("hi");
    }
    return clone;                   // 返回这个增强对象
}

var person = {
    name: "Chester",
    friends: ['Rose', 'Aron']
};

var anotherPerson = createAnother(person);

anotherPerson.sayHi();      // 'hi'
```

新对象`anotherPerson`不仅有`person`的所有属性和方法，还有自己的`sayHi()`方法。

# 6. 寄生组合式继承
我们无非就是需要一份父类的原型副本，通过寄生式继承来继承父类的原型，然后再将结果指定给子类的原型。说白了，就是拿到父类原型，赋值给子类原型。

```javaScript
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}

function inheritPrototype(subType, superType) {
    var prototype = object(superType.prototype);
    prototype.constructor = subType;        // 弥补因重写原型而失去的默认的constructor属性
    subType.prototype = prototype;          // subType的原型对象指定对象
}

function SuperType(name) {
    this.name = name;
    this.colors = ['red', 'blue'];
}

SuperType.prototype.sayName = function () {
    console.log(this.name);
}

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function () {
    console.log(this.age);
}

// 测试部分
var test = new SubType('test', 11);
console.log(test.name);         // 'test'
test.colors.push('pink');
console.log(test.colors);       // [ 'red', 'blue', 'pink' ]

var test2 = new SubType('test2', 21);
console.log(test2.name);        // test2
console.log(test2.colors);      // [ 'red', 'blue' ]
```

这种继承方式高效率体现在，它只调用了一次`SuperType`构造函数，并且避免了在`SubType.prototype`上面创建多余的的属性、原型链还能保持不变，因此能正常使用`instanceof`和`isPrototypeOf()`。