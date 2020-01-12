#  1、属性类型
ES中有两种属性：数据属性和访问器属性。

## 数据属性
数据属性4个描述行为的特性：
- [[Configurable]]: 能否通过delete删除属性或者能否修改属性。默认true
- [[Enumerable]]：能否同通过for-in循环返回属性。默认true
- [[Writable]]：能否修改属性的值。默认true
- [[Value]]：改属性的数据值。默认undefined

`Object.defineProperty(obj, prop, descriptor)`  
- obj要在其上定义属性的对象。
- prop要定义或修改的属性的名称。
- descriptor将被定义或修改的属性描述符。

修改属性默认特性，必须使用ES5中的`Object.defineProperty() `方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。  

```javaScript
var test = {
    name : 'chester',
    age: 23,
    sayName: function() {
        console.log(this.name)  
    }
}
console.log(test.name);     // 'chester'

Object.defineProperty( test, 'name', { value:'rose' });

console.log(test.name);     // 'rose'
```
也就是修改或新增对象属性， 其他的特性值雷同， 不一一列举了。


## 访问器属性
访问器属性4个描述行为的特性：
- [[Configurable]]: 能否通过delete删除属性或者能否修改属性。默认true
- [[Enumerable]]：能否同通过for-in循环返回属性。默认true
- [[Get]]：在读取属性时调用。默认undefined
- [[Set]]：在写入属性时调用。默认undefined


```javaScript
var person = {
    name: 'chester',
    age_: 20,
    flag: 1
};

Object.defineProperty(person, 'age', {
    get: function() {
        return this.age_;
    },
    set: function(newAge) {
        this.age_ = newAge;
        this.flag = 0;
    }
});

console.log(perosn.age);        // 20


person.age = 99;
console.log(perosn.age_);        // 99
console.log(person.flag);        // 0
```

# 2、工厂模式
工厂模式通过用函数封装以特定接口创建对象的细节，并返回对象。
```JavaScript
function personFactory(name, age) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.sayName = function() {
        console.log(this.name);
    }
    return o;
}

var person1 =  personFactory('chester', 21);
var person2 =  personFactory('Rose', 22);

```
工厂模式解决了创建多个相似对象的问题，但是没能解决对象识别的问题，即不知道对象的类型。

# 3、构造函数模式
```JavaScript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.sayName = function() {
        console.log(this.name);
    }
}

var person1 = new Person('chester', 21);
var person2 = new Person('Rose', 22);

```
与工厂模式不同的是，构造函数模式没有显示的创建对象，直接将属性和方法赋值给this。那么这个this指向的对象是谁呢？

创建一个新实例，需要用到new操作符，当创建新对象`person1`时， 将构造函数的作用域赋给了新对象(this指向了新对象)，接着执行构造函数中的代码，将属性及方法赋值给新对象。

```JavaScript
console.log(person1 instanceof Object);     // true
console.log(person1 instanceof Person);     // true
```
可以看到新创建的对象既是Object的实例又是Person的实例。

## 3.1 构造函数中的问题
在上面的例子中，`person1`和`perosn2`实例都包含了sayName()的方法，但是这两个方法不是同一个Function的实例。`console.log(person1.sayName == person2.sayName);   // false`, 所以不同实例上的同名函数是不等价的。

根据以上问题，创建了两个执行同样任务的Function实例显得都点多余。可以使用this对象，把函数定义转移到构造函数外部。
```JavaScript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.sayName = sayName;
}

function sayName() {
    console.log(this.name);
}

var person1 = new Person('chester', 21);
var person2 = new Person('Rose', 22);

```
这样一来，sayName包含的是一个指向函数的指针，`person1`和`person2`共享全局作用域中的同一个sayName()函数。解决了两个函数做同一件事，但是如果对象需要定义很多的方法，那么全局作用域下，就要定义非常多的全局函数，造成全局污染。

# 4、原型模式
我们创建的每个函数都有个prototype(原型)属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。使用原型对象的好处是，可以让所有对象实例共享它所包含的属性和方法。
```JavaScript
function Person(name, age) {
  
}

Person.prototype.name = 'Rose';
Person.prototype.age = 21;
Person.prototype.sayName = function() {
    console.log(this.name);
};

var person1 = new Person();
person1.sayName();      // 'Rose'

var person2 = new Person();  
person2.sayName();      // 'Rose'

console.log(person1.sayName == person2.sayName);        // true
```
我们看到`Person`构造函数是个空函数，我们将方法和属性都写到了`Person`的原型对象里了,所有`Person`的实例都会获得相同的属性和方法,  `person1`和`person2`都是同一组属性和同一个方法`sayName()`。

## 4.1 原型对象
无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个constructor(构造函数)属性，该属性指向prototype属性所在函数的指针。就上面的例子来说`Person.prototype.constructor`指向的是`Person`。

当调用构造函数创建一个新实例后，该实例的内部将包含一个指针`[[Prototype]]`，指向构造函数的原型对象，脚本中没有标准的方式访问`[[Prototype]]`，但在浏览器中每个对象都支持一个属性`__proto__`,该属性就是`[[Prototype]]`。要注意的是，这个指针之间的连接是存在于实例与构造函数的原型对象之间的，而不是存在于实例与构造函数之间。
ES5中提供了`Object.getPrototypeOf()`方法，在所有支持的实现中，返回`[[Prototype]]`的指。eg.
```javaScript
console.log(Object.getPrototypeOf(person1) == Person.prototype); // true

console.log(Object.getPrototypeOf(person1).name); // 'Rose'
```
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.12/01.png?raw=true)

每当代码读取对象的某个属性时，会首先从对象实例本身开始搜索，如果找到，则返回该属性的值，否则，继续搜索指针指向的原型对象。
```JavaScript
function Person(name, age) {
  
}

Person.prototype.name = 'Rose';
Person.prototype.age = 21;
Person.prototype.sayName = function() {
    console.log(this.name);
};

var person1 = new Person();
var person2 = new Person();  

person1.name = 'Chester';
console.log(person1.name);      // 'Chester'来自实例
console.log(person2.name);      // 'Rose'来自原型
```

## 4.2 原型的动态性
```JavaScript
function Person() {
  
}

var friend = new Person();
Person.prototype = {
    name: 'Chester',
    age: 33,
    sayName: function() {
        console.log(this.name);
    }
}

friend.sayName();       // error
```
我们在修改原型之前创建了实例`friend`, 修改原型之后调用`friend.sayName();`，发现报错: `sayName() not a function`。为什么会这样呢？

尽管可以随时为原型修改或添加属性和方法，但是如果重写了整个原型对象，就等于切断了构造函数与最初原型之间的联系。**谨记：实例中的指针仅指向原型，而不是指向构造函数。**
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.12/02.png?raw=true)
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.01.12/03.png?raw=true)

由上图可以看出，重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系，它们仍然引用的是最初的原型。说白了，就是重写原型后新建的实例，与重写前创建的实例不再共享一份原型。旧的用旧的，新的用新的。


# 5、组合使用构造函数模式和原型模式
组合上这两种模式，构造函数模式定义实例属性，原型模式定义方法和共享的属性。这样的好处是，每个实例都会有一份实例属性的副本，方法却是共用的。

```JavaScript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.friends = ['Chester', 'Rose', 'Aron']
}

Person.prototype = {
    constructor: Person,
    sayName: function() {
        console.log(this.name);
    }
}

var person1 = new Person('chester', 21);
var person2 = new Person('Rose', 22);

person1.friends.push('Jack');

console.log(person1.friends);       // ["Chester", "Rose", "Aron", "Jack"]
console.log(person2.friends);       // ["Chester", "Rose", "Aron"]

console.log(person1.friends == person2.friends);        // false
console.log(person1.sayName == person2.sayName);        // true
```
可以看到`person1`和`person2`中的实例属性互不干扰，但`sayName()`方法却是共享的，最大限度的节省了内存。

# 6、动态原型模式
根据上述组合的模式，我们还可以把原型封装到构造函数里，通过检查某个某个应该存在的方法是否有效，来决定是否需要初始化原型。
```JavaScript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.friends = ['Chester', 'Rose', 'Aron'];
    if(typeof this.sayName != 'function') {
        Person.prototype.sayName = function() {
            console.log(this.name);
        };
    }
}

var person1 = new Person('chester', 21);
person1.sayName();

```
这种模式，只会在sayName()方法不存在的时候，才会去初始化原型。这里对原型所作 修改，能够立即在所有实例中得到反映。

> 要注意的是，在使用动态原型模式时，不能使用字面量重写原型，因为如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与原型之间的联系。

# 7、寄生构造函数模式
```JavaScript
function Person(name, age) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.sayName = function() {
        console.log(this.name);
    }
    return o;
}

var person1 = new Person('chester', 21);
person1.sayName();    //'chester'

```
除了用new操作符把包装函数作为构造函数外，其他和工厂模式简直一毛一样。
> 该模式需要注意一点的是，返回的对象与构造函数或者与构造函数的原型属性之间没有关系。也就是说，构造函数返回的对象和在构造函数外部创建的对象没有不同。在有选择的情况下，不建议使用该模式。

# 8、稳妥构造函数模式
稳妥对象，就是指没有公共属性，而且其方法也不引用this的对象。稳妥构造与寄生构造不同的是：
- 新创建的对象的实例方法不引用this
- 不使用new操作符调用构造函数

```JavaScript
function Person(name, age) {
    var o = new Object();

    // 可以在此处定义私有变量和函数

    //
    o.sayName = function() {
        console.log(name);
    }
    return o;
}

var person1 = Person('chester', 21);
person1.sayName();    //'chester'

```