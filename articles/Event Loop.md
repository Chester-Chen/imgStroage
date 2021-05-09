date: 2020.04.14

## Event Loop 浏览器与Node.js 环境差异整理

### 1、前言
前几天看完 Promise 后，接触到了许多陌生的名词，什么微任务、宏任务、什么任务队列啥的。对于Promise的执行顺序也不是很清楚，后来了解到是与event loop 有关 。下面来整理以下浏览器环境及Node.js环境中的 Event Loop。

下面讨论的是浏览器环境中的 Evnet Loop ，与 Node.js 中的 Event Loop 存在差异。

不多说了，先来看一个栗子。

```js
setTimeout1(()=>{
	console.log('timer1')

    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)

setTimeout2(()=>{
	console.log('timer2')
	Promise.resolve().then(function() {
    	console.log('promise2')
	})
}, 0)

Promise.resolve().then(function() {
    console.log('promise3')
})
```

Browser output：`promise3 -> timer1 -> promise1 -> timer2 -> promise2 `
Node.js output：`promise3 -> timer1 -> timer2 -> promise1 -> promise2 `

小朋友发出了纳尼？的疑问。别急，下面逐步解析。

下面内容均为浏览器环境展开分析，Node 环境会提示您！
下面内容均为浏览器环境展开分析，Node 环境会提示您！
下面内容均为浏览器环境展开分析，Node 环境会提示您！

### 2、 同步任务与异步任务

先来了解同步任务与异步任务。图是网上盗的！图是网上盗的！图是网上盗的！

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.04.14/a&s.jpg?raw=true)

可以看到任务被分为同步任务和异步任务，分别进入不同的环境。

- 同步任务进入主线程，异步任务 进入 Event Table 并注册任务中的函数。

- 指定的任务完成时， Event Table 会将注册的函数移入 Event Queue (事件队列)

- 当主线程内的任务执行完时，会调用 Event Queue 中的函数进入主线程执行

上诉步骤往返循环。

```js
$.ajax({
    url: www.xxx.com,
    data: xxx,
    success: () => {
        console.log('xxx!');
    }
})
console.log('代码执行结束');
```
上面代码执行一段 Ajax 请求，简单分析下执行流程：

- `ajax` 进入异步任务，` Event Table `注册 `success`回调函数，
- 执行同步任务，`console.log('代码执行结束');`
- `ajax` 事件完成，回调函数`success`进入`Event Queue`。
- 主线程任务为空，从` Event Queue` 中读取回调函数 `success` 执行

### 3、setTimeout

```js
function show() {
	console.log('1');
}
setTimeout(() => {
	show();
}, 1000);

sleep(20000); // 此函数自行定义，目的是延迟进程执行
```

上面代码你会发现，`1` 并没有如期输出。

当`show()` 进入`Event Table` 注册并注册，然后开始挤时间。由于`sleep() `很慢，所以1秒后，`setTimeout()` 计时完成，`show() `函数推入 `Event Queue` 。当sleep 运行完时， `show() `函数终于可以进入主线程执行了。这里我们注意到，若当主线程的执行时间过长时，就会导致，并不能在如期的时间内执行异步回调。

`setTimeout(fn, 0)` 那么对于这种，0秒执行的情况，是否能立刻执行呢。答案是不能的。这段代码的意思是，在主线程同步任务执行完的情况下，立马执行`fn` 。这里要注意的是，即使主线程开始就为空，也不能`0 ms`执行，根据H5标准，最低是`4ms`。对于这个标准我找不到在看的，我也时看别人的文章才了解到的，呜呜呜~，如果你看到了，麻烦告诉我。0_0。

所以说 `setTimeout` 这个函数并不是说在指定的间隔后，就能执行的。而是在指定的时间后，将注册函数推入 `Event Queue `，等待主线程执行栈内的同步代码执行完毕。

根据阮一峰文章里的说法：执行栈中的代码（同步任务），总是在读取"任务队列"（异步任务）之前执行；

### 4、setInterval

`setInterval(fn, ms)`是每隔多少ms执行一次，根据上面的 `setTimeout` 的分析，我们能知道，并不是每隔ms执行一次` fn` ，而是 每隔 `ms `将`fn` 推入`Event Queue `一次。如果说，回调函数 `fn`  的执行时间大于间隔时间，那根本就察觉不到延迟的存在。

### 5、宏任务与微任务

对于前面讲到的同步任务和异步任务，来做更精确的划分。即为宏任务与微任务。

- macrotask：包含执行整体的js代码，事件回调，xhr，定时器（setTimeout/setInterval/setImmediate），IO操作，UI render
- microtask：更新应用程序状态的任务，包括promise回调，MutationObserver，process.nextTick，Object.observe

与同/异任务一样，宏/微任务会进入到对应的执行环境。如图。

图是盗的！图是盗的！图是盗的！

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.04.14/task.jpg?raw=true)

配合上面的图来分析一波这段代码。
```js
setTimeout(function() {
    console.log('1');
})

new Promise(function(resolve) {
    console.log('2');
}).then(function() {
    console.log('3');
})

console.log('4');

```
首先进入整体代码(宏任务)，这段代码作为宏任务进入主线程，
第一个遇到的是`setTimeout`，将其回调函数注册后推入宏任务`Event Queue` 。
然后，遇到`Promise`，`new Promise(fn)`执行，`then`方法推入微任务的`Event Queue`。
最后，遇到`console.log('4');`，直接执行。
整体代码作为第一个宏任务执行结束。根据上图，每次宏任务结束都应该执行所有的微任务，发现微任务的`Event Queue`中有`then`方法还没有执行，所以，执行。
第一轮事件循环结束。开始第二轮循环。
从宏任务的`Event Queue`中，发现还有`setTimeout`的回调函数没有执行，立即执行。
结束了。

对于浏览器下的 `Event Loop` 应该能理解的差不多了。

不知道你发现没有，在每一次新的循环开始，只会从宏任务`Event Loop`中取一个宏任务执行。

##  Node 环境

对于`Node` 环境下，每一次新的循环会读取所有的宏任务队列。
这是文章开头的代码段，这里用`node`环境分析一遍，看看自己是不是作对了呢。

```js
setTimeout1(()=>{
	console.log('timer1')

    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)

setTimeout2(()=>{
	console.log('timer2')
	Promise.resolve().then(function() {
    	console.log('promise2')
	})
}, 0)

Promise.resolve().then(function() {
    console.log('promise3')
})
// `promise3 -> timer1 -> timer2 -> promise1 -> promise2`
```

第一次循环，`setTimeout1` 和 `setTimeout2` 进入宏任务队列，`then` 推入微任务队列，第一次宏任务结束，清空微任务队列，立即执行` then `输出 `promise3` 。此时宏任务：`setTimeout1`、`setTimeout2`。微任务：空。

这里与浏览器中有所不同，第二次循环，读取宏任务队列中的所有任务，执行其中的同步代码，并把微任务推入微任务队列。此时宏任务：空。微任务：`promise1`、`promise2`.

第二次宏任务结束，检擦微任务队列，并执行清空。输出 `promise1` 和 `promise2`

这里要声明的一点是，对于node 环境下 ，我这里只是介绍了初略的分析，实际上的流程有些不同，具体如下：
```
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘

```

1、timers阶段
	阶段包括setTimeout()和setInterval()
2、IO callbacks
	大部分的回调事件，普通的caollback
3、poll阶段
	网络连接，数据获取，读取文件等操作
4、check阶段
	setImmediate()在这里调用回调
5、close阶段
	一些关闭回调，例如socket.on(‘close’, …)


微任务队列追加在process.nextTick队列的后面，就是说 process.nextTick 会优于其他微任务先执行

Node 环境下的分析以后再写，这里只记录一些大体的知识点，，得赶论文了。。。。

2020.04.20最后修改。