# JavaScript 问题—函数、对象属性和 NoScript

> 原文：<https://javascript.plainenglish.io/javascript-problems-functions-object-properties-and-noscript-989f95eccc17?source=collection_archive---------6----------------------->

![](img/17764da34f1c049d2b144f45b562a22b.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将 JavaScript 函数作为参数传递

要将一个函数作为参数传递给另一个函数，我们只需像任何参数一样编写它。

我们只是将一个函数的引用传递给另一个函数。

这意味着我们不叫它。

函数是 JavaScript 中的对象，所以它们可以被传递到任何地方。

例如，我们可以写:

```
function addItem(id, callback) {
  //...
}
```

我们可以通过编写以下代码将回调函数作为参数`callback`的值传入:

```
const callback = () => {
  //...
}
addItem(id, callback);
```

`callback`只是一个函数。

# 检测 JavaScript 是否被禁用

我们可以把我们的东西放在`noscript`标签里。

我们只是把东西放在那里。

例如，我们可以写:

```
<noscript>
  <p>no JavaScript</p>
</noscript>
```

如果浏览器中禁用了 JavaScript，标签之间的任何内容都会显示出来。

# 用 JavaScript 获取元素的类列表

要用 jQuery 获取元素中的类列表，我们可以获取 DOM element 对象的`className`属性。

然后我们可以通过用空格分割来得到类名。

例如，我们可以写:

```
const classList = document.getElementById('myDiv').className.split(/\s+/);
```

`split`使用一个正则表达式，我们可以通过它来划分类。

使用 jQuery，我们可以编写:

```
const classList = $('#myDiv').attr('class').split(/\s+/);
```

我们获取属性`class`的值，并以同样的方式对其调用`split`。

名称可以用任何类型的空格分隔，所以我们需要一个正则表达式，而不仅仅是`' '`字符串。

# 创建一个带有是或否选项的对话框

创建具有这些选项的对话框的最简单方法是使用`confirm`函数。

例如，我们可以写:

```
const yes = confirm('Are you sure');
```

如果我们点击确定，则`yes`将为`true`，否则为`false`。

将显示确认对话框的文本。

# 用 JavaScript 从文本中剥离 HTML

如果我们想获取一个 HTML 字符串并从文本中剥离 HTML，那么我们可以用 JavaScript 创建一个元素并将 HTML 字符串设置为`innerHTML`属性的值。

然后我们可以使用`textContent`或`innerText`属性来获取文本内容。

例如，我们可以写:

```
const tmp = document.createElement("div");
tmp.innerHTML = html;
const text =  tmp.textContent || tmp.innerText || "";
```

我们创建 div 元素。

然后我们将带有 HTML 代码的`html`字符串设置为`innerHTML`属性。

然后我们从最后一行得到文本。

# 需要中间值的承诺链

如果我们有一个承诺需要在调用它们之前从先前的承诺中解析出一个值，那么我们可以从先前的承诺中得到它。

然后，一旦我们在第二个承诺中有了数据，我们就可以调用不需要任何东西的承诺和需要该承诺的价值的承诺。

例如，我们可以写:

```
const a = promiseA(…);
const b = a.then((resultA) => {
  return promiseB();
});Promise.all([a, b])
.then(([resultA, resultB]) => {
  //...
});
```

我们有两个承诺`a`和`b`。`a`不依赖任何东西。

`b`需要来自`a`的结果，所以我们先调用`a`，然后在回调中调用`promiseB`。

一旦`b`获得了它需要的所有信息，那么我们就用`Promise.all`并行调用`a`和`b`并得到所有的值。

# React 中数组子项的唯一键

React 中数组的惟一键用于标识数组的惟一条目。

它必须是唯一的 ID，因为无论数组条目如何排序，它都必须相同。

出于同样的原因，嵌套条目也需要它们自己的惟一键。

# 对象属性顺序

自 ES2015 以来，对象属性的顺序遵循一些规则，但并不总是遵循插入顺序。

一个`Map`保证了键按照它们被插入的顺序被枚举。

自 ES2015 以来的规则是按升序排列整数键。

普通键按插入顺序排列。

符号也按插入顺序排列。

当我们使用`Object.assign`、`Object.defineProperties`、`Object.getOwnPropertyNames`、`Object.hetOwnPropetySymbols`和`Reflect.ownKeys`时，顺序是有保证的。

其他方法和 for-in 不保证任何顺序。

![](img/7e965c37a5d8cc7bb7db80a97b4d6dc5.png)

Photo by [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

自 ES2015 以来，对象属性顺序保持一致。

函数是对象，所以我们可以将 JavaScript 中的函数传递给另一个函数。

元素的文本内容可以通过`textContent`属性获得。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**