# 常见 JavaScript 日期格式任务的解决方案

> 原文：<https://javascript.plainenglish.io/solutions-to-common-javascript-date-formatting-tasks-8a77f0a8f3c6?source=collection_archive---------12----------------------->

![](img/144e991b8ca03541b0aa7eec6b28d612.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，`Date`构造函数为我们提供了一些日期和时间操作功能。然而，许多操作不能只用一个方法调用来完成。

在本文中，我们将研究一些解决我们中许多人都会遇到的常见日期处理问题的方法。

# 用 JavaScript 格式化本地日期

在 JavaScript 中有很多格式化日期的方法。JavaScript `Date`实例有许多内置方法，可以按照我们的意愿将日期格式化为字符串。

例如，如果我们有以下`Date`对象:

```
const date = new Date(2019, 0, 1);
```

然后我们可以调用下面的方法来按照我们希望的方式格式化日期。

我们可以这样称呼`toString`方法:

```
date.toString()
```

我们得到的是“2019 年 1 月 1 日星期二 00:00:00 GMT-0800(太平洋标准时间)”

`toTimeString`同，我们称之为:

```
date.toTimeString()
```

我们的时间是“00:00:00 GMT-0800(太平洋标准时间)”

`toUTCString`法，我们称之为:

```
date.toUTCString())
```

我们得到的是“2019 年 1 月 1 日星期二，08:00:00 GMT”。

还有`toDateString`法:

```
date.toDateString()
```

我们得到的是“2019 年 1 月 1 日星期二”。

用`toISOString`法:

```
date.toISOString()
```

我们得到“2019-01-01t 08:00:00.000 z”

`toLocaleString`为我们提供了一个带有 AM 和 PM 指示器的日期和时间的日期字符串，我们称之为:

```
date.toLocaleString()
```

我们得到的是“2019 年 1 月 1 日，12:00:00 AM”。

`toLocaleTimeString`方法从日期对象中获取时间，我们称之为:

```
date.toLocaleTimeString()
```

我们得到的是“12:00:00 AM”。

`getDate`方法得到的是月日，从 1 到 31 为月日。

例如，我们可以这样称呼它:

```
date.getDate()
```

然后我们得到 1。

`getDay`方法得到一周中的某一天，0 是星期天，6 是星期六。

我们可以这样称呼它:

```
date.getDay()
```

我们得到 2，这是星期二。

`getFullYear`方法得到整年的数字。例如，我们可以这样称呼它:

```
date.getFullYear()
```

得到 2019 年。

`getMonth`获取`Date`实例的月份，1 月从 0 开始，12 月从 11 开始。

例如，我们可以写:

```
date.getMonth()
```

一月份得到 0。

`getHours`方法从`Date`实例返回日期的小时，我们可以这样称呼它:

```
date.getHours()
```

获取 0，因为它是 0 小时，这是默认的小时。

同样，我们有`getMinutes()`和`getSeconds()`方法，分别是分和秒，得到分和秒。

我们可以这样称呼他们:

```
date.getMinutes()
date.getSeconds()
```

分别得到 0。

`getMilliseconds`获取在`Date`实例中设置的毫秒数，它相对于一天的开始。

当我们这样称呼它时:

```
date.getMilliseconds()
```

我们得到 0，因为它是默认值。

`getTime`方法获取自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的毫秒数。

当我们这样称呼它时:

```
date.getTime()
```

我们得到 1546329600000。

`getTimezoneOffset`方法以分钟为单位获取相对于 UTC 的时区差异。

例如，如果我们在太平洋时区，我们打电话给`date.getTimezoneOffset()`会得到 480 分钟，因为它比 UTC 晚 8 个小时。

![](img/33156ac9a276f341e90c77d2755f4a24.png)

Photo by [Luis Cortes](https://unsplash.com/@luiscortestamez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 用 JavaScript 格式化 UTC 日期

上述方法都依赖于当地时间，JavaScript 的`Date`构造函数也有返回 UTC 值而不是当地时区值的方法。

如果我们有下面的`date`对象:

```
const date = new Date(2019, 0, 1);
```

我们可以调用下面的 UTC 方法。

`getUTCDate`方法从`Date`实例返回一个月的日期。我们可以这样称呼它:

```
date.getUTCDate()
```

因为日期被设置为 1。

`getUTCDay`以 UTC 返回一周中的某一天，范围从 0 表示星期日到 6 表示星期六。

如果我们调用`date.getUTCDay()`，我们得到星期二的 2。

`getUTCFullYear`方法返回给定`Date`对象的完整年份。

如果我们打电话:

```
date.getUTCFullYear()
```

我们得到 2019 年。

`getUTCMonth`返回`Date`对象的月份，范围从 0 表示一月到 11 表示十二月。

如果我们打电话:

```
date.getUTCMonth() 
```

一月份我们得到 0。

`getUTCHours`获取一天中相对于当地时间的 UTC 时间。因此，如果我们从太平洋时区的设备中调用它，如下所示:

```
date.getUTCHours()
```

我们得到它返回 8，因为太平洋时间的 0 小时是 UTC 的上午 8 点。

`getUTCMinutes`、`getUTCSeconds`、`getUTCMilliseconds`返回在`Date`对象中设置的分、秒和毫秒。如果我们这样称呼它:

```
date.getUTCMinutes()
date.getUTCSeconds()
date.getUTCMilliseconds())
```

我们每个人得 0 分。

# 结论

有许多`Date`实例方法可以通过内置的日期方法获得日期的各个部分。在考虑使用图书馆之前，我们应该利用它们。

# **简明英语笔记**

您知道我们已经推出了一个 YouTube 频道吗？我们制作的每段视频都旨在教会您一些新东西。通过 [**点击这里**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 查看我们，一定要订阅这个频道😎