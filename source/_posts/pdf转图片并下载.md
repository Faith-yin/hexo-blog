---
title: pdf转图片并下载
date: 2020-12-12 18:15:00
tags: pdf
categories: JavaScript
---



### 一、实现效果

选择本地 pdf 文件上传，会生成 pdf 文件的预览，点击保存功能。

![预览图1](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201022165147927-1392643265.png)

![预览图2](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201022165256769-1608228895.png)

![预览图3](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201022165306613-1977214439.png)


<br>

### 二、所用插件

1. pdf文件相关的文件处理插件：`pdfjs-dist@2.3.200`
2. zip压缩文件相关的包： `jszip@3.5.0`
3. 文件保存下载相关的包：`file-saver@2.0.2`

<br>

### 三、相关代码

> vue 中测试

```javascript

<!--
 * @Date: 2020-10-21 10:44:54
 * @information: pdf 转图片并下载
-->
<template>
  <div id="page09">

    <div class="info-box">
      <div class="input">
        <input id="input" type="file" accept="application/pdf" @change="convertFile()"/>
      </div>
      <div class="cell">
        <div>名称：{{fileName || '-'}}</div>
        <div>大小：{{Number(fileSize).toFixed(2)}}M</div>
        <div>页数：{{filePage}}</div>
        <button @click="onExportImg">保存图片</button>
      </div>
    </div>

    <div id="container"></div>

  </div>
</template>

<script>
import PDFJS from 'pdfjs-dist'
import JSZIP from 'jszip'
import FileSaver from 'file-saver'

export default {
  data() {
    return {
      // pdf地址
      pdfPath2: `https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf`,
      // arrayBuffer
      arrayBuffer: null, 
      // 文件名称
      fileName: null,
      // 文件大小
      fileSize: 0,
      // 文件页数
      filePage: 0,
    }
  },
  methods: {
    
    /**
     * 读取文件内容
     */
    convertFile() {
      let file = document.getElementById('input').files
      if(!file.length) return;
      let {name, size} = file[0]
      Object.assign(this, {fileName: name, fileSize: size/1024/1024})
      // 使用FileReader对象，web应用程序可以异步的读取存储在用户计算机上的文件(或者原始数据缓冲)内容，可以使用File对象或者Blob对象来指定所要处理的文件或数据
      let fileReader = new FileReader()
      // 异步按字节读取文件内容，结果用ArrayBuffer对象表示
      fileReader.readAsArrayBuffer(file[0])
      fileReader.onload = (e) => {
        let arrayBuffer = this.arrayBuffer = e.target.result
        // 创建canvas节点
        this.createCanvas(arrayBuffer)
      }
    },

    /**
     * 创建canvas
     */
    createCanvas(val) {
      // 清空节点下数据
      document.getElementById('container').innerHTML = ''
      // 使用getTextContent获取pdf内容
      PDFJS.getDocument(val).promise.then(el => {
        let filePage = this.filePage = el.numPages
        for(let i = 1; i <= filePage; i ++) {
          let canvas = document.createElement('canvas')
          canvas.id = `pageNum-${i}`
          let context = canvas.getContext('2d')
          document.getElementById('container').append(canvas)
          // 渲染canvas
          this.openPage(el, i, context)
        }
      })
    },

    /**
     * 渲染canvas
     */
    openPage(pdfFile, pageNumber, context) {
      // 获取PDF文档中的各个页面
      pdfFile.getPage(pageNumber).then(page => {
        // 设置展示比例
        let scale = 3
        // 获取pdf尺寸
        let viewport = page.getViewport(scale)
        let canvas = context.canvas
        canvas.width = viewport.width
        canvas.height = viewport.height
        canvas.style.width = "100%"
        canvas.style.height = "100%"

        let model = {
          canvasContext: context,
          viewport: viewport,
        }
        // 渲染PDF
        page.render(model)
      })
    },

    /**
     * 保存图片
     */
    onExportImg() {
      if(!this.arrayBuffer) {
        alert(`请上传pdf文件`)
        return;
      }

      let jszip = new JSZIP()
      // 解压缩后的文件夹名称
      let images = jszip.folder("images")
      let eleList = document.querySelectorAll('canvas')
      // 遍历所有canvas节点
      for(let i = 0; i < eleList.length; i ++) {
        let canvas = document.getElementById(`pageNum-${i+1}`)
        // 向此文件夹中加入文件
        // toDataURL() 方法返回一个包含图片展示的 data URI 。可以使用 type 参数其类型，默认为 PNG 格式。图片的分辨率为96dpi
        images.file(`image-${i+1}.png`, this.dataURLtoBlob(canvas.toDataURL("image/png", 1.0)), {
          base64: true
        })
      }

      // 生成一个zip文件
      jszip.generateAsync({
        type: "blob"
      }).then((content) => {
        // 使用 FileSaver 保存下载 zip 文件
        FileSaver.saveAs(content, "pdfToImages.zip");
      })
    },

    /**
     * dataURL 转成 Blob
     */
    dataURLtoBlob(val) {
      let arr = val.split(','),
          mime = arr[0].match(/:(.*?);/)[1],
          bstr = atob(arr[1]),
          n = bstr.length,
          u8arr = new Uint8Array(n);
      while(n --) {
        // charCodeAt() 方法可返回指定位置的字符的 Unicode 编码。这个返回值是 0 - 65535 之间的整数
        u8arr[n] = bstr.charCodeAt(n)
      }
      // new Blob(blobParts, [options]) 参数说明:
      // 1. blobParts：数组类型，数组中的每一项连接起来构成Blob对象的数据，数组中的每项元素可以是ArrayBuffer, ArrayBufferView, Blob, DOMString 
      // 2. options：可选项，字典格式类型，可以指定如下两个属性：
      //    (1) type，默认值为 ""，它代表了将会被放入到blob中的数组内容的MIME类型。
      //    (2) endings，默认值为"transparent"，用于指定包含行结束符\n的字符串如何被写入。 它是以下两个值中的一个： "native"，表示行结束符会被更改为适合宿主操作系统文件系统的换行符； "transparent"，表示会保持blob中保存的结束符不变

      return new Blob([u8arr], { type: mime })
    },

    



  },
  created() {

  },
  mounted() {

  }
}
</script>

<style lang="scss">
#page09 {
  width: 1000px;
  margin: 0 auto;

  .info-box {
    position: relative;

    .input {
      margin: 15px 0;

      #input {
        width: 100%;
        height: 100%;
        cursor: pointer;
      }
    }

    .cell {
      margin: 15px 0;
      display: flex;  
      justify-content: space-around;

      div {
        margin-right: 20px;
      }
    }
  }

  #container {
    width: 100%;
    min-height: 850px;
    margin:  0 auto;
    border: 1px solid #eee;
    border-radius: 10px;

    canvas {
      margin-bottom: 10px;
      border: 1px solid #ff6700;
      border-radius: 10px;
    }
  }


}
</style>
```

<br>

### 四、相关知识点

#### 1. 使用 FileReader 进行文件读取

> [[HTML5] FileReader对象](https://www.cnblogs.com/hhhyaaon/p/5929492.html)

（1）创建实例

```javascript
let fileReader = new FileReader()
```


（2）方法

方法定义 | 描述
---|---
abort():void | 终止文件读取操作
readAsArrayBuffer(file):void | 异步按字节读取文件内容，结果用ArrayBuffer对象表示
readAsBinaryString(file):void | 异步按字节读取文件内容，结果为文件的二进制串
readAsDataURL(file):void | 异步读取文件内容，结果用data:url的字符串形式表示
readAsText(file,encoding):void | 异步按字符读取文件内容，结果用字符串形式表示


（3）事件： FileReader 通过异步的方式读取文件内容，结果均是通过事件回调获取。

方法定义 | 描述
---|---
onabort | 当读取操作被中止时调用
onerror | 当读取操作发生错误时调用
onload | 当读取操作成功完成时调用
onloadend | 当读取操作完成时调用,不管是成功还是失败
onloadstart | 当读取操作将要开始之前调用
onprogress | 在读取数据过程中周期性调用



<br>

#### 2. 使用 canvas 的 `toDataURL()` 方法返回 包含 data URI 的DOMString


```javascript
let canvas = document.getElementById(`canvas`)

// 参数说明：
// 1. type（可选）：图片格式，默认为 image/png
// 2. encoderOptions（可选）：在指定图片格式为 image/jpeg 或 image/webp的情况下，可以从 0 到 1 的区间内选择图片的质量。如果超出取值范围，将会使用默认值 0.92。其他参数会被忽略。
let result = canvas.toDataURL("image/png", 1.0)
```
> [MDN: HTMLCanvasElement.toDataURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL)

<br>


#### 3. ArrayBuffer 二进制数组

> [ArrayBuffer，二进制数组](https://zh.javascript.info/arraybuffer-binary-arrays)

（1） JS 中的二进制数据格式，例：ArrayBuffer，Uint8Array，DataView，Blob，File 及其他。

**基本的二进制对象是 ArrayBuffer —— 对固定长度的连续内存空间的引用。**


（2）如要操作 ArrayBuffer，我们需要使用“视图”对象。 视图对象本身并不存储任何东西。它是一副“眼镜”，透过它来解释存储在 ArrayBuffer 中的字节。例如下：


视图类型 | 说明
---|---
Uint8Array | 将 ArrayBuffer 中的每个字节视为 0 到 255 之间的单个数字（每个字节是 8 位，因此只能容纳那么多）。这称为 “8 位无符号整数”。
Uint16Array | 将每 2 个字节视为一个 0 到 65535 之间的整数。这称为 “16 位无符号整数”。
Uint32Array  | 将每 4 个字节视为一个 0 到 4294967295 之间的整数。这称为 “32 位无符号整数”。
Float64Array | 将每 8 个字节视为一个 5.0x10-324 到 1.8x10308 之间的浮点数。




<br>

#### 3. Blob

> [细说Web API中的Blob](https://segmentfault.com/a/1190000011563430)

（1）概述： Blob 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 ReadableStream 来用于数据操作。Blob 表示的不一定是JavaScript原生格式的数据。File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

（2）创建 Blob 对象


```javascript
// 参数说明
// 1. blobParts： 数组类型，数组中的每一项连接起来构成Blob对象的数据，数组中的每项元素可以是ArrayBuffer, ArrayBufferView, Blob, DOMString
// 2. options：可选项，字典格式类型，可以指定如下两个属性：

type，默认值为 ""，它代表了将会被放入到blob中的数组内容的MIME类型。
endings，默认值为"transparent"，用于指定包含行结束符\n的字符串如何被写入。 它是以下两个值中的一个： "native"，表示行结束符会被更改为适合宿主操作系统文件系统的换行符； "transparent"，表示会保持blob中保存的结束符不变。

let blob = new Blob(blobParts[, options])

let data1 = "a"
let data2 = { "name": "abc" }

let blob1 = new Blob([data1])
let blob2 = new Blob([JSON.stringify(data4)])
let blob3 = new Blob([data4])

console.log(blob1);  // Blob {size: 1, type: ""}
console.log(blob2);  // Blob {size: 14, type: ""}
console.log(blob3);  // Blob {size: 15, type: ""}
```

> size代表Blob 对象中所包含数据的字节数。这里要注意，使用字符串和普通对象创建Blob时的不同，blob4使用通过JSON.stringify把data4对象转换成json字符串，blob5则直接使用data4创建，两个对象的size分别为14和15。blob4的size等于14很容易理解，因为JSON.stringify(data4)的结果为："{"name":"abc"}"，正好14个字节(不包含最外层的引号)。blob5的size等于15是如何计算而来的呢？实际上，当使用普通对象创建Blob对象时，相当于调用了普通对象的toString()方法得到字符串数据，然后再创建Blob对象。所以，blob5保存的数据是"[object Object]"，是15个字节(不包含最外层的引号)。


（3）Blob 方法： slice()

（4）Blob使用场景

- 分片上传
- Blob URL



<br>

> [PDF转成图片](https://xxlllq.github.io/readme/2018/05/09/pdf2img.html)

<br><br>
