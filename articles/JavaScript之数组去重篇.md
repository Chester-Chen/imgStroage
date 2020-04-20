date: 2020.03.01

# JavaScript数组去重整理

- indexOf去重

```js
/**
 * 
 * 建立临时数组，迭代传入数组的数据，如果在临时数组中没有
 * 找到该元素，则推入数组
 * 
 * **/
function unique(arr) {
    let temp = [];
    for(let i = 0; i < arr.length; i++) {
        if(temp.indexOf(arr[i]) == -1) {    // 没找到相同元素
            temp.push(arr[i]);
        }
    }
    return temp;
}

var array = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(array));                 //  [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]

可以看到这种方法无法对`NaN`和`{}`去重
```

- 两个for循环嵌套去重

```js
/**
 * 利用嵌套循环，比较两个值，相等则利用splice删除arr中的元素
 * 
 * **/
function unique(arr) {
    for(let i = 0; i < arr.length; i++) {
        for(let j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j]) {     // 如果相邻的两个值相等，这删除第二个，
                arr.splice(j, 1);
                j--;    // 因为删除了一个数据，所以j自减，保持下一次比较时，是相邻的两个值相比较
            }
        }
    }
    return arr;
}

var array = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(array));                 // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]

可以看到这种方法也无法对`NaN`和`{}`去重
```

上面这两种方法都对`NaN`和`{}`无法去重，查阅资料才知道。`NaN`是非数字类型，任意字符`A`和`B`都是`NaN`，既然都是`NaN`，这两个字符当然不相等。

**还有就是{} 和 []**

[] 和 {} 都是属于引用类型，引用类型是存放在堆内存中的，而在栈内存中会有一个或者多个地址来指向这个堆内存相对应的数据。所以在使用 == 操作符的时候，对于引用类型的数据，比较的是地址，而不是真实的值。所以在使用 == 操作符的时候，对于引用类型的数据，比较的是地址，而不是真实的值。

 ```js
console.log({} === {}) // false     对象相比较时，会判断是否是同一个对象，若两个操作数指向同一个对象返回true，否则返回false
console.log([] === [])  // false
 ```

详细解释可以看`2020.03.02`日的文章

- sort() 去重

```js
function unique (arr) {
    if (!Array.isArray(arr)) {
        console.log('不是数组，类型错误');
    }
    arr.sort(); // 先将数组进行排序
    let temp= [arr[0]];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i-1]) {
            temp.push(arr[i]);
        }
    }
    return temp;
}

var array = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(array));     //  [0, 1, 15, NaN, NaN, "NaN", {…}, {…}, "a", false, null, "true", true, undefined]      NaN 和 {}没有去重

```

- reduce + includes 去重











# ES6去重

es6提供的新方法，代码量简短的不得了

- Set 去重

ES6 提供了新的数据结构 `Set`。它类似于数组，但是成员的值都是唯一的，没有重复的值。

`Set` 本身是一个构造函数，用来生成 `Set` 数据结构。

```js
var array = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];

var unique = [...new Set(array)]; 

console.log(unique);    // [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]   {}没有去重

```

这里也可以结合Array.from方法，可以将类数组对象或者可遍历的对象，包括，ES6的新数据结构，Set和Map都属于类数组，没有重复的值。
`function unique (arr) { return Array.from(new Set(arr)) }`

- Map 去重









