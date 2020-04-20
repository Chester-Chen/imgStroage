## Array原型写入swap


```js
/**
 * @description 将swap写入Array 的原型，供实例调用,若数组的前一项大于后一项，则交换两个的值
 */
Array.prototype.swap = function (i, j) {
    let temp = this[i];
    this[i] = this[j];
    this[j] = temp;
}

```


## 冒泡排序


```js

/**
 * @param  {Array} arr 接收一个数组
 */
function bubbleSort(arr) {
    for(let i = arr.length; i > 0; i--) {
        for(let j = 0; j < i -1; j++) {
            if (arr[j] > arr[j+1]) {
                arr.swap(j, j+1);
            }
        }
    }
    return arr;
}

let num = [2,4,1,56,2,1,34,0,21];

console.log(`冒泡前：${num}`);	// 冒泡前：2,4,1,56,2,1,34,0,21
console.log(`冒泡后：${bubbleSort(num)}`);	// 冒泡后：0,1,1,2,2,4,21,34,56


```

## 快速排序

```js
let num = [2,4,1,56,2,1,34,0,21];

/**
 * @param  {Array} arr
 */
console.time('start')
function quickSort(arr) {
    // 递归终止条件
    if (arr.length < 2) {
        return arr;
    }

    let baseIndex = 0;
    let left = [];
    let right = [];

    for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[baseIndex]) {
            left.push(arr[i]);
        } else {
            right.push(arr[i]);
        }
    }

    left = quickSort(left);
    right = quickSort(right);

    return left.concat(arr[baseIndex], right);
}gu


console.log(`快排前：${num}`);  // 快排前：2,4,1,56,2,1,34,0,21
console.log(`快排后：${quickSort(num)}`);   // 快排后：0,1,1,2,2,4,21,34,56


```

## 归并排序

```js
/**
 * 
 * @title  归并排序
 * @description     将数组分割成两个数组，直至只有一个元素，最后合并
 * 
 */
function mergeSortedArray(arrA, arrB) {
    var result = [];
    var i = 0,
        j = 0,
        arrALen = arrA.length,
        arrBLen = arrB.length;
    while (i < arrALen && j < arrBLen) {
        if (arrA[i] < arrB[j]) {
            result.push(arrA[i++]);
        } else {
            result.push(arrB[j++]);
        }
    }
    while(i < arrALen) {
        result.push(arrA[i++]);
    }
    while(j < arrBLen) {
        result.push(arrB[j++]);
    }
    return result;
}


function mergeSort(arr) {
    if (arr.length === 1) { 
        return arr;
    }
    var mid = Math.floor(arr.length / 2);
    var left = arr.slice(0, mid);
    var right = arr.slice(mid);

    /**
     * @description 
     * 此处递归调用 mergeSort，对于本身无序的数组，不能保证
     * 第一次切割的两个数组有序，所以利用分而治之的办法，将切割的两个子数组
     * 继续切割成两个子数组，直至切割的子数组只有一个元素时，因为一个元素的
     * 数组是有序的，然后根据 mergeSort 的递归终止条件，将把两个只有一个元素的
     * 数组返回给 mergeSortedArray， 
     */
    return mergeSortedArray(mergeSort(left), mergeSort(right));
}


console.log(mergeSort(num));    // [ 0, 1, 1, 2, 2, 4, 21, 34, 56 ]
```

## 二分查找

```js

/**
 * @description     二分查找递归实现，若传入的数组无序，则采用上面的排序方法，进行排序
 * @param {*} searchValue 查找的值
 * @param {Array} arr 需传入有序数组，
 * @param {Number} start 开始查找的位置
 * @param {Number} end 结束查找位置
 */
let binaryArray = [ 0, 1, 1, 2, 2, 4, 21, 34, 56 ];
function binarySearch(searchValue, arr, start, end) {
    var start = start || 0;
    var end = end || (arr.length) - 1;
    var mid = Math.floor((start + end) / 2);

    if (searchValue === arr[mid]) {
        return mid;     // 若找到则返回中间索引
    } else if (searchValue > arr[mid]) {
        return binarySearch(searchValue, arr, mid +1, end);
    } else {
        return binarySearch(searchValue, arr, start, mid -1);
    }
    return false;   // 没有找到返回 false
}

console.log(`查找值的索引为：${binarySearch(21, binaryArray)}`);    // 查找值的索引为：6

/**
 * @description 二分查找非递归实现
 * @param {*} searchValue 需查找的值
 * @param {Array} arr 有序数组
 */
function binarySearch(searchValue, arr) {
    let start = 0;
    let end = arr.length - 1;

    while (start <= end) {
        let mid = Math.floor((start + end) / 2);
        if (searchValue === arr[mid]) {
            return mid;
        } else if (searchValue < arr[mid]) {
            end = mid -1;
        } else {
            start = mid + 1;
        }
    }
    return false;
}

console.log(`查找值的索引为：${binarySearch(21, binaryArray)}`);    // 查找值的索引为：6


```











