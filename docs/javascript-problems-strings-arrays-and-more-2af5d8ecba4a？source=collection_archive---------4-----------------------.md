# JavaScript 问题——字符串、数组等等

> 原文：<https://javascript.plainenglish.io/javascript-problems-strings-arrays-and-more-2af5d8ecba4a?source=collection_archive---------4----------------------->

![](img/1ae23975d655a67de7cbab7962ffd6c7.png)

Photo by [Susan Mohr](https://unsplash.com/@theinnervizion?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 计算字符串中的字符串出现次数

我们可以使用 regex 通过`match`方法找到一个给定字符串的所有出现。

例如，我们可以写:

```
const str = "foo bar baz";
const count = (str.match(/bar/g) || []).length;
```

我们在我们的`str`字符串中搜索所有出现的`'bar'`字符串。

# 重复字符 N 次

字符串有`repeat`方法多次重复字符串。

例如，我们可以写:

```
"foo".repeat(10)
```

重复`'foo'`十遍。

我们也可以写:

```
Array(11).join("foo")
```

我们需要 100，因为`join`将`'foo'`放在条目之间。

因此`repeat`可能更有意义，

# 最简单的数组交集代码

我们可以通过使用`filter`返回一个数组和包含在另一个数组中的条目来得到两个数组的交集。

例如，我们可以写:

```
array1.filter(value => array2.includes(value))
```

如果我们有数组`array1`和`array2`，那么我们使用`filter`返回`array1`的数组条目，它也在`array2`中，方法是在回调中使用`includes`方法。

这将返回一个新数组，该数组包含两个条目中的数组。

# 用 JavaScript 从元素中移除 CSS 类

使用`classList`属性的`remove`方法从元素中移除一个类。

例如，我们可以写:

```
element.classList.remove("foo");
```

假设元素`element`有`foo`类。

我们可以用参数`'foo'`调用`classList.remove`来移除它。

# 从日期中减去天数

我们可以用 JavaScript 从一个`Date`实例中减去天数。

为此，我们可以使用`getDate`方法来获取一个月中的某一天。

然后减去我们想要的天数。

例如，我们可以写:

```
const d = new Date();
d.setDate(d.getDate() - 5);
```

我们调用`setDate`通过用我们想要的数字改变一个月中的某一天来改变日期。

它不必在月份范围内，因为它会自动换行。

# 检查对象是否是 jQuery 对象

我们可以使用`instanceof`操作符来检查一个对象是否是`jQuery`构造函数的实例。

例如，我们可以写:

```
obj instanceof jQuery
```

现在我们可以在其中调用 jQuery 方法。

# 创建一个用零填充的数组

为了创建一个用零填充的数组，我们可以使用带有`fill`方法的`Array`构造函数。

例如，我们可以写:

```
new Array(len).fill(0);
```

其中`len`是数组的长度。

`fill`用 0 填充槽。

# 检查 JavaScript 对象是否是日期

我们可以用`instanceof`操作符检查一个 JavaScript 对象是否是一个`Date`实例。

例如，我们可以写:

```
date instanceof Date
```

如果对象没有跨越框架边界，这种方法有效。

如果一个`Date`实例跨越了框架边界，我们可以写:

```
Object.prototype.toString.call(date) === '[object Date]'
```

我们得到了`Date`实例的字符串表示。

# 强制刷新页面

我们可以设置代码的`Cache-Control`头。

例如，我们可以写:

```
Cache-Control: max-age=86400, must-revalidate
```

将缓存设置为持续 1 天，即 86400 秒。

我们可以使用以下命令禁用缓存:

```
Cache-Control: no-cache, must-revalidate
```

标题。

# 检查数组是否有特定的字符串

我们可以使用`indexOf`方法来检查一个数组是否有一个特定的字符串。

例如，我们可以写:

```
const arr = ["foo", "bar", "baz"];
const hasFoo = arr.indexOf("foo") > -1;
```

检查`arr`是否有串`'foo'`。-1 表示字符串不在`arr`中，

否则，返回字符串在数组中的索引。

如果我们不关心索引，还有`includes`方法。

我们可以写:

```
const arr = ["foo", "bar", "baz"];
const hasFoo = arr.includes("foo");
```

如果字符串在`arr`中，则返回`true`，否则返回`false`。

![](img/6b3a39b7faf8db7004aa47864fe485b7.png)

Photo by [Terricks Noah](https://unsplash.com/@major001?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在 web 主机中设置缓存头，以设置缓存何时过期。

同样，我们可以用内置操作符检查`Date`实例。

我们可以用数组方法得到数组交集。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**