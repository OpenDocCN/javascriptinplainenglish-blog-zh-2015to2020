# ES2018 的最佳特性— Regex

> 原文：<https://javascript.plainenglish.io/best-features-of-es2018-regex-b04f98dd4bed?source=collection_archive---------12----------------------->

![](img/744c5310cfdc20fd90ce8e6a8750901e.png)

Photo by [James Bold](https://unsplash.com/@jamesbold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了很大的改进。

现在使用它比以往任何时候都愉快。

在本文中，我们将了解 ES2018 的最佳功能。

# 编号的捕获组

编号的捕获组让我们可以用正则表达式将字符串分开。

这是获得团体比赛的老方法。

与字符串匹配的正则表达式返回匹配的对象。

然后，我们可以通过它的索引得到每个部分。,

例如，我们可以写:

```
const PHONE_REGEX = /([0-9]{3})-([0-9]{3})-([0-9]{4})/;const matchObj = PHONE_REGEX.exec('123-456-7890');
```

我们有 3 个抓捕小组。2 个 3 位数字，1 个 4 位数字。

然后我们可以根据每个组的索引得到它。

命名捕获组的问题是我们必须计算括号。

我们必须从正则表达式模式中知道这些组是什么。

如果我们改变正则表达式，就必须改变代码。

# 命名的捕获组

S2018 为我们提供了一种获取组匹配的新方法。

例如，我们可以写:

```
const PHONE_REGEX = /(?<areaCode>[0-9]{3})-(?<officeCode>[0-9]{3})-(?<number>[0-9]{4})/;const matchObj = PHONE_REGEX.exec('123-456-7890');
console.log(matchObj.groups)
```

我们把名字加到了组里。

然后我们可以在`groups`属性中找到匹配项。

所以我们会得到:

```
{ areaCode: "123", officeCode: "456", number: "7890" }
```

作为`macthObj.groups`的值。

这意味着我们可以破坏匹配:

```
const PHONE_REGEX = /(?<areaCode>[0-9]{3})-(?<officeCode>[0-9]{3})-(?<number>[0-9]{4})/;const matchObj = PHONE_REGEX.exec('123-456-7890');
const {
  areaCode,
  officeCode,
  number
} = matchObj.groups;
```

命名的捕获组很好，因为更容易识别捕获组。

匹配的代码是描述性的，因为 ID 描述了它捕获的内容。

我们可以在不改变匹配代码的情况下改变模式。

名字使模式更容易理解。

# 回溯参考

我们可以在正则表达式中通过名称引用匹配的组。

例如，我们可以写:

```
const REGEX = /^(?<word>[abc]+)\k<word>$/;const matchObj = REGEX.exec('abcabc');
console.log(matchObj.groups)
```

然后用`marchObj.groups.words`拿火柴。

`word`具有相同的模式。

# `replace()`

我们可以对命名的捕获组使用`replace`。

例如，我们可以写:

```
const PHONE_REGEX = /(?<areaCode>[0-9]{3})-(?<officeCode>[0-9]{3})-(?<number>[0-9]{4})/;console.log('123-456-7890'.replace(PHONE_REGEX,
  '$<areaCode>$<officeCode>$<number>'))
```

我们用正则表达式调用`replace`，用我们想要格式化字符串的模式调用一个字符串。

我们只需获取命名的捕获组，并在没有空格的情况下组合它们。

所以我们得到:

```
'1234567890'
```

回来了。

它还接受一个回调，该回调将组作为参数之一:

```
const PHONE_REGEX = /(?<areaCode>[0-9]{3})-(?<officeCode>[0-9]{3})-(?<number>[0-9]{4})/;console.log('123-456-7890'.replace(PHONE_REGEX,
  (g0, ac, oc, num, offset, input, {
    areaCode,
    officeCode,
    number
  }) => `${areaCode}${officeCode}${number}`))
```

最后一个参数有组。

并且，`ac`、`oc`、`num`为组。

我们返回一个新的字符串，这样它就是`replace`返回的字符串。

`g0`已满串。

`offset`指定匹配的位置。

`input`有完整的输入字符串。

# 不匹配的命名组

如果我们有不匹配的命名组，那么属性将是`groups`属性中的`undefined`。

![](img/9eaa87e084e34f6c3c4e3603e2af3959.png)

Photo by [Devin Justesen](https://unsplash.com/@devjustesen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

命名捕获组便于命名匹配项，因此我们可以更容易地理解它们，而不必更改匹配代码。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)