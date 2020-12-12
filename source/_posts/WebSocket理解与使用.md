---
title: WebSocket理解与使用
date: 2020-12-12 18:13:22
tags: websocket
categories: Websocket
---


### 一、WebSocket 理解

1. **概念：** WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

2. **特点：** WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

3. **流程：** 在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。


4. **目前：** 现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

5. **优势：** HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

<br>

### 二、WebSocket 属性


1. **WebSocket 对象**

```javascript
let ws = new WebSocket('ws://localhost:3000')  // 创建 WebSocket 对象
```

<br>

2. **WebSocket 对象属性：** 

属性 | 描述
---|---
ws.readyState | 只读属性 readyState 表示连接状态，可以是以下值: <br>   0 - 表示连接尚未建立。 <br> 1 - 表示连接已建立，可以进行通信。<br>2 - 表示连接正在进行关闭。<br> 3 - 表示连接已经关闭或者连接不能打开。
   
<br>

3. **WebSocket 对象事件：**

事件 | 事件处理程序 | 描述
---|---|---
open | ws.onopen | 连接建立时触发
message | ws.onmessage | 客户端接收服务端数据时触发
error | ws.onerror | 通信发生错误时触发
close | ws.onclose | 连接关闭时触发

<br>

4. **WebSocket 对象方法：**

方法 | 描述
---|---
ws.send() | 使用连接发送数据
ws.close() | 关闭连接

<br>

### 三、WebSocket 使用

```javascript

// 创建 WebSocket 对象
let ws = new WebSocket('ws://localhost:3000')
// 定时器
let timer;

// 监听打开
ws.onopen = webSocketOpen;
// 监听异常
ws.onerror = webSocketError;
// 监听消息
ws.onmessage = webSocketMessage;
// 监听关闭
ws.onclose = webSocketClose;


function webSocketOpen() {
    console.log(`连接成功`)
    start()
},

function webSocketError() {
    console.log(`连接异常，请刷新页面重试`)
},

function webSocketMessage(e) {
    console.log(`接收到消息:${e.data}`)
},

function webSocketClose() {
    console.log(`连接关闭`)
    clearInterval(timer)
},

// 发送心跳, 因为长时间不发送消息，就会断
function start() {
  clearInterval(timer)
  timer = setInterval(() => {
    let date = new Date()
    ws.send(`发送心跳给后端${date}`)
  }, 2 * 60 * 1000)
}

```

<br>


### 四、WebSocket 应用

1. 双向通信，如聊天室。

2. 微信小程序对 WebSocket 进行了封装，wx.connectSocket() 可以理解为创建了一个 WebSocket 实例 SocketTask。

3. `socket.io` 支持 WebSocket、轮询、HTTP 流等方式。

<br><br><br><br><br>

