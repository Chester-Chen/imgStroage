date: 2021.05.06 2:03AM

## 浅拷贝简单实现

```js
function cloneShallow(source) {
    var target = {};
    for (var key in source) {
    	// Object.prototype.hasOwnProperty 会忽略继承来的属性检测
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            target[key] = source[key];
        }
    }
    return target;
}

// 测试用例
var a = {
    name: "muyiy",
    book: {
        title: "You Don't Know JS",
        price: "45"
    },
    a1: undefined,
    a2: null,
    a3: 123,
    a4: ['a', 'b']
}
var b = cloneShallow(a);

a.name = "高级前端进阶";
a.book.price = "55";

console.log(b);
// { 
//   name: 'muyiy', 
//   book: { title: 'You Don\'t Know JS', price: '55' },
//   a1: undefined,
//   a2: null,
//   a3: 123,
//	 a4: ['a', 'b']
//  }
```

根据打印的数据来看，`book` 属性的值是一个对象，所以在浅拷贝时并并不完美，在修改 `a` 里面 `book` 的 `price`值后，先前拷贝的 `b` 里的数据也随之改变了，这就需要深拷贝了。

上面是浅拷贝简单实现，在上面的基础上加上是否为对象的判断就可以用递归实现简单的深拷贝了。

## 深拷贝简单实现

何为深拷贝：浅拷贝 + 递归，浅拷贝时判断属性值是否为对象，若为对象则进行递归。

```js
function cloneDeep1(source) {
    var target = {};
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (typeof source[key] === 'object') {
                target[key] = cloneDeep1(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

// {
//   name: 'muyiy',
//   book: { title: "You Don't Know JS", price: '45' },
//   a1: undefined,
//   a2: {},
//   a3: 123,
//   a4: { '0': 'a', '1': 'b' }   // 这里明显不对了，数组没有做兼容
// }
```
上面就实现了简单的深拷贝，但同时也存在许多问题。

- 没有对传入的参数进行校验，传入 `null` 时应该返回 `null` 而不是 `{}` 
- 是否为对象的判断不严谨， `typeof null === 'object'` ， 
- 没有考虑数组的兼容

## 兼容数组情况

```js
typeof null //"object"
typeof {} //"object"
typeof [] //"object"
typeof function foo(){} //"function" (特殊情况)
```

判断是否为对象，需要考虑是否数组的情况

```js
function isObject(obj) {
	return typeof obj === 'object' && obj != null;
}
```

兼容数组写法如下。

```js
function cloneDeep2(source) {

    if (!isObject(source)) return source; // 非对象返回自身
      
    var target = Array.isArray(source) ? [] : {};
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (isObject(source[key])) {
                target[key] = cloneDeep2(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

// 使用上面测试用例测试一下
var b = cloneDeep2(a);
console.log(b);
// {
//   name: 'muyiy',
//   book: { title: "You Don't Know JS", price: '45' },
//   a1: undefined,
//   a2: null,
//   a3: 123,
//   a4: [ 'a', 'b' ]
// }
```