# JavaScript 技巧—禁用 div、动态脚本元素等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-disabling-divs-dynamic-script-elements-and-more-af9dcbf18ffb?source=collection_archive---------3----------------------->

![](img/af3f6ee5edbbdd7d38855edc70ac9f11.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 如何禁用所有 div 内容

我们可以使用`pointer-events: none`样式来禁用 div 元素上的鼠标交互。

因此，我们可以编写以下代码，用 JavaScript 禁用 div 上的交互:

```
$("div").addClass("disabled");
```

那么`disabled`类将具有以下样式:

```
.disabled {
  pointer-events: none;
  opacity: 0.5;
}
```

我们禁用鼠标交互，并使 div 内容半透明。

# 用 async-await 语法拒绝承诺

如果我们想拒绝一个异步函数返回的承诺，我们可以抛出一个错误或者使用`Promise.reject`。

例如，我们可以写:

```
const asyncFn = async () => {
  try {
    const val = await foo();
    //...
  }
  catch(error) {
    throw new Error('error');
  }
}
```

然后返回的承诺被拒绝，原因是`'error'`。

同样，我们可以用`Promise.reject`来写:

```
const asyncFn = async () => {
  try {
    const val = await foo();
    //...
  }
  catch(error) {
    return Promise.reject(new Error('error'));
  }
}
```

在`catch`块中，我们返回一个被`Promise.reject`方法拒绝的承诺。

我们仍然以原因`'error'`传入`Error`实例。

# 用 JavaScript 计算昨天的日期

我们可以用`getDate`方法计算昨天的日期来返回当月的日期

然后我们从中减去 1，并将其传递给`setDate`方法。

为此，我们写道:

```
const date = new Date();
date.setDate(date.getDate() - 1);
```

# 追加

为了动态地向 DOM 添加一个脚本元素，我们可以调用`createElement`来创建一个元素。

然后我们可以调用`appendChild`来追加新创建的脚本元素。

例如，我们可以写:

```
const script = document.createElement("script");
script.type = "text/javascript"; 
script.text = "console.log('hello');"
document.body.appendChild(script);
```

我们用内嵌代码创建了一个脚本。

然后我们调用`appendChild`来追加脚本。

此外，我们可以编写以下内容来追加链接的脚本:

```
const script = document.createElement("script");
script.type = "text/javascript"; 
script.src   = "/script.js";
document.body.appendChild(script);
```

我们追加`script.js`来设置脚本的路径。

其他都一样。

# 获取鼠标点击画布元素的坐标

要获得鼠标点击画布元素的坐标，我们可以使用`clientX`和`clientY`属性。

例如，我们可以写:

```
const getCursorPosition = (canvas, event) => {
  const rect = canvas.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;
  console.log(x, y);
}
```

我们可以使用`getBoundingClientRect`方法来获得画布的`left`和`top`偏移量。

我们分别从`clientX`和`clientY`中减去它们，得到鼠标在画布中的坐标。

那么我们可以通过编写来使用这个函数:

```
const canvas = document.querySelector('canvas')
canvas.addEventListener('mousedown', (e) => {
  getCursorPosition(canvas, e)
})
```

# 检查 JavaScript 字符串是否是 URL

我们可以使用`URL`构造函数来检查 JavaScript 字符串是否是有效的 URL。

例如，我们可以写:

```
const isValidUrl = (string) => {
  try {
    new URL(string);
  } catch(e) {
    return false;  
  }
  return true;
}
```

我们尝试创建一个`URL`实例，看看它是否是一个 URL。

如果是，那么不会抛出异常，我们返回`true`。

否则，我们在`catch`块中返回`false`，因为当`URL`构造函数试图解析一个无效的 URL 字符串时会抛出一个异常。

# 使用 encodeURI 或 encodeURIComponent 对 URL 进行编码

`encodeURI`假设输入是我们作为参数传入的完整 URI。

用特殊的含义编码一切。

例如，我们可以写:

```
encodeURIComponent('&');
```

并返回`“%26”`。

我们可以写:

```
encodeURI('http://example.com/?foo= bar')
```

并得到`“http://example.com/?foo=%20bar"`。

![](img/d8e6d73ddd7b92cbdec8e470daaf2495.png)

Photo by [Ashwini Chaudhary](https://unsplash.com/@suicide_chewbacca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些 CSS 禁用 div 交互性。

在异步函数中有两种方法返回被拒绝的承诺。

`URL`构造函数消除了解析 URL 字符串的困难。

脚本标签可以动态插入。

我们可以用一些属性来检查鼠标在画布上的位置。

可以用`Date`实例方法计算昨天的日期。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**