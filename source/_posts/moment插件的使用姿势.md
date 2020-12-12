---
title: moment插件的使用姿势
date: 2020-12-12 16:31:57
tags: [插件, moment]
categories: Vue
---



### 日期格式化：

```javascript
1 moment().format('MMMM Do YYYY, h:mm:ss a'); // 三月 7日 2020, 11:59:47 中午
2 
3 moment().format('dddd');                    // 星期六
4 
5 moment().format("MMM Do YY");               // 3月 7日 20
6 
7 moment().format('YYYY [escaped] YYYY');     // 2020 escaped 2020
8 
9 moment().format();                          // 2020-03-07T11:59:47+08:00
```


### 相对时间：

```javascript
1 moment("20111031", "YYYYMMDD").fromNow(); // 8 年前
2 
3 moment("20120620", "YYYYMMDD").fromNow(); // 8 年前
4 
5 moment().startOf('day').fromNow();        // 12 小时前
6 
7 moment().endOf('day').fromNow();          // 12 小时内
8 
9 moment().startOf('hour').fromNow();       // 1 小时前
```


### 日历时间：

```javascript
 1 moment().subtract(10, 'days').calendar(); // 2020/02/26
 2 
 3 moment().subtract(6, 'days').calendar();  // 上星期日11:59
 4 
 5 moment().subtract(3, 'days').calendar();  // 上星期三11:59
 6 
 7 moment().subtract(1, 'days').calendar();  // 昨天11:59
 8 
 9 moment().calendar();                      // 今天11:59
10 
11 moment().add(1, 'days').calendar();       // 明天11:59
12 
13 moment().add(3, 'days').calendar();       // 下星期二11:59
14 
15 moment().add(10, 'days').calendar();      // 2020/03/17
```


### 其他格式：

```javascript
 1 moment.locale();         // zh-cn
 2 
 3 moment().format('LT');   // 11:59
 4 
 5 moment().format('LTS');  // 11:59:47
 6 
 7 moment().format('L');    // 2020/03/07
 8 
 9 moment().format('l');    // 2020/3/7
10 
11 moment().format('LL');   // 2020年3月7日
12 
13 moment().format('ll');   // 2020年3月7日
14 
15 moment().format('LLL');  // 2020年3月7日中午11点59分
16 
17 moment().format('lll');  // 2020年3月7日 11:59
18 
19 moment().format('LLLL'); // 2020年3月7日星期六中午11点59分
20 
21 moment().format('llll'); // 2020年3月7日星期六 11:59
```

 
### 常用格式：

 
```javascript
moment().format("YYYY年MM月DD日 HH时mm分ss秒"); //2020年04月20日 18时59分50秒

moment(1711641720000).format('YYYY-MM-DD HH:mm:ss'); //2020-04-20 18:59:50（24小时制）
 
moment(1711641720000).format('YYYY-MM-DD hh:mm:ss'); //2020-04-20 06:59:50（12小时制）
```
 
 

 

> [moment 文档](http://momentjs.cn/docs/)

 

