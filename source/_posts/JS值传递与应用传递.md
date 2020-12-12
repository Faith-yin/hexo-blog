---
title: JS值传递与应用传递
date: 2020-12-12 18:01:18
tags: 值与引用传递
categories: JavaScript
---

JS 有7中基本数据类型：Boolean、Null、Undefined、Number、BigInt、String、Symbol。这些基本数据类型都是通过值传递的方式。

值得注意的是还有另外三种类型: Array、Function 和 Object，它们通过引用来传递。从底层技术上看，它们三都是对象。

### 一、基本数据类型

> 基本类型存放在栈区，访问时按值访问，赋值是按照普通方式赋值

1. 如果一个基本的数据类型绑定到某个变量，我们可以认为该变量包含这个基本数据类型的值。


```javascript
let x = 10;
let y = "abc";
let z = null;
```

2. 当我们使用 = 对这些基本数据类型进行过赋值操作时，实际上是将对应的值拷贝了一份，然后赋值给新的变量。我们把它称作值传递。


```javascript
let a = 11
let b = 'ab'

let aa = a
let bb = b

console.log(a, b, aa, bb) // 11, ab, 11, ab
```

3. a 和 aa 都包含 11， 并且他们是相互独立的拷贝，互不干涉，如果我们将 a 的值改变，aa 不会受到影响。


```javascript
a = 1111
console.log(a, aa) // 1111, 11

b = 'abcd'
console.log(b, bb) // abcd, ab
```


### 二、引用数据类型

> 引用类型指的是对象。可以拥有属性和方法，并且我们可以修改其属性和方法。引用对象存放的方式是：在栈中存放对象变量标示名称和该对象在堆中的存放地址，在堆中存放数据。

> 对象使用的是引用赋值。当我们把一个对象赋值给一个新的变量时，赋的其实是该对象的在堆中的地址，而不是堆中的数据。也就是两个对象指向的是同一个存储空间，无论哪个对象发生改变，其实都是改变的存储空间的内容，因此，两个对象是联动的。


如果一个变量绑定到一个非基本数据类型(Array, Function, Object)，那么它只记录了一个内存地址，该地址存放了具体的数据。注意之前提到指向基本数据类型的变量相当于包含了数据，而现在指向非基本数据类型的变量本身是不包含数据的。


1. 对象在内存中被创建，当我们声明 arr = []，我们在内存中创建了一个数组。arr 记录的是该内存的地址


```javascript
let arr = [1, 2, 3]
```
当执行完之后，内存中创建了一个空的数组对象，其内存地址为 #001 ，arr指向该地址



变量 | 地址 | 对象
---|---|---
arr | #001 | [1, 2, 3]

2. 对象是通过引用传递，而不是值传递。也就是说，变量赋值只会将地址传递过去


```javascript
let arr2 = arr
console.log(arr, arr2) // [1, 2, 3], [1, 2, 3]
```

变量 | 地址 | 对象
---|---|---
arr | #001 | [1, 2, 3]
arr2 | #001 | (↑)

3. arr 和 arr2 指向同一个数组。 如果我们更新 arr，arr2 也会受到影响


```javascript
arr.push(4)
console.log(arr, arr2) // [1, 2, 3, 4], [1, 2, 3, 4]
```

变量 | 地址 | 对象
---|---|---
arr | #001 | [1, 2, 3, 4]
arr2 | #001 | (↑)

4. 引用重新赋值：如果我们将一个已经赋值的变量重新赋值，那么它将包含新的数据或则引用地址。如果原来的对象内容没有任何变量去引用，JS就会释放掉原来的对象内存。


```javascript
let obj = {a: 1}
console.log(obj) // {a: 1}
```

变量 | 地址 | 对象
---|---|---
obj | #0001 | {a: 1}


```javascript
obj = {a: 1, b: 2}
console.log(obj) // {a: 1, b: 2}
```

变量 | 地址 | 对象
---|---|---
(空)| #0001 | {a: 1}
obj | #0002 | {a: 1, b: 2}


### 三、== 和 ===

1. 对于引用类型的变量，== 和 === 只会判断引用的地址是否相同，而不会判断对象具体里属性以及值是否相同。因此，如果两个变量指向相同的对象，则返回 true

```javascript
let aa = [1, 2]
let bb = aa

console.log(aa === bb) // true
```

变量 | 地址 | 对象
---|---|---
aa | #001 | [1, 2]
bb | #001 |  (↑)

2. 如果是不同的对象，即使包含相同的属性和值，也会返回 false


```javascript
let aa = [1, 2]
let bb = [1, 2]

console.log(aa === bb) // false
```

变量 | 地址 | 对象
---|---|---
aa | #001 | [1, 2]
bb | #002 | [1, 2]

3. 如果想判断两个不同的对象是否真的相同，一个简单的方法就是将它们转换为字符串然后判断(不完美)


```javascript
let str = JSON.stringify(aa)
let str2 = JSON.stringify(bb)

console.log(str === str2) // true
```

### 四、函数参数传递

1. js 的函数参数传递为值传递。当传入的是 基本类型的参数时：就是复制了份内容给 i 而已，i 与 age 之间没有关系


```javascript
function setAge(i) {
    alert(i); // 24
    i = 18;
    alert(i);//18,i的改变不会影响外面的age
};
 
let age = 24;
setAge(age);
alert(age); // 24
```

2. 当传入的参数为引用类型时。传进去的是个地址。


```javascript
function setName(obj) {
    obj.name = 'haha';
};
 
let obj2 = new Object();
setName(obj2);
alert(obj2.name);    // haha
```


### 五、相关面试题

> 阿里2014年的笔试题

```javascript
let a = 1

let obj = {
    b: 2
}

let fn = function () {}
fn.c = 3
 
function test(x, y, z) {
    x = 4
    y.b = 5
    z.c = 6
    return z
}
test(a, obj, fn)

alert(a + obj.b + fn.c) // 12
```

首先test传递进去的实参中，a是基本类型（，复制了一份值），obj是object（指向地址，你动我也动），fn也当然不是基本类型啦。在执行test的时候，x被赋值为4(跟a没关系，各玩各的，a仍然为1)，y的b被赋值为5，那obj的b也变为5，z的c变为6，那fn的c当然也会是6. 所以alert的结果应该是1+5+6 =12. （其实test不返回z也一样，z仍然改变的）。



> [JS 的引用赋值与传值赋值](https://www.cnblogs.com/cench/p/6019453.html)  
[JavaScript的值传递和引用传递](https://blog.fundebug.com/2017/08/09/explain_value_reference_in_js/)



