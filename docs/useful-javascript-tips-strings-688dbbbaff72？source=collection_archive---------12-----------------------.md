# 有用的 JavaScript 技巧—字符串

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-strings-688dbbbaff72?source=collection_archive---------12----------------------->

![](img/437fc03f102f60a71bfaca48f94193d2.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 将字符串转换为大写

我们可以调用`toUpperCase`将字符串字符改为大写。

例如，我们可以写:

```
'foo'.toUpperCase()
```

然后返回`'FOO'`。

# 将字符串转换成小写

要将一个字符串转换成小写，我们可以使用`toLowerCase`方法。

例如，我们可以写:

```
'FOO'.toLowerCase()
```

然后返回`'foo'`。

# 以区分区域设置的方式将字符串转换为大写

我们可以使用`toLocaleUpperCase`方法将一个字符串转换成一个区分地区的大写字符串。

例如，我们可以写:

```
'foo'.toLocaleUpperCase('tr');
```

然后我们得到`“FOO”`。

# 以区分区域设置的方式将字符串转换为小写

有一个`toLocaleLowerCase`方法可以将一个字符串转换成一个区分地区的小写字符串。

例如，我们可以写:

```
'FOO'.toLocaleLowerCase('tr');
```

然后我们得到`“foo”`。

# 从字符串中提取子字符串

为了从字符串中提取子串，我们可以使用`substring`方法。

它接受一个字符串的开始和结束索引。

例如，我们可以写:

```
'jane, joe, and alex'.substring(5, 10);
```

并且返回 `“ joe,”`。

指数也可以是负数。

所以我们可以写:

```
'jane, joe, and alex'.substring(-10, 4);
```

并得到`'jane'`。

负索引从-1 开始，它是最后一个字符的索引，-2 是第 2n 个最后一个字符的索引，依此类推。

# 检查字符串是否以什么开头

字符串有一个`startsWith`方法，我们可以用它来检查一个字符串是否以给定的子字符串开始。

例如:

```
'jane, joe, and alex'.startsWith('jane');
```

返回`true`。

另一方面；

```
'jane, joe, and alex'.startsWith('foo');
```

返回`false`。

它还需要第二个参数来使`startsWith`从给定的索引开始搜索。

例如，我们可以写:

```
'jane, joe, and alex'.startsWith('jane', 5);
```

然后`startsWith`从索引 5 开始搜索。

# 根据分隔符拆分字符串

我们可以根据一个分隔符调用`split`。

例如，我们可以写:

```
'jane, joe, and alex'.split(' ');
```

然后我们根据空格字符分割字符串。

所以我们得到`[“jane,”, “joe,”, “and”, “alex”]`作为结果。

# 提取一段字符串

`slice`方法可以帮助从字符串中提取子串。

它需要一个开始和结束索引。

例如，我们可以编写以下代码:

```
'jane, joe, and alex'.slice(5, 10)
```

然后返回字符串`“ joe,”`。

它还接受负索引，其中-1 是最后一个字符，-2 是倒数第二个字符，依此类推。

例如，我们可以写:

```
'jane, joe, and alex'.slice(-10, -5)
```

并且它返回`“, and”`。

# 使字符串重复

方法可以让一个字符串重复。

例如，我们可以写:

```
'fooo'.repeat(3)
```

然后我们得到`“fooofooofooo”`。

# 向字符串的开头添加字符

`padStart`方法接受一个参数作为填充后字符串的目标长度。

第二个参数是用来填充字符串的字符串。

例如，我们可以写:

```
‘foo’.padStart(8, ‘abcd’)
```

然后它返回`“abcdafoo”`,因为它用重复的`'abcd'`填充`'foo'`,直到返回的字符串有 8 个字符。

# 向字符串末尾添加字符

`padEnd`方法接受一个参数作为填充到末尾的字符串的目标长度。

第二个参数是用来填充字符串的字符串。

例如，我们可以写:

```
‘foo’.padEnd(8, ‘abcd’)
```

然后我们返回`“fooabcda”`，因为我们将字符串添加到了字符串的末尾。

# 移除多余的 Unicode 字符

我们可以使用`normalize`删除多余的字符，用单个字符替换它们。

例如，我们可以写:

```
'\u1E9B\u0323'.normalize()
```

然后我们得到`“ẛ̣”`。

# 查找匹配给定正则表达式模式的字符串

`match`方法返回给定正则表达式模式的匹配。

例如，我们可以写:

```
'foo'.match(/(foo|bar)/)
```

那么它将返回`[ "foo", "foo" ]`。

![](img/aaa890203d02af8f3f0d945c3746b51e.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用区域设置比较来比较字符串

`localeCompare`以区分区域设置的方式比较字符串。

例如，我们可以写:

```
'a'.localeCompare('à', 'fr-ca')
```

则返回-1，因为`‘à’`在`'a'`之后。

# 结论

我们可以用字符串方法做很多事情。

我们可以比较字符串并将它们填充到给定的长度。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道，解码**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**