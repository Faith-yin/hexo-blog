---
title: vue-cli3.x多页面配置
date: 2020-12-12 18:21:03
tags: [多页面, vue]
categories: Vue
---


## 一、前言

### 1. 单页面（SPA Single-Page-Application）


（1）入口：单页面只是一张 Web 页面的模式，只有一个 html 文件，整个项目只有一个入口文件。

（2）资源加载：单页面应用的资源只在开始初始化进入的的时候加载一次（属于全局加载使用），之后的组件跳转不再重新加载。项目初始化压力较大。

（3）页面跳转：单页面内的页面跳转其实是 vue 运用了 vue-router 实现了组件切换。

（4）数据传递：可通过全局变量、参数或 store 进行数据交互。 


### 2. 多页面（MPA Multi-Page-Application）

（1）入口：多页面是多张 web 页面的模式，有多个 html 文件，整个项目有多个入口文件。

（2）资源加载：多页面之间的资源互不影响，npm 依赖包是全局安装，但是在多页面每个入口文件（main.js）手动按需引入。多页面之间资源互不共享，跳转需要资源重新加载，项目初始化压力较小，但多页面之间跳转资源需重新加载，压力相对较大。

（3）页面跳转：多页面的单个页面内部是 vue-router 形式的组件切换；多页面之间需通过 a 标签跳转页面。

（4）数据传递：多页面的单个页面内部和 SPA 一致；多页面之间需通过地址栏传参形式进行数据交互。


![对比1](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201201181302818-1993906304.png)

![对比2](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201201185858593-1215493062.png)

<br>

## 二、配置

### 1. 新建项目 demo 

（1）通过 `vue-cli 3.x` 新建项目（vue create demo），删除项目中自带的 `App.vue` 和 `main.js`

（2）安装 `path` 和 `glob` 依赖包（npm i path -D 、npm i glob -D）

### 2. 配置入口文件

（1） 在 `src` 目录下新建 `pages` 用于配置多页面模块的文件夹。

（2）在 `pages` 文件夹下新建多页面模块，在此举栗 `home` 和 `index` 文件夹。

（3）在多页面的模块下（`home` 和 `index`），分别新建 `App.vue` 和 `main.js`，按原有的内容填充这两个文件，用于每个页面模块的入口。 


```javascript
<!--
 * @Date: 2020-12-01 13:53:49
 * @information: App.vue
-->
<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
```


```javascript
/*
 * @Date: 2020-12-01 13:53:55
 * @information: main.js
 */
import Vue from 'vue'
import App from './App.vue'
import router from '../../router/home'
import store from '../../store'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```



### 3. 添加全局启动和打包配置

（1）在项目根目录下新建 `vue.config.js` 配置文件 

（2）添加入口声明配置 和 打包配置，具体如下：

```javascript
/*
 * @Date: 2020-12-01 11:35:31
 * @information: vue.config.js
 */
const path = require('path')
const glob = require('glob')

const titles = {
  home: '这是home标题',
  index: '这是index标题'
}

// 获取pages文件夹下的文件
function getEntry(globPath) {
  let entries = {}, tmp;
  // 读取js文件
  glob.sync(globPath+'js').forEach(function(entry) {
    tmp = entry.split('/').splice(-3)
    entries[tmp[1]] = {
      entry,
      template: 'index.html',
      filename: tmp[1] + '.html',
      title: titles[tmp[1]],
    }
  })
  return entries
}

const htmls = getEntry('./src/pages/**/*.')

module.exports = {
  pages: htmls,
  publicPath: './',
  outputDir: 'dist', // 打包后的文件夹名称，默认dist
  devServer: {
    open: true,
    hot: true,
    index: './index.html', // 默认启动页面
    host: '0.0.0.0',
    port: 8090,
  },
  productionSourceMap: false, // 生产环境是否生成 sourceMap 文件
}
```

<br>

## 三、爬坑注意

### 1. 项目目录划分

（1）将 `components` 、`router`、`views`、`store` 、静态数据配置层、业务层等文件结构按照多页面模块严格划分，多页面之间不容有业务上的耦合，防止进坑。

（2）对于每个页面模块所用到的资源按需引入，减轻模块加载压力。

### 2. 项目目录及打包的 html 文件如图

（1）项目访问地址方式：【http://localhost:8090/home.html#/about】 先指定要访问的静态 html 文件，再添加此页面下的路由地址即可。


![项目 pages 文件夹](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201201182116492-1947954617.png)

项目 pages 文件夹

<br>

![home.html 文](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201201182158294-281140356.png)

![index.html](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201201182206734-111809435.png)

项目打包后的多页面生成的 html 文件


<br><br><br>

