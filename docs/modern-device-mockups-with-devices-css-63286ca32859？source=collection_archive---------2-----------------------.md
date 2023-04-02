# 带有 Devices.css 的现代设备模型

> 原文：<https://javascript.plainenglish.io/modern-device-mockups-with-devices-css-63286ca32859?source=collection_archive---------2----------------------->

## 使用纯 CSS 的现代设备模型

![](img/1633f791417baf69ca7569824c402080.png)

Devices.css 使开发人员能够轻松地将现代设备模型直接集成到他们的项目中。CSS 是一个展示应用程序、网页、截图等等的伟大的库。它目前支持一些著名的设备，如 iPhone X(基本上是 iPhone 11)、MacBook Pro、iPad Pro、Surface Book 等。

## 在项目中包含 Devices.css

要将 Devices.css 包含到您的项目中，只需从 [GitHub](https://github.com/picturepan2/devices.css) 中克隆存储库，并将`devices.css`或`devices.min.css`(如果您喜欢缩小的版本)复制到您的应用程序资产中。然后，确保像`<link rel=”stylesheet” href=”mypath/devices.css”>`一样将它包含在你的申请的`<head>`部分。您现在可以开始使用 Devices.css 了！

## 开始使用 Devices.css

现在它已经被添加到您的项目中，您所需要的就是几行 HTML 来开始使用 Devices.css。

```
<div class="device device-iphone-x">
  <div class="device-frame">
    <img class="device-content" src="assets/screenshot.jpg">
  </div>
  <div class="device-stripe"></div>
  <div class="device-header"></div>
  <div class="device-sensors"></div>
  <div class="device-btns"></div>
  <div class="device-power"></div>
</div> 
```

在浏览器中打开你的文件，你会看到你已经只用 HTML & CSS 创建了你的第一个 iPhone X 模型！

![](img/5ca72037b25b1013ddb0e8c339b67f45.png)

## 可定制性

注意，在最外层的 div 中，您可以通过遵循格式`device-[name-of-device]`将类名从`device-iphone-x`更改为任何其他设备来指定不同的设备(检查它们的 [GitHub](https://github.com/picturepan2/devices.css) 上支持的设备)。如果您想去掉设备的按钮、扬声器、前置摄像头等，也可以省略内部 div。

该库支持`device-frame` div 中的图像和视频。只需将`<img class="device-content" src="assets/screenshot.jpg">`更改为以下内容，您将拥有一个看似仿真的设备。

```
<video class="device-content" muted="muted">
  <source src="video.mp4" type=”video/mp4"/>
</video>
```

虽然 GitHub 页面上没有提到，但是你也可以在设备内部嵌入一个 iframe。只需将下面的 HTML 放入`device-frame`中。

```
<div class="device-content">
  <iframe src="[https://medium.com/@giuseppecampanelli](https://medium.com/@giuseppecampanelli)"/>
</div>
```

## 新设备支持

有提到添加新设备，但它似乎已经过时了，所以请随意+1 并评论我提出的问题[以添加新设备。](https://github.com/picturepan2/devices.css/issues/11)

请随意查看我用 Vue 创建的[演示](https://github.com/themilanfan/devices.css-demo)。