---
title: JS数组方法（ES5、ES6）
date: 2020-12-12 13:56:55
tags: array
categories: JavaScript
---

### 1. arr.push() 
从后面添加元素，添加一个或多个，返回值为添加完后的数组长度

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.push(6,7)) // 7
console.log(arr) // [1,2,3,4,5,6,7]
```

### 2. arr.pop() 
从后面删除元素，只能是一个，返回值是删除的元素

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.pop())  // 5
console.log(arr)  // [1,2,3,4]
```

### 3. arr.shift() 
从前面删除元素，只能是一个，返回值是删除的元素

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.shift())  // 1
console.log(arr)  // [2,3,4,5]
```


### 4. arr.unshift() 
从前面添加元素，添加一个或多个，返回值是添加完后的数组的长度

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.unshift(6,7))  // 7
console.log(arr)  // [6,7,1,2,3,4,5]
```

### 5. arr.splice(index,num) 
删除从index（索引值）开始之后的那num（默认到数组的结束位置）个元素，返回值是删除的元素数组

参数：index 索引值，num 个数

```javascript
// 1. 删除数组中的某些项
let arr = [0, 1, 2, 3, 4]
console.log(arr.splice(2, 2))  // [2, 3]
console.log(arr)  // [0, 1, 4]

// 2. 将数据添加至数组的特定索引位置index
let arr2 = [1, 2, 3, 4, 5]
arr2.splice(2, 0, '测试值')
console.log(arr2) // [1, 2, "测试值", 3, 4, 5]
```

### 6. str.split() 
将字符串转化为数组

```javascript
let str = '12345'
console.log(str.split(''))  // ["1","2","3","4","5"]
let str1 = '1/2/3/4/5'
console.log(str1.split('/'))  // ["1","2","3","4","5"]
```

### 7. arr.concat() 
连接两个数组，返回值是连接后的新数组

```javascript
let arr = [1,2,3,4,5] 2 console.log(arr.concat([6,7]))  // [1,2,3,4,5,6,7]
console.log(arr)  // [1,2,3,4,5]
```

### 8. arr.sort() 
将数组进行排序，返回值是排好的数组，默认是按照最左边的数字进行排序（非数字整体大小）

```javascript
let arr = [40,8,10,5,79,3] 
console.log(arr.sort())  // [10,3,40,5,79,8]

let arr2 = arr.sort((a,b) => a - b) 5 console.log(arr2)  // [3,5,8,10,40,79]

let arr3 = arr.sort((a,b) => b - a) 8 console.log(arr3)  // [79,40,10,8,5,3]
```

### 9. arr.reverse() 
将原数组反转，返回值是反转后的数组

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.reverse())  // [5,4,3,2,1]
console.log(arr)   // [5,4,3,2,1]
```

### 10. arr.slice(start, end) 
切去索引值start到索引值end（不包含end的值）的数组，返回值是切出去的数组

```javascript
let arr = [1,2,3,4,5] 
console.log(arr.slice(1,3))   // [2,3]
console.log(arr)    // [1,2,3,4,5]
```

### 11. arr.forEach(callback) 
遍历数组，无返回值

```javascript
let arr = [1,2,3,4,5]
arr.forEach((value, index, array) => {
    console.log(`value--${value}    index--${index}    array--${array}`) 
})

// value--1    index--0    array--1,2,3,4,5
// value--2    index--1    array--1,2,3,4,5
// value--3    index--2    array--1,2,3,4,5
// value--4    index--3    array--1,2,3,4,5
// value--5    index--4    array--1,2,3,4,5
```

### 12. arr.map(callbak) 
遍历数组(对原数组的值进行操作)，返回一个新数组

```javascript
let arr = [1,2,3,4,5] 

let arr2 = arr.map( (value, index, array)=>{
    return value = value * 2
}) 
console.log(arr2) // [2,4,6,8,10]
```

### 13. arr.filter(callback) 
过滤数组，返回一个满足要求的数组

```javascript
let arr = [1,2,3,4,5] 
let arr2 = arr.filter((value, index) => value >2) 
console.log(arr2)  // [3,4,5]
```

### 14. arr.every(callback) 
根据判断条件，遍历数组中的元素，是否都满足，若都满足则返回true，反之返回false

```javascript
let arr = [1,2,3,4,5] 
let arr2 = arr.every((value, index) => index > 2) 
console.log(arr2)  // false

let arr3 = arr.every((value, index) => index > 0) 
console.log(arr3)  // true
```

### 15. arr.some(callback) 
根据判断条件，遍历数组中的元素，是否存在至少有一个满足，若存在则返回true，反之返回false

```javascript
let arr = [1,2,3,4,5]

let arr2 = arr.some((value, index) => index > 2)
console.log(arr2) // true
 let arr3 = arr.some((value, index) => index > 5)
console.log(arr3) // false
```

### 16. arr.indexOf() 
从前往后查找某个元素的索引值，若有重复的，则返回第一个查到的索引值，若不存在，返回 -1

```javascript
let arr = [1,2,3,4,5,4] 2 
let arr2 = arr.indexOf(4) 
console.log(arr2)  // 3

let arr3 = arr.indexOf(6) 
console.log(arr3)  // -1
```

### 17. arr.lastIndexOf()  
从后往前查找某个元素的索引值，若有重复的，则返回第一个查到的索引值，若不存在，返回 -1

```javascript
let arr = [1,2,3,4,5,4] 
let arr2 = arr.lastIndexOf(4) 
console.log(arr2)  // 5

let arr3 = arr.lastIndexOf(6) 
console.log(arr3)  // -1
```

### 18. Array.from() 
[ES6]将伪数组变成数组，只要有length的就可以转成数组

```javascript
let str = '12345'
console.log(Array.from(str))    // ["1", "2", "3", "4", "5"]

let obj = {0:'a',1:'b',length:2}
console.log(Array.from(obj))   // ["a", "b"]
```

### 19. Array.of()  
[ES6]将一组值转换成数组，类似于声明数组

```javascript
let str = '11'
console.log(Array.of(str))   // ['11']

等价于 
console.log(new Array('11'))   // ['11]

ps:
new Array()有缺点，就是参数问题引起的重载
console.log(new Array(2)) // [empty × 2] 是个空数组
console.log(Array.of(2)) // [2]
```

### 20. arr.find(callback) 
[ES6]找到第一个符合条件的数组成员

```javascript
let arr = [1,2,3,4,5,2,4] 
let arr2 = arr.find((value, index, array) => value > 2) 
console.log(arr2)   // 3
```

### 21. arr.findIndex(callback) 
[ES6]找到第一个符合条件的数组成员的索引值

```javascript
let arr = [1,2,3,4,5] 
let arr1 = arr.findIndex((value, index, array) => value > 2) 
console.log(arr1)  // 2
```

### 22. arr.includes() 
[ES6]判断数组中是否包含特定的值

```javascript
let arr = [1,2,3,4,5]

let arr2 = arr.includes(2)  
console.log(arr2) // ture

let arr3 = arr.includes(9) 
console.log(arr3) // false

let arr4 = [1,2,3,NaN].includes(NaN)
console.log(arr5) // true
```

### 23. arr.fill(target, start, end) 
[ES6]使用给定的值，填充一个数组（改变原数组）

参数：  target – 待填充的元素； start – 开始填充的位置 - 索引； end – 终止填充的位置 - 索引（不包括该位置)
       

```javascript
let arr = [1,2,3,4,5]

let arr2 = arr.fill(5)
console.log(arr2) // [5, 5, 5, 5, 5]
console.log(arr)   // [5, 5, 5, 5, 5]

let arr3 = arr.fill(5,2)
console.log(arr3)  // [1,2,5,5,5]

let arr4 = arr.fill(5,1,3)
console.log(arr4)  // [1,5,5,4,5]
```

### 24. arr.keys() 
[ES6]遍历数组的键名

```javascript
let arr = [1,2,3,4,5] 
let arr2 = arr.keys() 

for (let key of arr2) { 
    console.log(key)   // 0,1,2,3,4
}
```

### 25. arr.values() 
[ES6]遍历数组键值

```javascript
let arr = [1,2,3,4,5]
let arr1 = arr.values() 

for (let val of arr1) {
     console.log(val); // 1,2,3,4,5
}
```

### 26. arr.entries() 
[ES6]遍历数组的键名和键值

entries() 方法返回迭代数组。
迭代数组中每个值 前一个是索引值作为 key， 数组后一个值作为 value。

```javascript
let arr = [1,2,3,4,5] 
let arr2 = arr.entries() 

for (let e of arr2) { 
    console.log(e);   // [0,1] [1,2] [2,3] [3,4] [4,5]
}
```



### 27.arr.copyWithin() 
[ES6]在当前数组内部，将制定位置的数组复制到其他位置，会覆盖原数组项，返回当前数组

参数:　　target --必选 索引从该位置开始替换数组项
　　　　 start --可选 索引从该位置开始读取数组项，默认为0.如果为负值，则从右往左读。
　　　　 end --可选 索引到该位置停止读取的数组项，默认是Array.length,如果是负值，表示倒数

```javascript
let arr = [1,2,3,4,5,6,7]

let arr2 = arr.copyWithin(1)
console.log(arr2)   // [1, 1, 2, 3, 4, 5, 6]

let arr3 = arr.copyWithin(1,2)
console.log(arr3)   // [1, 3, 4, 5, 6, 7, 7]

let arr4 = arr.copyWithin(1,2,4) 
console.log(arr4)   // [1, 3, 4, 4, 5, 6, 7]
```

### 28. Array.isArray(value) 
判断一个值是否为数组的方法，若为数组，返回true，反之返回false

```javascript
let a = 1234
let b = "fsaufh"
let c = {a:1,b:2}
let d = [1,2]

let mark1 = Array.isArray(a) 
 console.log(mark1)  // false

let mark2 = Array.isArray(b) 
console.log(mark2)  // false

let mark3 = Array.isArray(c) 
console.log(mark3)  // false

let mark4 = Array.isArray(d) 
console.log(mark4)  // true
```

### 29. arr.join(separate)
把数组中的所有元素放入一个字符串，separate表示分隔符，可省略，默认是逗号

```javascript
let arr = [1,2,3,4,5] 

console.log(arr.join()) // 1,2,3,4,5
console.log(arr.join("")) // 12345
console.log(arr.join("-"))  // 1-2-3-4-5
```

### 30. arr.flat(pliy)
[ES6]对数组内嵌套的数组“拉平”，就是把数组中的数组的元素挨个拿出来，放数组元素所在位置，返回一个新的数组，不会影响到原来的数组

参数：pliy表示拉平的层数，默认是1层，想无限拉平可以传入Infinity关键字

```javascript
let arr = [1, 2, [3, [4, 5]]] 
console.log(arr.flat(2))  // [1, 2, 3, 4, 5]

let arr2 = [1,[2,[3,[4,5]]]] 
console.log(arr2.flat(Infinity))  // [1,2,3,4,5]
```

### 31. arr.flatMap()
[ES6]对原数组的每个成员执行一个函数，相当于执行Array.prototype.map(),然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组。只能展开一层数组。

```javascript
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()

let arr = [2, 3, 4]
arr.flatMap((x) => [x, x * 2]) 3 // [2, 4, 3, 6, 4, 8]
```

### 32. arr.toString()
将数组转换为字符串并返回。数组中的元素之间用逗号分隔。

```javascript
let arr = ["Banana", "Orange", "Apple", "Mango"]
console.log(arr.toString())  // Banana,Orange,Apple,Mango
```

### 33. arr.reduce() 
对数组中的每个元素执行一个提供的函数（升序执行），将其结果汇总为单个返回值。

接收4个参数：

1.  Accumulator (acc) (累计器)
2.  Current Value (cur) (当前值)
3.  Current Index (idx) (当前索引)
4.  Source Array (src) (源数组)

```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue; // 1 + 2 + 3 + 4
console.log(array1.reduce(reducer)); // expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5)); // expected output: 15
```







