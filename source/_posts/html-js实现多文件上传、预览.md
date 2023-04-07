---
title: html+js实现多文件上传、预览
date: 2023-04-07 15:21:34
tags: 
	- 文件上传
categories: 
	[前端,HTML]
---

## html+js实现多文件上传、预览

当我们需要上传图片时，一般是通过文件上传的方式上传图片到服务器，但有时候我们需要在上传之前预览图片，以便查看是否选择了正确的图片或者调整裁剪等。这时我们就需要用到前端技术来实现图片预览功能。
下面将介绍如何使用 HTML、CSS 和 JavaScript 实现文件上传和图片预览的功能，具体实现如下。

### 文件上传表单和预览区

首先，我们需要一个文件上传表单和一个预览区域。文件上传表单可以使用标准的 HTML `<form>`标签实现，如下所示：

```Html
<form action="#" method="post" enctype="multipart/form-data">
    <input type="file" name="fileToUpload" id="fileToUpload" multiple>
    <input type="submit" value="上传文件">
</form>
```

这个表单包含一个文件选择框和一个提交按钮，用户可以通过文件选择框选择要上传的文件并点击提交按钮上传。
为了实现预览功能，我们还需要一个预览区域，用于在上传文件之前显示选择的图像文件。预览区域可以使用一个空的 `<div>`元素实现，如下所示：

```Html
<!-- 图像预览区域 -->
<div id="imagePreviews"></div>
```

在这个示例中，我们将预览区域的 ID 设置为`imagePreview`。

### CSS样式

为预览区域定义一些基本的 CSS 样式，并确保它的大小和文件上传框的大小相同，如下所示

```Css
#imagePreview {
    width: 200px;
    height: 200px;
    margin-top: 20px;
    background-color: #eee;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;
}
```

这个 CSS 样式定义了预览区域的宽度、高度、边距、背景颜色和背景图像属性，使其以一种较为合理的方式呈现预览图片。在这个示例中，我们将预览区域的大小设置为 200 x 200 像素，并将背景颜色设置为浅灰色

### JavaScript 实现文件上传和预览

现在，我们需要一些 JavaScript 代码来实现文件上传和图像预览的功能

```JavaScript
// 获取文件上传表单中的 input 元素
var fileInput = document.getElementById('fileToUpload');
// 监听 input 元素的 onchange 事件
fileInput.addEventListener('change', function(event) {
// 获取上传的文件
var file = event.target.files[0];
// 如果上传的文件是图像文件，则进行预览
if (file.type.match('image.*')) {
    var reader = new FileReader();
    // 将图像文件转换为 Data URL
    reader.readAsDataURL(file);
    // 当读取完成时，显示预览图像
    reader.onload = function() {
    var imagePreview = document.getElementById('imagePreview');
    imagePreview.style.backgroundImage = 'url(' + reader.result + ')';
    }
}
});
```

这个示例代码使用了`JavaScript`监听文件上传表单中的`input`元素的`onChange`事件。在事件回调函数中，我们判断上传的文件是否为图像文件，如果是，则创建一个新的 FileReader 对象，并调用它的`readAsDataURL`方法将文件转换为 Data URL。最后，我们使用 Image 的 CSS 属性将 Data URL 应用到预览区域的背景图片上，以显示图像预览。

### 实现多文件上传和预览

如果需要同时上传多个文件并预览它们，可以使用`multiple`属性将输入框设置为多选模式，并稍加修改 JavaScript 代码来处理多个文件。
以下是针对多文件上传和预览的JavaScript和CSS代码：

```JavaScript
// 获取文件上传表单中的 input 元素
var fileInput = document.getElementById('fileToUpload');
// 监听 input 元素的 onchange 事件
fileInput.addEventListener('change', function(event) {
  // 获取上传的文件
  var files = event.target.files;
  // 如果上传的文件是图像文件，则进行预览
  for (var i = 0; i < files.length; i++) {
    if (files[i].type.match('image.*')) {
      var reader = new FileReader();
      // 使用闭包保存每个 reader 对象的状态
      (function(reader) {
        // 将图像文件转换为 Data URL
        reader.readAsDataURL(files[i]);
        // 当读取完成时，显示预览图像
        reader.onload = function() {
          var imagePreviews = document.getElementById('imagePreviews');
          // 创建预览图像的容器和图像元素
          var previewContainer = document.createElement('div');
          previewContainer.classList.add('image-preview');
          var previewImage = document.createElement('img');
          previewImage.src = reader.result;
          // 将图像元素添加到容器中，并将容器添加到预览区域中
          previewContainer.appendChild(previewImage);
          imagePreviews.appendChild(previewContainer);
        }
      })(reader);
    }
  }
});
```

```Css
.image-preview {
    width: 200px;
    height: 200px;
    margin: 10px;
    background-color: #eee;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;
}
```

这个修改后的代码使用了一个`for`循环来迭代每个上传的文件。对于每个图像文件，我们创建一个新的`FileReader`对象并调用它的`readAsDataURL`方法将文件转换为 Data URL。最后，我们创建一个图片`img`元素，将 Data URL 设置为其源属性，并将图片元素包装在一个`<div>`元素中添加到预览区域中。
同时使用了一个闭包来保存每个 reader 对象的状态。在每个循环中，用一个匿名函数包装`reader`对象，以便在读取完成事件回调函数中访问正确的数据。这样每个`reader`对象的状态就不会在过程中被覆盖，每个图像文件都可以正确地显示预览了
