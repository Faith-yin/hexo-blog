---
title: Web前端存储之sessionStorage和localStorage
date: 2020-12-12 17:37:57
tags: 前端存储
categories: JavaScript
---

## 前言

1. 对浏览器来说，使用 Web Storage 存储键值对比存储 Cookie 方式更直观，而且容量更大，它包含两种：localStorage 和 sessionStorage

localStorage和sessionStorage的存储数据大小一般都是：5MB

- sessionStorage（临时存储） ：为每一个数据源维持一个存储区域，在浏览器的此标签页打开期间存在，包括此标签页的页面重新加载

- localStorage（长期存储） ：与 sessionStorage 一样，但是浏览器关闭后，数据依然会一直存在
 

> 用法说明：sessionStorage 和 localStorage 的用法基本一致，引用类型的值需要转换成 JSON 进行存储

## 用法
 

### 1. 保存数据到本地

```javascript
let obj = {
    name: 'xiaoming',
    age: 18,
    birthday: '2000-01-01',
}

sessionStorage.setItem('userInfo', JSON.stringify(obj))

localStorage.setItem('userInfo', JSON.stringify(obj))
```

> 注：若第二次存储的key值与第一次存储的key值相同，则会覆盖第一次的值。

 

### 2. 从本地读取数据

 
```javascript
let obj1 = JSON.parse(sessionStorage.getItem('userInfo'))

let obj2 = JSON.parse(localStorage.getItem('userInfo'))
``` 

### 3. 从本地删除指定 key 值

 
```javascript
sessionStorage.removeItem('userInfo')

localStorage.removeItem('userInfo')
```

### 4. 清空存储的所有值

 
```javascript
sessionStorage.clear()

localStorage.clear()
 ```
 
