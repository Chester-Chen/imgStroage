date: 2020.04.05 

## 1、回文数判断

```js
/**
 * @description 反转字符实现
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function (x) {
    if (x < 0) {
        return false;
    } else if( String.prototype.split.call(x, '').reverse().join('') == x ){
        return true;
    }
    return false;
};

/**
 * @description 双指针夹逼法
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function (x) {
    if (x < 0) {
        return false;
    }
    let str = x.toString();
    let i = 0,
        j = str.length - 1;

    while(i < j) {
        if(str[i] != str[j]) {
            return false;
        }
        i++;
        j--;
    }
    
    return true;
}

```

## 2、回文链表判断

```js
/**
 * @description 取中间值，反转前半部分后与后半部分比较
 * @param {number} x
 * @return {boolean}
 * 
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

var isPalindrome = function (head) {
    if (head === null || head.next === null)
        return true;
    let mid = head;
    let pre = null;
    let reverse = null;
    // 取中间值
    while (head !== null && head.next !== null) {
        // 在mid 前进前取得其位置
        pre = mid;
        mid = mid.next;
        head = head.next.next;
        // 反转前半部分
        pre.next = reverse;
        reverse = pre;
    }
    // 如果链表数目为奇数，则抛弃对称点
    if (head) mid = mid.next;

    while (mid) {
        if (reverse.val !== mid.val)
            return false;
        reverse = reverse.next;
        mid = mid.next;
    }
    return true;
}


/**
 * @description 复制链表至数组，利用夹比比较
 * @param {ListNode} head
 * @return {boolean}
 * 
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
var isPalindrome = function (head) {
    let arr = [];
    while(head) {
        arr.push(head.val);
        head = head.next;
    }
    while(arr.length > 1) {
        if(arr.pop() != arr.shift()) return false; // 掐头掐尾比较
    }
    return true;
}
```

## 3、 删除链表的倒数第N个节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @description 两次遍历实现。第一次遍历取得链表长度，即删除弟 `length - n + 1` 处的节点
 * 				第二次遍历至删除前一位 `first.next = first.next.next;`即可
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function (head, n) {
    let first = head;
    let preHead = new ListNode(-1);
    preHead.next = head;
    let length = 0;
    while (first) {
        length++;
        first = first.next;
    }
    first = preHead;
    length -= n;
    while(length != 0) {
        length--;
        first = first.next;
    }
    first.next = first.next.next;
    return preHead.next;
};

/**
 * @description 双指针实现
 * 当链表总长度是k时，如果要删除倒数第n个节点(假设n小于k)，那么首先要找到第k-n个节点，k-n这个节点  就是要删除的节点的前一个节点。当找到k-n这个节点就好办了，直接将k-n的next指针指向下下一个节点即可。我们需要两个指针a和b。b指针先走n步，接着a和b指针同时往前走，当b指针走到链表末尾时，a指针就正好走到要删除的节点的前一个位 置了，最后a节点的next指针指向下下一个节点，就可以完成删除操作了。
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let preHead = new ListNode(-1); // 设置哨兵
    preHead.next = head;
    let slow = preHead;
    let fast = preHead;
    // 设置快慢指针步长
    while(n--) {
        fast = fast.next;
    }
    while(fast.next != null) {  // 当快指针走到末尾时，慢指针指向被删除节点的前一个节点
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return preHead.next;
};

感觉双指针的方案更舒服，也不用遍历两次。

```

## 4、无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的 `最长子串` 的长度。

示例:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。


```js
/**
 * @description 滑动窗口(队列)实现
 * 			   其实就是一个队列,比如字符串 abcbd，进入这个队列（窗口）为 abc 满足题目要求，当再进入 b，队列变成了 abcb，这时候*    			   不满足要求。所以，我们要移动这个队列！如何移动？我们只要把队列的左边的元素移出就行了，直到满足题目要求！一直维持这样的队			     列，找出队列出现最长的长度时候，求出解！
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let res = 0,
        i = 0;
    let que = [];  // 队列
    while(i < s.length) {
        if(que.indexOf(s[i]) == -1) {
            que.push(s[i]);
        } else {
            que.shift();
            continue;
        }
        res = Math.max(res, que.length);
        i++;
    }
    return res;
};
```

<img src="D:\desktop\Desktop\滑动窗口.jpg"  />

## 5、判断字符串是否存在回文串

```js

每个字符出现的次数为偶数, 或者有且只有一个字符出现的次数为奇数时, 是回文的排列; 否则不是.
/**
 * @param {string} s
 * @return {boolean}
 */
var canPermutePalindrome = function(s) {
    let a = new Set();
    for(let i of s) {
        a.has(i) ? a.delete(i) : a.add(i);
    }
    return a.size <= 1;
};

```