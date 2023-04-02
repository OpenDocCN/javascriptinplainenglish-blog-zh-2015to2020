# 面向对象的 JavaScript —代理、函数和内置对象

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-proxies-functions-and-built-in-objects-eece7e0ccaee?source=collection_archive---------17----------------------->

![](img/54525dcc5caec84936b4a7c3cf22f477.png)

Photo by [William Bayreuther](https://unsplash.com/@wbayreuther?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究带有代理和内置浏览器对象的 JavaScript 元编程。

# 代理和函数陷阱

我们可以用`get`和`apply`操作拦截函数调用。

例如，我们可以通过编写以下代码来创建一个处理程序:

```
const person = {
  name: "james",
  foo(text) {
    console.log(text);
  }
}const proxy = new Proxy(person, {
  get(target, propKey, receiver) {
    const propValue = target[propKey];
    if (typeof propValue !== "function") {
      return propValue;
    } else {
      return function(...args) {        
        return propValue.apply(target, args);
      }
    }
  }
});proxy.foo('james')
```

我们有用`get`方法控制的`person`对象。

`target`是我们控制的对象。

在本例中，它是`person`对象。

`propKey`是属性键。

我们检查属性是否是一个函数。

如果不是，我们返回它的任何值。

否则，我们调用带有参数的`this`为`target`的函数。

我们返回结果。

# 浏览器环境

浏览器环境由 JavaScript 控制。

它是 JavaScript 程序的天然宿主。

# 在 HTML 页面中包含 JavaScript

我们可以用 Script 标签在 HTML 页面中包含 JavaScript。

例如，我们可以写:

```
<!DOCTYPE>
<html><head>
    <title>app</title>
    <script src="script.js"></script>
  </head> <body>
    <script>
      let a = 1;
      a++;
    </script>
  </body></html>
```

我们有一个将`src`设置为`script.js`的脚本标签。

我们还有一个内嵌的脚本，里面有 JavaScript 代码。

# BOM 和 DOM

页面上的 JavaScript 代码可以访问各种对象。

它们包括核心 ECMAScript 对象，如`Array`、`Object`等。

DOM 是与当前页面相关的对象。

BOM 有处理页面之外的所有东西的对象，比如浏览器窗口和桌面屏幕。

DOM 在各种 W3C 规范中被标准化。

但是 BOM 没有任何标准。

# 月初（beginning of month 的缩写）

BOM 是对象的集合，它让我们可以访问浏览器和计算机屏幕。

这些对象可通过全局`window`对象访问。

# 窗口对象

`window`对象是浏览器环境提供的全局对象。

例如，我们可以通过编写以下代码来添加一个全局变量:

```
window.foo = 1;
```

那么`foo`就是 1。

一些核心 JavaScript 函数是`window`对象的一部分。

例如，我们可以写:

```
parseInt('123');
```

然后我们得到 123。

它与以下内容相同:

```
window.parseInt('123');
```

这些可能因浏览器而异，但它们在所有主流浏览器上都得到了一致和可靠的实现。

# window.navigator 属性

属性有一些关于浏览器及其功能的信息。

一个是`navigator.userAgent`属性。

例如，我们可以写:

```
window.navigator.userAgent
```

然后我们得到用户代理字符串。

我们可能会得到这样的字符串:

```
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.105 Safari/537.36 Edg/84.0.522.52"
```

回来了。

我们可以用它来检查哪个浏览器。

例如，我们可以写:

```
if (navigator.userAgent.indexOf('MSIE') !== -1) {
  //...
} else {
  //...
}
```

检查脚本是否在 Internet Explorer 中运行。

但是这是一种检查浏览器功能的糟糕方法，因为用户代理可能被欺骗。

我们只需直接检查所需功能:

```
if (typeof window.addEventListener === 'function') {
  // ...
} else {
  // ...
}
```

![](img/bcebf3c78a7615101eceafc6e3dc133c.png)

Photo by [Randy Fath](https://unsplash.com/@randyfath?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

浏览器有各种我们可以使用的内置对象。

此外，我们可以使用代理来控制函数。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**