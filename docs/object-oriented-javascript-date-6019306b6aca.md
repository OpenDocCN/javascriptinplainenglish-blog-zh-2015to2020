# 面向对象的 JavaScript —日期

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-date-6019306b6aca?source=collection_archive---------16----------------------->

![](img/5d3356d93e47ade8f9ad18f7661dd888.png)

Photo by [Wiktor Karkocha](https://unsplash.com/@rotkif?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看`Date`对象。

# 日期

`Date`构造函数让我们创建日期对象。

我们可以创建一个新的`Date`实例，不传递任何内容，只传递日期字符串、日、月、时间等的值。或者时间戳。

例如，要用当前日期创建日期，我们可以写:

```
new Date();
```

此外，我们可以传入一个日期字符串:

```
new Date('2020 1 1');
```

我们得到了:

```
Wed Jan 01 2020 00:00:00 GMT-0800 (Pacific Standard Time)
```

或者:

```
new Date('1 1 2020');
```

我们得到了:

```
Wed Jan 01 2020 00:00:00 GMT-0800 (Pacific Standard Time)
```

或者:

```
new Date('1 jan 2020');
```

我们得到了:

```
Wed Jan 01 2020 00:00:00 GMT-0800 (Pacific Standard Time)
```

`Date`构造函数可以从不同的字符串中计算出日期。

我们还可以将数值传递给`Date`构造函数来表示:

*   年
*   月，0 表示 1 月，11 表示 12 月
*   第 1 天到第 31 天
*   小时— 0 到 23
*   分钟— 0 到 59 分钟
*   秒— 0 到 59 秒
*   毫秒— 0 到 999

例如，我们可以写:

```
new Date(2020, 0, 1, 17, 05, 03, 120);
```

我们得到了:

```
Wed Jan 01 2020 17:05:03 GMT-0800 (Pacific Standard Time)
```

如果我们传入一个超出范围的数字，它将被调整到范围内。

例如，如果我们有:

```
new Date(2020, 11, 32, 17, 05, 03, 120);
```

然后我们得到:

```
Fri Jan 01 2021 17:05:03 GMT-0800 (Pacific Standard Time)
```

我们还可以传入一个时间戳。

例如，我们可以写:

```
new Date(1557027200000);
```

我们得到了:

```
Sat May 04 2019 20:33:20 GMT-0700 (Pacific Daylight Time)
```

如果我们在没有`new`的情况下调用`Date`，那么我们将得到一个表示当前日期的字符串。

我们是否传入参数并不重要。

所以如果我们有:

```
Date();
```

我们得到:

```
"Mon Aug 03 2020 15:02:32 GMT-0700 (Pacific Daylight Time)"
```

# 日期方法

我们可以用一些实例方法来调整日期。

例如，我们可以使用`getMonth()`、`setMonth()`、`getHours()`、`setHours()`等。设定日期的各个部分。

为了返回一个字符串，我们调用`toString`:

```
const d = new Date(2020, 1, 1);
d.toString();
```

然后我们得到:

```
"Sat Feb 01 2020 00:00:00 GMT-0800 (Pacific Standard Time)"
```

为了设置月份，我们调用`setMonth`:

```
d.setMonth(2)
```

这将返回新的时间戳:

```
1583049600000
```

为了获得人类可读的日期，我们可以调用`toString`:

```
d.toString();
```

并获得:

```
"Sun Mar 01 2020 00:00:00 GMT-0800 (Pacific Standard Time)"
```

我们可以用`Date.UTC`获得日期的时间戳:

```
Date.UTC(2029, 0, 1);
```

我们可以将它传递给`Date`构造函数:

```
new Date(Date.UTC(2029, 0, 1))
```

`now`方法也返回当前时间戳，所以我们写:

```
Date.now();
```

我们得到了:

```
1596492422408
```

我们可以用`valueOf`或`+`操作器做同样的事情:

```
new Date().valueOf();
+new Date();
```

并且它们都返回当前时间戳。

![](img/912e3c4ec648d34ae38b69e83e0c61ce.png)

Photo by [Lon Christensen](https://unsplash.com/@lonwake?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

日期可以用 JavaScript 的`Date`构造函数来操作。

我们可以在时间戳、日期对象和字符串之间进行转换。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**