# 可维护的 JavaScript —应用程序逻辑和原始值检查

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-app-logic-and-primitive-value-check-5f15f68a5767?source=collection_archive---------12----------------------->

![](img/d1087d5e85511a4620b84cf02e97a6e8.png)

Photo by [Tim Johnson](https://unsplash.com/@mangofantasy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过将应用程序逻辑与事件处理分离来了解创建可维护的 JavaScript 代码的基础。

此外，我们看看如何检查原始值。

# 事件对象

当我们处理事件时，我们应该只在事件处理程序中保存`event`对象。

这将减少代码中的耦合。

因此，如果我们需要使用来自`event`对象的东西，我们将它们保存在事件处理程序中。

例如，我们可以写:

```
function onclick(event) {
  const {
    clientX,
    clienty,
    preventDefault,
    stopPropagation
  } = event;

  preventDefault();
  stopPropagation();
  showPopup(clientX, clienty);
}function showPopup(x, y) {
  const popup = document.getElementById("popup");
  popup.style.left = `${x}px`;
  popup.style.top = `${y}px`;
  popup.className = "popup";
}const button = document.querySelector('button');
button.addEventListener('click', onClick);
```

我们从事件处理程序中的`event`对象调用了`preventDefault`和`stopPropagation`方法。

这比传递`event`对象并在那个函数中调用它要好得多。

我们不希望我们的应用程序逻辑代码和事件处理代码混淆。

这样，我们可以独立地更改应用程序逻辑和事件处理代码。

而且我们可以直接调用 app 逻辑代码。

所以我们可以这样写:

```
showPopup(10, 10);
```

这也使得测试更加容易，因为我们可以直接调用`showPopup`来测试它。

# 避免空值比较

JavaScript 代码中一个常见的问题模式是根据`null`测试变量。

例如，我们可能有这样的代码:

```
function process(items) {
   if (items !== null) {
     items.sort();
     items.forEach((item) => {
       //...
     });
   }
 }
```

我们有一个`process`函数，它根据`null`来检查`items`。

然后我们假设`items`是一个数组，并对其调用`sort`和`forEach`。

与`null`比较并不意味着我们确定它是一个数组。

我们只知道不是`null`。

我们可以改进我们的价值比较，这样我们就可以真正检查我们想要什么。

# 检测原始值

JavaScript 中有几种原始值。

它们包括 bigints、strings、numbers、booleans、`null`和`undefined`。

`typeof`操作符是检查原始值类型的最佳操作符。

我们只需要把想要检查的表达式作为操作数传入。

对于字符串，`typeof`返回`'string'`。

对于数字，`typeof`返回`'number'`。

对于布尔值，`typeof`返回`'boolean'`。

对于 bigints，`typeof`返回`'bigint'`。

而对于`undefined`，`typeof`返回`'undefined'`。

`typeof`的基本语法是:

```
typeof variable
```

我们也可以写:

```
typeof(variable)
```

然而，第二种方法使`tyepof`看起来像一个函数。

因此，我们应该不用括号。

我们可以使用`tupeof`操作符来检查表达式值的类型，从而编写防御性的 bu 代码。

例如，我们可以写:

```
if (typeof name === "string") {
  let part = name.substring(3);
}
```

检查字符串。

我们可以写:

```
if (typeof count === "number") {
  setCount(count);
}
```

检查并设定一个数字。

并且:

```
if (typeof canShowMessage === "boolean" && canShowMessage) {
  showMessage("hello");
}
```

检查布尔值以及它是否是`true`。

我们可以写:

```
if (typeof app === "undefined") {
  app = {
    // ....
  };
}
```

来检查`undefined`。

![](img/cc10982fdaabf438e495c967a603e5df.png)

Photo by [Rami Al-zayat](https://unsplash.com/@rami_alzayat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该让我们的应用程序逻辑依赖于`event`对象。

此外，检查`null`并不足以检查大多数类型的数据。