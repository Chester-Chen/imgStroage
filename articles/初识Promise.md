date: 2020.04.13


## Promise 对象

Promise 实现了异步操作的同步写法，不用苦逼层层嵌套 callback.....  

promise  有3种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

promise 有以下两个特点：
- 对象的状态不受外界影响
- 一旦状态改变，就不会在变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为` resolved`（已定型）。


Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

### 1、基本用法

`Promise` 构造函数接受一个函数作为参数，该函数有两个参数`resolve` 和 `rejected` ，且它们都是函数。这两个参数由 js 引擎提供，不用自行部署。

- `resolve` 的作用是将 `Promise` 的实例对象的状态由 `pending` 转为`fulfilled`
- `reject`  的作用是将 `Promise` 的实例对象的状态由 `pending` 转为`rejected`

```js
const promise = new Promise((resolve, reject) => {
    console.log('promise');
    // resolve('success');
    reject('failed');
})

promise.then((value) => {	// 成功调用
    console.log('success:', value);
}, (err) => {	// 失败调用
    console.log('err:', err);
})

```
当 `promise` 对象调用then方法时，会根据 `promise` 的状态执行相应的函数


下面来看个模拟异步的操作。

```js
function timeout(ms) {
    return new Promise((resolve, reject) => {
        console.log('test1');
        setTimeout(resolve, ms, 'fulfilled');
    })
}

timeout(3000).then(value => console.log(value));

console.log('test2');
```

上面说到过，`Promise` 新建后会立即执行，所以，输出为` 'test1 -> test2 -> fulfilled'` 。`timeout `函数返回一个 `promise` ，不难看出，代码在3秒后输出` ‘fulfilled’`，该` promise` 会在3秒后将状态转为 `fulfilled` ，所以在运行到 `timeout(3000).then(value => console.log(value));` 这行时，会先执行后面的同步任务，等状态改变后在回去调用 `.then()` 方法。

上面也有提到，当调用 `resolve` 和 `reject` 函数带有参数时，该参数会传递给回调函数。reject 函数的参数通常是传递 错误信息。`resolve `除了传递的是正常的值外，还可能是一个`Promise`实例，如：

```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('success!');
        console.log('2');
    }, 3000);
})

const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(promise1);
        console.log('1');
    }, 1000);
})

promise2.then(value => {
    console.log(value);
})

// 1
// 2
// success!
```

该段代码中，`promise2`的状态在1秒后改变为` resolve `，但是`resolve` 方法返回的是 `promise1`，是一个`Promise`实例，，这就导致`promise2 `的状态失效，其状态由`promise1` 决定，。而 `promise1` 又在过了2秒后 状态改变为`resolve` 且返回信息 `'success!'` 所以，后面的` then` 语句 都是针对`promise1` 的状态的。

然后还要注意的一点是，在调用 resolve() 后，后面的 `console.log()` 语句会继续执行，**且会首先打印出来。这是因为立即 `resolved `的 `Promise` 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。**所以说应该在`promise`中`return` 出状态，其后续操作统一放到`then` 语句中执行。


### 2、Promise.prototype.then()

```js

let p = new Promise((resolve, reject) => {
	// ...
})
p._proto_	// Promise {Symbol(Symbol.toStringTag): "Promise", constructor: ƒ, then: ƒ, catch: ƒ, finally: ƒ}

```
该段代码在浏览器运行结果，可以清晰看到，Promise的实例对象，具有`then`方法，也就是说`then`方法是写在原型对象`Promise.prototype`上的。可以看到原型上，默认定义了`then` 、`catch`、`finally`这3个方法。所以只要是Promise对象的派生都能根据原型链继承这3个方法。。

`p.then(onFulfilled[, onRejected]);` 
最多接受两个参数，这两个分别为Promise的成功和失败情况的回调函数，如下：

- onFulfilled（optional），当Promise 的状态变为 `fulfilled`时调用。如果该参数不是函数，则会在内部被替换为（x） => x, 即原样返回 Promise 最终结果的函数。
- onRejected（optiona），当Promise 的状态变为 `rejected`时调用.如果该参数不是函数，则会在内部被替换为 `Thrower` 函数，即抛出异常。

`then()` 返回值
`then` 方法返回的是一个**新的** Promise 实例。根据这种特性，我们就可以优雅的使用`then` 的链式写法了，即`then().then().then()......`

```js
getData('/api/data.json').then(
	data => getData(data.url)
).then(
	resolved => console.log('resolved: ', resolved),
	rejected => console.log('rejected: ', rejected)
)
```
利用链式的写法，可以自主按照一定的次序调用回调函数。如上代码，第二个then 会等待第一个 then 的状态改变才会被调用。


### 3、Promise.prototype.catch()

catch(onRejected) 与 Promise.prototype.then(undefined, onRejected) 相同

- onRejected
当Promise 被rejected时,被调用的一个Function。 该函数拥有一个参数：reason  ，为rejection的原因。

如果 onRejected **抛出一个错误或返回一个本身失败的 Promise** ，  通过 catch() 返回的Promise 被rejected；否则，它将显示为成功（resolved），这里要注意的是，catch() 返回的是一个新的 Promise。

```js
  const promise = new Promise(function (resolve, reject) {
      throw new Error('test');
  });
  promise.catch((error) =>
      console.log(error)    // Error: test
  );
  // 等价于
  promise.then(undefined, (error) =>
      console.log(error)    // Error: test
  );
```
上面代码中，如果promise 抛出错误，则会被catch() 方法的回调捕获。Promise 对象的错误具有`‘冒泡’` 性质，会一直向后传递，直至被捕获，也就是错误总会被下一个catch 语句捕获。

还有一点就是，建议不要使用 then() 方法里定义的 rejected 状态调用的函数，而总是使用 catch 方法，即，
```js
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// 建议使用这种写法
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
因为第二种写法接近于同步写法，在阅读上更美观清晰、可以捕获前面 then() 方法抛出的错误。且 Promise 对象抛出的错误不会传递到外层代码，即发生错误不会有任何反应，进程也不会中断。

```js
let doSomething = function() {
    return new Promise((resolve, reject) => {
        resolve(x + 2);
    })
}

doSomething().then((value) => {
    console.log('do something');
})

setTimeout(() => {
    console.log('123');
}, 3000);
// Uncaught (in promise) ReferenceError: x is not defined
// 123
```
在浏览器运行代码后，可以看见，`x` 没有定义，抛出错误，但是进程并没有终止，3秒后，还是输出了`123`, 所以 Promise 会吃掉错误，不会影响到外部代码。

### 4、Promise.prototype.finally()

ES2018 引入的新标准，`finally()` 方法不管最后 `Promise` 对象的状态如何，都会执行该方法。该方法不做过多解释了，较为简单。

`finally()` 方法的回调不接受任何参数，这就告诉问们，该方法与状态无关，不依赖于 Promise 的结果。

```js
promise
.finally(() => {
  // 语句
});

// 等同于
promise
.then(
  result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```
`finally()` 本质上是 `then` 的特列，使用 `finally` 就不用写成功与失败的回调了。

### 5、Promise.all()

`Promise.all()` 用于将多个 `Pormise` 对象实例，包装成一个新的 `Promise `实例。
`const p = Promise.all([p1, p2, p3]);`

上面代码中的 `all()` 方法，接受一组由 Promise 实例组成的数组作为参数，如果参数不是 Promise 实例，就会调用 `Promise.resolve()` 方法，将参数转为 Promise 实例。`Promise.all()` 方法的参数可以不是数组，但必须具有 `Iterator` 接口，且返回的每个成员都是 Promise 实例。

p 的状态由 p1, p2, p3 决定：
类似于`&&` 操作符
- 只有 ` p1`, `p2`, `p3` 的状态都为 `fulfilled` 时，p 的状态才会变成 `fulfilled` ，此时，` p1`, `p2`, `p3` **的返回值**组成数组，传递给p的回调函数。
- 只要  `p1`, `p2`, `p3` 中有一个的状态为 `rejected` ，p 的状态就变成 `rejected` ，这个第一个被 `rejected` 的实例的返回值，会传递给p的回调函数。

### 6、Promise.any()

`Promise.any()` 与 `Promise.all()`相似，同样是接受一个数组作为参数，包装成一个新的 Pormse 实例。
`any()` 类似`||`操作符。

- 只要参数实例中由一个变为 `fulfilled` 状态，返回的包装实例，就会变成 `fulfilled` 状态，
- 如果所有参数都为`rejected` 状态， 包装实例就会变成`rejected` 状态

`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是不会因为某个` Promise `变成`rejected`状态而结束。

### 7、Promise.race()

`race` 这个单词译过来为`赛跑、竞争`，正如这个方法的特征一样，谁变化的快，就取谁的状态。`const p = Promise.race([p1, p2, p3]);`

`p1`,` p2`,` p3` 中有一个实例率先改变状态， `p` 的状态就跟着改变，同时那个率先改变的实例的返回值，传递给`p `的回调函数。

### 8、Promise.resolve(value)

如果value不为Promise对象，该方法可以将对象转化为 Promise 对象。

例如，jQuery中返回的 `deffered` 对象，转化为一个新的Promise 对象。
```js
Promise.resolve('test')
// 等价于
new Promise(resolve => resolve('test'))
```
总结为以下2点：
- value 为 Promise 对象，那么返回该Promise对象
- value 是一个`thenable` 对象，即`thenable`对象中含有 `then` 方法。eg.
```js
let thenable = {
  then: function(resolve, reject) {
    resolve('test');
  }
};
```
`Promise.resolve`方法会将这个对象转为` Promise `对象，并且立即执行`thenable`对象的`then`方法。
- 参数既不是 `thenable`或根本不是对象的情况，eg.`const p = Promise.resolve('test');` ，这将直接返回一个新的状态为`resolved`的Promise ，同时将`test`字符串传递给回调函数。
- 不带任何参数，将直接返回状态为`resolved` 的`Promise`


### 9、Promise.reject(reason)

`Promise.reject(reason)` 该方法会返回一个状态为`rejected`的 Promise 实例。

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了

```

有一点要注意的是，`` 该方法的参数，会缘分不动的作为 `` 的理由，作为后续链式操作的参数，这一点与`Promise.resolve(value)`不同。

```js
const thenable = {
  then(resolve, reject) {
    reject('出错了');
  }
};

Promise.reject(thenable)
.catch(e => {
  console.log(e)	// { then: [Function: then] }
  console.log(e === thenable)	// true
})
```
上面代码中，`Promise.reject(reason)` 的参数为 `thenable` ，且后续执行中的`console.log(e === thenable)` 返回为 `true` ，即`catch` 里的错误参数`e`是与 `thenable`对象全等的。


至此已经大体上了解了Promise这个对象基本方法，这篇笔记大多都采用阮一峰的es6案例。好记性不如烂笔头，从头到尾敲一遍，还是获益良多的。