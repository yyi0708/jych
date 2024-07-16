---
title: Web应用程序-处理文件格式
date: 2023-09-11 19:00:47
tags: 
  - 前端
  - 小技巧
category: developer
timeline: front-end
toc: true
abstract: 一起来了解前端常见的文件格式及其转化，想必接口、组件接收存在花里胡哨各种要求。
---


## 核心
::: warning
1. ArrayBuffer、File、Blob、base64、string类型格式转换
2. 文件编码类型指定与转换(`utf-8` `gbk`等)
3. `File` `Blob` `ArrayBuffer`等格式定义
:::


## 正常操作
大家都知道，web应用无法控制本机资源文件，唯一方式根据`<input type="file">`表单获取文件，获取本地资源文件格式`file`，后根据后台接口`formdata`上传资源操作。

## 假设场景

### 假设场景-1
> 假设: 自定义图片资源预览，则 **需要将file文件展示base64或对象的 URL进行资源展示。**  
> 思路： base64展示，对象URL显示

```js
const file = // your file object
const reader = new FileReader()

reader.onload = function(e) {
  const ImgSrc = e.target.result
  console.log(ImgSrc)
}

reader.readAsDataURL(file)
```

```js
const file = // your file object
const reader = new FileReader()

reader.onload = function(e) {
  const fileAsBuffer = e.target.result
  console.log(fileAsBuffer)

  const blob = new Blob([fileAsBuffer])
  const ImgSrc = URL.createObjectURL(blob);
}

reader.readAsArrayBuffer(file)
```


### 假设场景-2
> 假设:
> 上传文件`data.csv`格式字符串编码为`utf-8`, 后台只接收如`gbk`格式，否则字符串乱码导致资源无法展示。

[iconv-lite: Pure JS character encoding conversion](https://github.com/ashtuchkin/iconv-lite)

```js
import iconvLite from 'iconv-lite'

const file = // your file object
const reader = new FileReader()

reader.onload = function(e) {
  const fileAsString = e.target.result
  const buff = iconvLite.encode(fileAsString, 'gbk')

  var myFile = new File([txtBuffer], name[, options]);

  const blob = new Blob([fileAsBuffer])
  const ImgSrc = URL.createObjectURL(blob);
}

reader.readAsText(file, 'utf-8')

```


## 文件定义与互转
::: warning
File 对象是特殊类型的 Blob，且可以用在任意的 Blob 类型的 context 中。如， FileReader, URL.createObjectURL(), createImageBitmap() (en-US), 及 XMLHttpRequest.send() 都能处理 Blob 和 File。
:::

### 格式定义
* [MDN-File](https://developer.mozilla.org/zh-CN/docs/Web/API/File/File#bits)
* [MDN-Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob/Blob)
* [MDN-ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer/ArrayBuffer)
* [MDN-createObjectURL](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL_static)
* [MIME 类型列表](https://www.iana.org/assignments/media-types/media-types.xhtml)
* [MDN-FileReader](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)



```js
// File
const myFile = new File(bits, name[, options]);
// bits 一个包含ArrayBuffer，ArrayBufferView，Blob，或者 DOMString 对象的 Array — 或者任何这些对象的组合。这是 UTF-8 编码的文件内容。
// options type: DOMString，表示将要放到文件中的内容的 MIME 类型。默认值为 "" 

// Blob
const blob = new Blob(array, options)
// array 一个可迭代对象，比如 Array，包含 ArrayBuffer、TypedArray、DataView、Blob、字符串或者任意这些元素的混合，这些元素将会被放入 Blob 中。

// ArrayBuffer() 表示通用的原始二进制数据缓冲区。
new ArrayBuffer(length)

// URL.createObjectURL()
const objectURL = URL.createObjectURL(object); 
// 用于创建 URL 的 File 对象、Blob 对象或者 MediaSource 对象。

```


### 字符串转ArrayBuffer
```js
function getBufferByStr(str = '') {
    const buf = new ArrayBuffer(str.length*2); // 每个字符占用2个字节
    const bufView = new Uint16Array(buf);
    for (const i=0, strLen=str.length; i<strLen; i++) {
        bufView[i] = str.charCodeAt(i);
    }
    return buf;
}
```

### ArrayBuffer转File
```js
// type 为MIME 类型，查看看上述[MIME 类型列表]
function getFile(buffer) {
    return new File([buffer], 'custom-name.txt', {
        type: "text/plain",
    })
}
```

### ArrayBuffer转Blob
```js
// type 为MIME 类型，查看看上述[MIME 类型列表]
function getBlob(buffer) {
    return new Blob([buffer], {
        type: "application/json",
    })
}
```

###  File或Blob 对象 -》 ArrayBuffer、base64 编码、字符串文本
* readAsText() 返回文本
* readAsDataURL() 返回base64 编码
* readAsArrayBuffer() 返回buffer
```js
const file = // your file object
const reader = new FileReader()

reader.onload = function() {
  const fileAsString = reader.result
  console.log(fileAsString)
}

reader.readAsText(file, 'utf-8') // 以'utf-8'解析文本内容
```