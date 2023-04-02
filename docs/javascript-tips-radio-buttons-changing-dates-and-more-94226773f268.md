# JavaScript 提示—单选按钮、更改日期等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-radio-buttons-changing-dates-and-more-94226773f268?source=collection_archive---------11----------------------->

![](img/e453bfdee0d720677115437eeb41a89e.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 在 Vue.js 中有条件地禁用输入

我们可以在 Vue.js 中通过设置元素的`disabled`属性来有条件地禁用输入。

为此，我们写道:

```
const app = new Vue({
  el: '#app',
  data: {
    disabled: false,
  },
});
```

然后我们写道:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17/vue.js"></script>
<div id="app">
  <button @click="disabled = !disabled">Toggle Enable</button>
  <input type="text" :disabled="disabled">
  <p>{{ $data }}</p>
</div>
```

当我们点击按钮时，我们切换`disabled`状态。

我们将`disabled`道具的值设置为`disabled`状态。

# 用 JavaScript 检查单选按钮是否被选中

要用 JavaScript 检查一个单选按钮是否被选中，我们可以获取一个元素的 checked 属性。

例如，如果我们有下面的 HTML:

```
<input type="radio" name="gender" id="male" value="Male" />
<input type="radio" name="gender" id="female" value="Female" />
```

然后我们可以写:

```
if (document.getElementById('male').checked) {
  // ...
}
else if (document.getElementById('female').checked) {
  // ...
}
```

我们只是得到无线电元素并检查`checked`属性。

# 在 for 循环中打印带有 setTimeout 的连续值

如果我们想在 for 循环中用`setTimeout`打印连续的值，那么我们必须使用`let`来声明索引变量。

`let`是块范围的，所以它被限制在循环中。

这意味着每次迭代都有一个不同的索引变量。

例如，我们可以写:

```
for (let i = 1; i <= 2; i++) {
  setTimeout(() => { console.log(i) }, 100);
}
```

# window.location.href 和 window.open()

`window.location.href`是一个让我们改变浏览器当前 URL 的属性。

更改该值将重定向页面。

`window.open()`是一个方法，让我们传递一个 URL 给它，在一个新窗口中打开它。

例如:

```
window.location.href = 'https://example.com';
```

会把我们带到[https://example.com。](https://example.com.)

另一方面，如果我们写:

```
window.open('https://example.com');
```

然后，我们将打开一个新的窗口与网址。

`window.open`可以对窗口进行多参数带设置。

# 替代已删除的 Lodash _。采摘方法

我们可以使用数组实例的`map`方法从每个条目中获取我们想要的属性。

例如，如果我们想获得每个条目的`id`属性，我们可以写:

```
const ids = arr.map(a => a.id)
```

然后，我们返回一个新数组，其中包含每个条目的`id`值。

# 用 Javascript 重复字符串

要重复一个字符串，我们可以使用字符串实例的`repeat`方法。

例如，如果我们想将一个字符串重复 3 次，我们可以这样写:

```
"foo".repeat(3);
```

然后我们得到:

```
"foofoofoo"
```

# 将 UTC 纪元转换为本地日期

我们可以通过创建一个`Date`实例将 UTC 纪元转换为本地日期。

然后我们用秒来调用`setUTCSeconds`来将秒设置为以秒为单位的 UTC 纪元。

例如，我们可以写:

```
const utcSeconds = 575849093;
const d = new Date(0);
d.setUTCSeconds(utcSeconds);
```

我们传入 0 来将`Date`设置为 UTC 纪元的开始。

然后我们设置秒来设置秒。

# 用 Javascript 获取一天的开始和结束

为了用 JavaScript 获得一天的开始和结束，我们可以在`Date`实例上写`setHours`。

例如，我们可以写:

```
const start = new Date();
start.setHours(0, 0, 0, 0);const end = new Date();
end.setHours(23, 59, 59, 999);
```

我们创建一个新的`Date`实例。

然后我们调用全零的`setHours`,将其设置为日期的开始。

然后，我们通过将时间设置为 23 小时、59 分钟、59 秒和 999 毫秒，将其设置为一天的结束。

我们可以在`start`和`end`上调用`toUTCString`来获得格式化的日期字符串。

# JavaScript 中的“elseif”语法

我们把`elseif`写成`else if`。

例如，我们写道:

```
if (foo) {} else if (bar) {} else {}
```

![](img/d1f2a8dccb3648df8d853491c8ffab5f.png)

Photo by [Alessandro Cerino](https://unsplash.com/@turalex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 Vue.js 和 props 有条件地禁用输入。

要获得单选按钮的选中值，我们可以使用`checked`属性。

我们可以将 UTC 纪元转换成秒。

我们可以设定日期的开始和结束。

`else if`是为条件分支写块的方式。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**