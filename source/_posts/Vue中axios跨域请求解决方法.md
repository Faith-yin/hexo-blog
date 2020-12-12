---
title: Vue中axios跨域请求解决方法
date: 2020-12-12 17:00:23
tags: [跨域, vue]
categories: Vue
---

### 前言

 **跨域** ：指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。

 

所谓 **同源** 是指，域名，协议，端口均相同，浏览器执行 js 脚本时，会检查这个脚本属于哪个页面，如果不是同源页面，就不会被执行。


### 以下举例：

（1）http://www.123.com/index.html 调用 http://www.123.com/server.php （非跨域）

（2）http://www.123.com/index.html 调用 http://www.456.com/server.php （主域名不同:123/456，跨域）

（3）http://abc.123.com/index.html 调用 http://def.123.com/server.php （子域名不同:abc/def，跨域）

（4）http://www.123.com:8080/index.html 调用 http://www.123.com:8081/server.php （端口不同:8080/8081，跨域）

（5）http://www.123.com/index.html 调用 https://www.123.com/server.php （协议不同:http/https，跨域）

（6）localhost和127.0.0.1虽然都指向本机，但也属于跨域。

　　

### 一，前端解决之 代理

仅开发环境下建议如此。。

#### 1.  vue-cli 2.x 版本解决方法如下 
 

（1） Vue 的 config 文件夹下的 index.js 文件中，在 proxyTable
对象中书写跨域配置项：将以  /api 开头的请求地址基础URL替换为 http://localhost:8888 

（2）将 axios 的 baseURL 改为 /api 

 ![](https://img2020.cnblogs.com/blog/1855591/202003/1855591-20200308174002544-1723905577.png)

 ![](https://img2020.cnblogs.com/blog/1855591/202003/1855591-20200308174512674-687478051.png)


#### 2.  vue-cli 3.x 版本解决方法如下 

（1）在项目根目录下创建全局配置文件 vue.config.js

（2）在配置文件中书写跨域配置（如下图）

（3）将 axios 的 baseURL 改为 /api 


![](https://img2020.cnblogs.com/blog/1855591/202008/1855591-20200830141057452-2064672090.png)

 

### 二，后端springboot项目解决之 配置项
 

推荐在服务端进行跨域相关配置，在项目中新建允许跨域配置类，如下图。


![](https://img2020.cnblogs.com/blog/1855591/202008/1855591-20200830141622702-1420880345.png)

 
