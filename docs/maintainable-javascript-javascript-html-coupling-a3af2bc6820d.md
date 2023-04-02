# 可维护的 JavaScript——JavaScript/HTML 耦合

> 原文：<https://javascript.plainenglish.io/maintainable-javascript-javascript-html-coupling-a3af2bc6820d?source=collection_archive---------10----------------------->

![](img/6d40e71686e9072a86eec4005d7d6fe0.png)

Photo by [Everton Vila](https://unsplash.com/@evertonvila?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看 JavaScript 和 HTML 之间的松散耦合来了解创建可维护的 JavaScript 代码的基础。

# 让 JavaScript 远离 HTML

我们应该让 JavaScript 远离我们的 HTML 代码。

很难调试 HTML 文件中的 JavaScript 代码。

例如，如果我们有:

```
<button onclick="doSomething()">click me</button>
```

然后当我们点击按钮时，调用我们的`doSomething`函数。

`doSomething`可能在外部文件或脚本标签中。

当我们在功能可用之前点击按钮时，我们可能会遇到问题，这会导致 JavaScript 错误。

产生的错误可能会弹出，并导致按钮看起来什么也不做。

这也很难维护，因为更改名称意味着我们还必须在 HTML 文件中查找名称。

我们可能还想改变`onclick`属性中的代码

在 HTML 中嵌入 JavaScript 代码使得修改代码更加容易。

我们的大部分 JavaScript 代码应该在外部文件中，并包含在带有脚本标签的页面中。

`on`属性不应该用于将事件处理程序附加到 HTML 中。

相反，我们应该通过用 JavaScript 获取元素并附加侦听器来将事件侦听器附加到元素:

```
function onClick() {
  // ...
}const btn = document.querySelector("button");
btn.addEventListener("click", onClick, false);
```

我们将`onClick`函数设置为按钮的点击处理程序。

这样做的好处是`onClick`与附加事件处理程序的代码在同一个文件中定义。

如果函数需要改变，那么我们需要改变 JavaScript 文件。

如果按钮在被点击时应该做别的事情，那么我们只需要修改文件中的代码。

IE8 和更早的没有`addEventListener`方法。

相反，我们必须使用`attachEvent`方法。

使用 jQuery，我们可以编写:

```
$("button").on("click", onClick);
```

我们还可以在 HTML 中嵌入一个包含内嵌代码的脚本元素。

但这并不好，因为它将 JavaScript 代码保留在 HTML 中，并使代码变得更长。

例如，我们不应该编写这样的代码:

```
<script>
  doSomething();</script>
```

最好将所有 JavaScript 放在外部文件中，并将内联 JavaScript 放在我们的 HTML 之外。

这有助于我们更容易地调试代码。

当脚本错误发生时，我们知道错误在 JavaScript 文件中。

如果 JavaScript 代码在我们的 HTML 文件中，那么我们必须搜索这两种文件。

![](img/ed0d08b20606abc3128545f7e33d7873.png)

Photo by [Dương Hữu](https://unsplash.com/@huuduong?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该把 JavaScript 代码放在 HTML 代码之外，这样我们可以更容易地调试它们。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**