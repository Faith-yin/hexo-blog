---
title: File、Blob、dataURL和canvas的应用与转换
date: 2020-12-12 18:19:25
tags: file
categories: JavaScript
---



## 一、 概念介绍

### 1. [File](https://developer.mozilla.org/zh-CN/docs/Web/API/File)

(1) 通常情况下， File 对象是来自用户在一个 `<input>` 元素上选择文件后返回的 FileList 对象,也可以是来自由拖放操作生成的 DataTransfer 对象，或者来自 HTMLCanvasElement 上的 mozGetAsFile() API。

(2) File 对象是特殊类型的 Blob，且可以用在任意的 Blob 类型的 context 中。比如：FileReader, URL.createObjectURL(), createImageBitmap(), 及 XMLHttpRequest.send() 都能处理 Blob 和 File。

<br>


### 2. [Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

(1) Blob 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 ReadableStream 来用于数据操作。

(2) Blob 表示的不一定是JavaScript原生格式的数据。File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

<br>

### 3. [dataURL](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/data_URIs)

(1) Data URLs，即前缀为 data: 协议的URL，其允许内容创建者向文档中嵌入小文件。

(2) Data URLs 由四个部分组成：前缀(data:)、指示数据类型的MIME类型、如果非文本则为可选的base64标记、数据本身：data:[<mediatype>][;base64],<data>

<br>

### 4. [canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

(1) Canvas API 提供了一个通过JavaScript 和 HTML的 `<canvas>` 元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

<br>

![关系图](https://img2020.cnblogs.com/blog/1855591/202012/1855591-20201202084724368-40559622.jpg)

## 二、相互转化

### 1. File、Blob 转化成 dataURL

> FileReader 对象允许 Web 应用程序异步读取文件(或原始数据缓冲区)内容，使用 File 或 Blob 对象指定要读取的文件或数据。

```javascript
function fileToDataURL(file) {
    let reader = new FileReader()
    reader.readAsDataURL(file)
    // reader 读取文件成功的回调
    reader.onload = function(e) {
      return reader.result
    }
}
```

<br>

### 2. dataURL(base64) 转化成 Blob(二进制)对象

```javascript
function dataURLToBlob(fileDataURL) {
    let arr = fileDataURL.split(','),
        mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]),
        n = bstr.length,
        u8arr = new Uint8Array(n);
    while(n --) {
      u8arr[n] = bstr.charCodeAt(n)
    }
    return new Blob([u8arr], {type: mime})
}
```
<br>

### 3. File, Blob 文件数据绘制到 canvas

```javascript
// 思路：File, Blob ——> dataURL ——> canvas

function fileAndBlobToCanvas(fileDataURL) {
    let img = new Image()
    img.src = fileDataURL
    let canvas = document.createElement('canvas')
    if(!canvas.getContext) {
      alert('浏览器不支持canvas')
      return;
    }
    let ctx = canvas.getContext('2d')
    document.getElementById('container').appendChild(canvas)
    img.onload = function() {
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
    }
}
```

<br>

### 4. 从 canvas 中获取文件 dataURL 

```javascript
function canvasToDataURL() {
    let canvas = document.createElement('canvas')
    let canvasDataURL = canvas.toDataURL('image/png', 1.0)
    return canvasDataURL
}
```

<br>

## 三、完整栗子

> 可以点击 [这里](http://8.131.67.8:8088/file.html) 在线预览

![预览图](https://img2020.cnblogs.com/blog/1855591/202011/1855591-20201125144437125-1637282205.png)

```javascript
<!--
 * @Date: 2020-11-22 14:33:55
 * @information: datadURL File Blob canvas 的互相转化
 * 
 * File.prototype instanceof Blob === true
 * Blob.prototype instanceof Object === true
-->
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>datadURL File Blob canvas</title>
  <style>
    .body {
      text-align: center;
    }
    .img-box {
      margin: 20px 0;
    }
    #img {
      width: 60%;
    }
  </style>
</head>
<body>
  
  <div class="body">

    <div class="input-box">
      <input id="input" type="file" accept="image/png, image/jpeg" onchange="onChangeInput()">
    </div>
    
    <div class="img-box">
      img: 
      <img src="" alt="img" id="img">
    </div>

    <div class="canvas-box" id="canvas-box">
      canvas: 
    </div>

  </div>


<script>
  // 文件对象
  let file; 
  // 文件 base64 码
  let fileDataURL;


  /**
   * @Date: 2020-11-25 10:32:51
   * @information: 获取文件
   */
  function onChangeInput() {
    file = document.getElementById('input').files[0]
    console.log('file->', file)
    if(!FileReader) {
      alert('浏览器版本过低，请升级版本')
      return;
    }
    fileToDataURL()
  }
  
  /**
   * @Date: 2020-11-25 10:31:47
   * @information: 使用 FileReader 读取文件内容， File(二进制) ——> dataURL(base64)   Blob ——> dataURL 同理
   */
  function fileToDataURL() {
    let reader = new FileReader()
    reader.readAsDataURL(file)
    reader.onload = function(e) {
      console.log('dataURL->', reader.result)
      fileDataURL = reader.result
      showImg()
      dataURLToBlob()
    }
  }

  /**
   * @Date: 2020-11-25 10:33:13
   * @information: 图片回显
   */
  function showImg() {
    let img = document.getElementById('img')
    img.src = fileDataURL
  }

  /**
   * @Date: 2020-11-25 10:34:47
   * @information: dataURL(base64) ——> Blob(二进制)对象
   */
  function dataURLToBlob() {
    let arr = fileDataURL.split(','),
        mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]),
        n = bstr.length,
        u8arr = new Uint8Array(n);
    while(n --) {
      u8arr[n] = bstr.charCodeAt(n)
    }
    console.log('blob->', new Blob([u8arr], {type: mime}))
    fileAndBlobToCanvas()
    return new Blob([u8arr], {type: mime})
  }

  /**
   * @Date: 2020-11-25 10:53:31
   * @information: File, Blob 文件数据绘制到 canvas
   * 思路：File, Blob ——> dataURL ——> canvas
   */
  function fileAndBlobToCanvas() {
    let img = new Image()
    img.src = fileDataURL
    let canvas = document.createElement('canvas')
    if(!canvas.getContext) {
      alert('浏览器不支持canvas')
      return;
    }
    let ctx = canvas.getContext('2d')
    document.getElementById('canvas-box').appendChild(canvas)
    img.onload = function() {
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
      canvasToDataURL()
    }
  }

  /**
   * @Date: 2020-11-25 11:23:54
   * @information: 从 canvas 中获取文件 dataURL 
   */
  function canvasToDataURL() {
    let canvas = document.createElement('canvas')
    let canvasDataURL = canvas.toDataURL('image/png', 1.0)
    console.log('从 canvas 中获取文件 dataURL :', canvasDataURL)
  }

</script>

</body>
</html>

```

<br><br><br><br>

