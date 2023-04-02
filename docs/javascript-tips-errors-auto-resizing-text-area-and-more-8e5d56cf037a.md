# JavaScript 提示-错误、自动调整文本区域大小等

> 原文：<https://javascript.plainenglish.io/javascript-tips-errors-auto-resizing-text-area-and-more-8e5d56cf037a?source=collection_archive---------15----------------------->

![](img/e2a77370295ac162f9c15bec8fc3ca53.png)

Photo by [Soledad Lorieto](https://unsplash.com/@sool_lorieto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 替换字符串中的所有逗号

我们可以用`replace`方法替换字符串中的所有逗号。

例如，我们可以写:

```
const str = 'foo,bar,baz';
const newStr = str.replace(/,/g, '-');
```

那么`newStr`就是`“foo-bar-baz”`。

# 扩展 JavaScript 中错误的好方法？

我们可以用`extends`关键字扩展`Error`类。

例如，我们可以写:

```
class SpecialError extends Error {
  constructor(message) {
    super(message);
    this.name = 'special error';
  }
}
```

我们创建了一个`SpecialError`类，它继承了`Error`类。

我们可以设置自己的`name`以区别于`Error`实例。

# 用逗号将字符串转换为数组

我们可以用`split`方法将带逗号的字符串转换成数组。

例如，我们可以写作；

```
const string = "1,2,3";
const array = string.split(",");
console.log(array);
```

我们有一个`string`，我们称之为`split`，用逗号将字符串分割成一个数组。

# 添加或更新查询字符串参数

要添加或更新查询字符串参数，可以使用该查询字符串创建`URLSearchParams`实例。

然后我们可以用键和值在上面调用`set`来设置参数。

例如，我们可以写:

```
const searchParams = new URLSearchParams(window.location.search)
searchParams.set("foo", "baz");
const newURL = `${window.location.pathname}?${searchParams.toString()}`;
history.pushState(null, '', newURL);
```

我们在浏览器搜索栏中用`window.location.search`得到查询字符串。

然后我们用它的键和值来调用`set`。

接下来，我们通过调用`URLSearchParams`实例上的`toString`得到查询字符串。

我们将路径名和查询字符串结合在一起。

然后我们用新的 URL 调用`pushState`来用新的查询字符串重新加载页面。

# 在新窗口而不是选项卡中打开网址

为了总是在新窗口而不是标签中打开一个 URL，我们可以将第三个参数传递给`window.open`窗口的宽度和高度。

例如，我们可以写:

```
window.open(url, '_blank', "height=200,width=200");
```

第三个参数有窗口的规格。

# 检查变量是否为数字

我们可以通过`isNaN`和`isFinite`功能来检查变量是否是数字。

例如，我们可以写:

```
!isNaN(parseFloat(num)) && isFinite(num);
```

我们使用`parseFloat`试图将`num`解析成一个浮点数。

然后我们可以用`isNaN`来检查返回值。

我们还通过`num`到`isFinite`来检查这一点。

如果两个表达式都是`true`，那么我们知道我们得到了一个有限的数。

# 创建自动调整大小的文本区域

为了创建一个自动调整大小的文本区域，我们可以监听输入事件。

然后我们可以将输入事件监听器中文本区域的高度设置为`scrollHeight`加 12 个像素。

`scrollHeight`获取文本的全高。

为此，我们可以写:

```
<textarea></textarea>
```

创建文本区域。

然后我们可以写:

```
function resizeTextarea(ev) {
  this.style.height = 'auto';
  this.style.height = `${this.scrollHeight}px`;
}
const textArea = document.querySelector('textarea');
textArea.addEventListener('keyup', resizeTextarea);
```

`'auto'`将高度重置为原始高度。

然后我们将高度设置为`scrollheight` ，使其成为文本的高度。

# 为什么我们不应该使用 document.write？

我们不应该使用`document.write`在屏幕上渲染 HTML。

它不适用于 XHTML。

它在页面完成加载并覆盖页面后运行。

此外，它会在遇到它的地方运行。

它不像我们处理 DOM 那样写内容，而且很容易产生错误，因为我们是在页面上写字符串的内容。

因此，我们不应该使用`document.write`将内容写到屏幕上。

# 检查 URL 是否有给定的字符串

我们可以在`window.location.href`上使用`indexOf`方法来检查 URL 是否包含给定的字符串。

例如，我们可以写:

```
window.location.href.indexOf("foo") > -1
```

检查`'foo'`是否在当前页面的 URL 中。

如果返回值是-1，这意味着它不在 URL 中。

否则，它就在 URL 中。

![](img/457bbf4e4063fae0788371c7b87ec444.png)

Photo by [David Dibert](https://unsplash.com/@dibert?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`URLSearchParams`构造函数来解析和操作查询字符串。

`window.open`可以打开一个新窗口。

`Error`构造函数可以用子类扩展。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**