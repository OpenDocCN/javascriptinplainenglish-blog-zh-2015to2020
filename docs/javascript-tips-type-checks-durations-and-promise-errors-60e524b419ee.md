# JavaScript 提示—类型检查、持续时间和承诺错误

> 原文：<https://javascript.plainenglish.io/javascript-tips-type-checks-durations-and-promise-errors-60e524b419ee?source=collection_archive---------11----------------------->

![](img/e09264b1c265e643f1609dea38881e4c.png)

Photo by [Johann Siemens](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# typeof 和 instanceof 之间的区别

`instanceof`应该用于检查一个对象是否是一个构造函数的实例。

例如，我们可以写:

```
foo instanceof Foo;
```

检查`foo`是否是`Foo`构造函数的实例。

`typeof`应该用于检查原语类型和函数。

例如，我们可以写:

```
typeof 1 === 'number'
```

或者:

```
typeof () => {} === 'function'
```

他们都回到了`true`。

但是`typeof null`返回`'object'`，所以我们不能用`typeof`来检查`null`。

# 将 HTMLCollection 转换为数组

要将 HTMLCollection 转换为数组，我们可以使用`Array.from`方法或 spread 操作符将 HTMLCollection 转换为数组。

例如，我们可以写:

```
const arr = Array.from(htmlCollection);
```

或者:

```
const arr = [...htmlCollection];
```

这两种方法也适用于节点列表。

# 从 JavaScript 对象中移除项目

要从 JavaScript 对象中移除一个项目，我们可以使用`delete`操作符。

例如，我们可以写:

```
const obj = { foo: 1, bar: 2 };
delete obj.foo;
```

然后从`obj`中移除`foo`属性。

# 打开新窗口或标签

要打开一个新窗口或标签，我们可以使用`window.open`方法。

例如，我们可以写:

```
window.open('http://example.com', '_blank');
```

`'_blank'`使`window.open`打开一个新窗口。

# JavaScript 警告框中的换行符

为了在 JavaScript 警告框中显示一个新行，我们可以将新行字符添加到我们传递给`alert`的字符串中。

例如，我们可以写:

```
alert("line 1\nline 2");
```

然后我们显示 2 行文本，因为我们有了`\n`换行符。

# 在 JavaScript 中更改 span 元素的文本

我们可以通过改变`textContent`属性来改变 span 元素的文本。

例如，我们可以写:

```
document.getElementById("aSpan").textContent = "new stuff";
```

我们只需获取跨度并将`textContent`属性设置为一个新的字符串。

# 检查用户是否正在使用 IE

要检查用户是否在使用 IE，我们可以检查用户代理。

例如，我们可以写:

```
const ua = window.navigator.userAgent;
const ieIndex = ua.indexOf("MSIE");if (ieIndex >= 0 || !!ua.match(/Trident.*rv\:11\./))  {
  console.log('is IE');
}
else {
  console.log('not IE');
}
```

我们用`window.navigator.userAgent`得到用户代理。

然后我们在用户代理中获取`'MSIE'`字符串的索引，看看它是否在那里。

我们还会检查其中是否有单词`'Trident'`作为另一项检查。

两者结合起来，我们就可以检查一个浏览器是不是 IE 了。

# 获取 Moment.js 中两个日期之间的时差

要获得 moment.js 中两个日期之间的小时差异，我们可以使用`duration`和`diff`方法。

例如，我们可以写:

```
const duration = moment.duration(end.diff(start));
const hours = duration.asHours();
```

我们调用`duration`方法从`start`减去`end`得到持续时间，这两个都是矩对象。

然后我们调用`duration`上的`asHours`将持续时间转换为小时。

# 在 JavaScript 对象中以字符串形式通过名称访问变量属性

我们可以使用括号 nation 来访问带有变量的 JavaScript 属性。

例如，我们可以写:

```
const foo = obj['foo'];
```

来获取`obj`的`foo`属性。

嵌套属性可以用多个括号来访问。

例如，如果我们有:

```
const obj = { a: 1, b: { x: 2 }};
```

然后，我们可以通过编写以下内容来访问`x`:

```
const foo = obj['b']['x'];
```

# JavaScript 承诺——拒绝还是放弃

我们可以在承诺回调中使用`throw`。

否则，我们使用`reject`。

例如，我们可以写:

```
new Promise(() => {
  throw 'error';
})
```

我们可以在传递给`Promise`构造函数的回调中使用`throw`。

`throw`也终止控制流程。

我们可以在那里或其他任何地方使用`reject`:

```
new Promise((resolve, reject) => {
  reject('error');
})
```

我们调用`reject`，但是它后面的东西如果在的话还是可以运行的。

它们都可以用`catch`方法抓住。

# 结论

`typeof`和`instanceof`用于检查不同的东西。

我们可以用 moment.js 得到两次约会之间的持续时间。

html 集合和节点列表可以转换为数组。

我们可以检查用户代理来检查用户是否在使用 IE。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**