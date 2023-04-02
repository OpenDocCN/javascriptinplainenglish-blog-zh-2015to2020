# JavaScript 提示—文本框值、时区和循环

> 原文：<https://javascript.plainenglish.io/javascript-tips-textbox-values-timezones-and-loops-e9ce1e35cb0f?source=collection_archive---------10----------------------->

![](img/e129abc12fd9c03fab04e4510b655afd.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 检查文本框是否已更改

我们可以通过监听输入事件来检查文本框是否发生了变化。

例如，我们可以写:

```
$('#text-box').on('input', () => {
  // ...
});
```

我们调用`on`来监听`input`事件。

然后我们运行回调中的代码来做我们想做的事情。

# 使用 JavaScript 检测触摸屏设备

我们可以使用 Modernizr 库来检测一个设备是否是触摸屏设备。

它为我们提供了添加到`html`元素中的`touch`和`no-touch`类。

因此，我们可以根据设备是否是触摸屏来使用它做我们想做的任何事情。

# 在 React 中修改状态数组的正确方法

在 React 中修改状态数组有一个正确的方法。

如果我们正在使用类组件，我们通过传入一个接受先前值的回调来使用`setState`。

然后我们基于此返回一个新值。

例如，我们可以写:

```
this.setState(prevState => ({
  arr: [...prevState.arr, 'foo']
}))
```

我们添加了`'foo'`字符串作为`arr`状态的最后一个元素。

如果我们使用钩子，我们做同样的事情，但是我们把回调传递给状态改变函数。

例如，我们写道:

```
setArr(arr => [...arr, 'foo'])
```

假设`setArr`是`useState`返回的状态变化函数。

# 检查一个数字是否是 NaN

要检查一个数字是否是`NaN`，我们需要使用`isNaN`函数。

例如，我们可以写:

```
isNaN('foo')
```

它会通过将字符串转换为数字来进行检查，然后进行检查。

所以它将返回`true`，因为它将被转换为`NaN`。

# 在对象数组中查找属性的最大值

我们可以使用`Math.max`和 array `map`方法来查找对象数组中属性的最大值。

例如，我们可以写:

```
Math.max(...array.map(o => o.y))
```

我们通过调用`map`返回一个包含值的数字数组来获取条目的`y`属性的最大值。

然后我们使用 spread 操作符将数组条目作为参数传播到`Math.max`方法中。

# 如何切换布尔值

我们可以使用`!`操作符来切换布尔值。

我们只是写:

```
bool = !bool;
```

将`bool`改为与当前值相反的值。

# 创建具有设定时区的日期，而不使用字符串表示

我们可以通过使用`setUTCHours`方法设置`Date`实例的 UTC 小时来创建一个设置了时区的`Date`对象，而不使用字符串。

例如，我们可以写:

```
new Date().setUTCHours(2)
```

然后我们得到日期的时间戳，时间设置为给定的 UTC 小时。

我们还可以设置分钟、秒和毫秒值。

此外，我们可以写:

```
const date = new Date(Date.UTC(year, month, day, hour, minute, second))
```

`Date.UTC`返回带有 UTC 日期的时间戳，该日期设置为给定的年、月、日、小时、分钟和秒。

当我们将它传递给`Date`构造函数时，我们得到的是具有相同值的日期。

# 遍历键值对

为了遍历键值对，我们可以将`Object.entries`与 for-of 循环一起使用。

例如，我们可以写:

```
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

为了做到这一点。

`obj`是一个对象。

# 自动滚动到页面底部

要自动滚动到页面底部，我们可以使用`window.scrollTo`方法。

例如，我们可以写:

```
window.scrollTo(0, document.body.scrollHeight);
```

我们滚动到参数中给出的 x 和 y 坐标。

`document.body.scrollHeight`是页面的底部。

![](img/140cb9cf14d9701ae7249ebe6df51ef2.png)

Photo by [Jonas Weckschmied](https://unsplash.com/@jweckschmied?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

让我们滚动到页面上的任何地方。

在 React 中修改数组状态的正确方法是传入一个回调并返回一个从旧数组派生的新数组。

我们可以通过观察文本区域的输入事件来观察我们输入的值。

我们可以使用`Math.max`来寻找一个数字数组的最大值。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**