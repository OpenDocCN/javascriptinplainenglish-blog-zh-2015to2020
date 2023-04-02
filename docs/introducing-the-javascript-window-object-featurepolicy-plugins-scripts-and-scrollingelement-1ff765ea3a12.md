# JavaScript 窗口对象简介—功能策略、插件、脚本和滚动元素

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-featurepolicy-plugins-scripts-and-scrollingelement-1ff765ea3a12?source=collection_archive---------4----------------------->

![](img/d55f5efe4f3eb21b2d7409b60ab6319b.png)

Photo by [Gareth Harper](https://unsplash.com/@garethharper?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。

在本文中，我们将研究一下`featurePolicy`、`plugins`、`scripts`和`scrollingElement`属性。

# 窗口.文档.插件

`plugins`属性是一个只读属性，它返回一个 HTMLCollection 对象，该对象包含文档的一个或多个`embed`元素。

HTMLCollection 是一个类似数组的对象，它包含我们视频的`embed`元素。

HTMLCollection 对象有一个`item`方法，让我们通过索引获取 HTMLCollection 对象中的 DOM 元素。此外，由于它是一个类似数组的对象，我们可以在`for...of`循环中使用它。例如，如果我们在 HTML 代码中嵌入了以下视频:

```
<embed type="video/webm" src="[https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4)" width="250" height="200"><embed type="video/webm" src="[https://media.w3.org/2010/05/sintel/trailer.mp4](https://media.w3.org/2010/05/sintel/trailer.mp4)" width="250" height="200">
```

然后我们可以用`for...of`遍历它们，就像我们处理下面的代码一样:

```
for (const embed of document.plugins) {
  console.log(embed);
}
```

如果我们运行上面的代码，我们将获得 web 页面中的`embed`元素。我们还可以获得`embed`元素的属性，如以下代码所示:

```
for (const embed of document.plugins) {
  const {
    height,
    src,
    type,
    width
  } = embed;
  console.log(height, src, type, width);
}
```

然后我们在我们的`console.log`输出中取回属性值:

```
200 [https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4](https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_30mb.mp4) video/webm 250200 [https://media.w3.org/2010/05/sintel/trailer.mp4](https://media.w3.org/2010/05/sintel/trailer.mp4) video/webm 250
```

我们可以用 HTMLCollection 对象可用的`item`方法通过索引获取元素来替换`for...of`:

```
console.log(document.embeds);
for (let i = 0; i < document.plugins.length; i++) {
  const {
    height,
    src,
    type,
    width
  } = document.plugins.item(i);
  console.log(height, src, type, width);
}
```

然后我们应该得到与上一个例子相同的`console.log`输出。如果没有`embed`元素，则返回`null`。

# window . document . feature 策略

`featurePolicy`是一个只读属性，它返回`FeaturePolicy`对象，让我们检查应用于特定文档的特性策略。

`FeaturePolicy`对象有一些方法。`allowsFeature`方法让我们检查单个功能是否打开。如果在特定上下文中启用，它将返回一个布尔值`true`，否则返回`false`。例如，如果我们想检查`geolocation`是否在我们特定的浏览器选项卡中启用，我们可以写:

```
const policy = document.featurePolicy;
console.log(policy.allowsFeature('geolocation'));
```

它还将原始 URL 的第二个可选参数作为一个字符串，这样我们就可以检查某个特性是否可以在各种网站上使用。例如，我们可以写:

```
const policy = document.featurePolicy;
console.log(policy.allowsFeature('geolocation', '[https://jsfiddle.net/'](https://jsfiddle.net/')));
```

`features`方法以字符串数组的形式返回浏览器用户代理支持的特性列表。列出的功能在当前上下文中可能是允许的，也可能是不允许的，因为浏览器中设置的权限阻止了这些功能的使用。例如，我们可以写:

```
const policy = document.featurePolicy;
console.log(policy.features());
```

然后，我们应该会得到浏览器中可以启用的功能的完整列表。例如，在 Chrome 中，我们得到:

```
["geolocation", "midi", "focus-without-user-activation", "camera", "usb", "fullscreen", "vr", "magnetometer", "picture-in-picture", "accelerometer", "autoplay", "document-domain", "encrypted-media", "ambient-light-sensor", "gyroscope", "payment", "sync-xhr", "microphone"]
```

`allowedFeatures`方法以字符串数组的形式返回允许在当前浏览器上下文中运行的所有特性的指令名列表。它应该是由`features`方法返回的特征的子集。例如，我们可以这样写:

```
const policy = document.featurePolicy;
console.log(policy.allowedFeatures());
```

那么我们应该得到这样的结果:

```
["geolocation", "midi", "camera", "fullscreen", "picture-in-picture", "document-domain", "encrypted-media", "payment", "sync-xhr", "microphone"]
```

从`console.log`输出。

`getAllowlistForFeature`方法让我们获得特定特性的允许列表。它有一个参数，是这个特性的字符串，我们希望得到允许使用你传入的特性的域。

例如，我们可以将从`allowedFeatures`方法返回的特性列表中的一个特性作为`getAllowListForFeature`方法的参数，如下面的代码所示:

```
const policy = document.featurePolicy;
console.log(policy.getAllowlistForFeature('geolocation'));
```

那么我们应该得到这样的结果:

```
["[https://fiddle.jshell.net](https://fiddle.jshell.net)"]
```

作为`console.log`输出返回。如果我们将返回数组中的域传递给`allowedFeature`方法的第二个参数，那么它应该返回`true`。例如，如果我们有:

```
const policy = document.featurePolicy;
console.log(policy.allowsFeature('geolocation', '[https://fiddle.jshell.net'](https://fiddle.jshell.net')));
```

那么上面的`console.log`应该会返回`true`。

![](img/aee28d3c0ddb1a6e2a4ab2e3e36e6bb9.png)

Photo by [James Barnett](https://unsplash.com/@jimo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 窗口.文档.脚本

`scripts`属性是一个只读属性，它返回一个 HTMLCollection 对象以及文档的脚本列表。

HTMLCollection 对象是一个类似数组的对象，我们可以用`for...of`循环遍历它，或者用 spread 操作符将其转换为数组。

例如，如果我们的 HTML 文档中有下面的`script`标签:

```
<script src="[https://code.jquery.com/jquery-3.3.1.slim.min.js](https://code.jquery.com/jquery-3.3.1.slim.min.js)" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script><script src="[https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js](https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js)" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script><script src="[https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js](https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js)" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

然后我们可以在下面的代码中循环类似的内容:

```
for (const script of document.scripts) {
  console.log(script);
}
```

这应该会让我们回到:

```
(index):33 <script src=​"https:​/​/​code.jquery.com/​jquery-3.3.1.slim.min.js" integrity=​"sha384-q8i/​X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin=​"anonymous">​</script>​<script src=​"https:​/​/​cdnjs.cloudflare.com/​ajax/​libs/​popper.js/​1.14.7/​umd/​popper.min.js" integrity=​"sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin=​"anonymous">​</script>​<script src=​"https:​/​/​stackpath.bootstrapcdn.com/​bootstrap/​4.3.1/​js/​bootstrap.min.js" integrity=​"sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/​nJGzIxFDsf4x0xIM+B07jRM" crossorigin=​"anonymous">​</script>​
```

从`console.log`输出。我们还可以使用 HTMLCollection 对象的`item`方法通过索引获取每个链接。为此，我们可以编写以下代码:

```
for (let i = 0; i < document.scripts.length; i++) {
  console.log(document.scripts.item(i));
}
```

两个循环应该以相同的方式输出链接。我们还可以通过属性链接直接获取链接的属性，如下面的代码所示:

```
for (const script of document.scripts) {
  const {
    src,
    integrity
  } = script;
  console.log(src, integrity);
}
```

然后，我们从`console.log`语句中得到类似下面的内容:

```
[https://code.jquery.com/jquery-3.3.1.slim.min.js](https://code.jquery.com/jquery-3.3.1.slim.min.js) sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo[https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js](https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js) sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1[https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js](https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js) sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM
```

# window . document . scrolling element

`scrollingElement`属性是一个只读属性，返回滚动元素的`Element`对象。在标准模式下，这应该是文档或`document.documentElement`对象的根元素。

例如，如果我们有:

```
console.log(document.scrollingElement);
```

然后我们应该得到如下输出所示的`html`元素:

```
<html><head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <title></title>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
  <meta name="robots" content="noindex, nofollow">
  <meta name="googlebot" content="noindex, nofollow">
  <meta name="viewport" content="width=device-width, initial-scale=1"><script type="text/javascript" src="/js/lib/dummy.js"></script><link rel="stylesheet" type="text/css" href="/css/result-light.css"><style id="compiled-css" type="text/css">

  </style><!-- TODO: Missing CoffeeScript 2 --><script type="text/javascript">//<![CDATA[window.onload=function(){

console.log(document.scrollingElement);}//]]></script></head>
<body><script>
    // tell the embed parent frame the height of the content
    if (window.parent && window.parent.parent){
      window.parent.parent.postMessage(["resultsFrame", {
        height: document.body.getBoundingClientRect().height,
        slug: ""
      }], "*")
    }// always overwrite window.name, in case users try to set it manually
    window.name = "result"
  </script></body></html>
```

有了`document`对象，我们就有了一些方便的属性，可以通过使用它的方便属性来获取文档中的元素。`document.plugins` 对于获取文档的`embed`元素很方便。

`document.featurePolicy`让我们知道浏览器中有哪些功能以及启用了哪些功能。功能包括地理定位、虚拟现实等。

属性为我们获取文档的所有标签。它返回一个 HTMLCollection，在这里我们可以通过使用属性名作为元素的属性来获取元素的属性，从而在我们的代码中获取属性值。

`scrollingElement`让我们得到`Element`对象，该对象滚动标准模式下应该是`documentElement`的元素。