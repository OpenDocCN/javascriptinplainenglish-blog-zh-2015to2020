# 有用的 JavaScript 技巧——检查字符串和样式元素

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-checking-strings-and-styling-elements-3910a2bb1799?source=collection_archive---------7----------------------->

![](img/44f7c14e403452ce130c946932edd7e5.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 获取子字符串在字符串中的最后一个匹配项

`lastIndexOf`方法返回子串在字符串中最后一次出现的索引。

例如，我们可以写:

```
'jane, joe, and joe'.lastIndexOf('joe')
```

然后我们得到 15。

如果没有找到子串，则返回-1。

# 获取子字符串在字符串中的第一个匹配项

`indexOf`方法返回子字符串在字符串中第一次出现的索引。

例如，我们可以写:

```
'jane, joe, and joe'.indexOf('joe')
```

然后我们得到 6。

如果没有找到子串，则返回-1。

# 检查子字符串是否在字符串中

我们可以用`includes`方法检查一个子串是否在一个字符串中。

例如，我们可以写:

```
'jane, joe, and alex'.includes('joe')
```

然后返回`true`。

否则，如果我们有:

```
'jane, joe, and alex'.includes('foo')
```

然后它返回`false`。

它还需要第二个参数来指示我们希望从哪个索引开始搜索。

例如，我们可以写:

```
'jane, joe, and alex'.includes('joe', 10)
```

然后返回`false`。

# 检查字符串是否以子字符串结尾

`endsWith`方法让我们检查一个字符串是否以子字符串结尾。

例如，我们可以写:

```
'jane, joe, and alex'.endsWith('alex')
```

然后返回`true`。

我们也可以传入一个整数来搜索给定长度的子串。

例如，我们可以写:

```
'jane, joe, and alex'.endsWith('alex', 5)
```

然后它返回`false`，因为`'alex'`不在字符串的前 5 个字符中。

# 组合两个或多个字符串

`concat`方法让我们组合两个或更多的字符串。

例如，我们可以写:

```
'jane'.concat(' ', 'smith');
```

为了得到`'jane smith'`。

此外，我们可以写:

```
'jane'.concat(' ').concat('smith');
```

得到同样的东西。

# 获取字符的 Unicode 字符代码

`charCodeAt`方法让我们获得一个字符的字符代码。/

例如，如果我们有:

```
'a'.charCodeAt(0)
```

然后我们得到`97`。

我们也可以使用`codePouintAt`方法:

```
'a'.codePointAt(0)
```

得到同样的结果。

# 通过索引从字符串中获取字符

`charAt`方法让我们通过字符的索引来获取字符。

例如，我们可以写:

```
'jane'.charAt(0)
```

返回`'j'`。

# 查找与给定模式匹配的子字符串的位置

`search`方法让我们搜索具有给定模式的子串。

例如，我们可以写:

```
'jane, joe, and alex'.search('jane')
```

那么我们得到 0。

我们也可以使用正则表达式进行搜索。

例如，我们可以写:

```
'jane, joe, and alex'.search(/\b[a-z]{3}\b/)
```

然后我们得到 6。

如果没有找到匹配，则返回-1。

# 用另一个字符串替换具有给定模式的字符串

`replace`方法允许我们用给定的子串替换给定模式的子串。

例如，我们可以写:

```
'jane, joe, and alex'.replace(/\b[a-z]{3}\b/, 'may')
```

然后我们得到`“jane, may, and alex”`。

# 列出对象的所有方法

借助`Object.keys`和`typeof`算子，我们可以得到一个物体的所有方法。

例如，我们可以写:

```
const getMethods = (obj) => Object.keys(obj).filter(key => typeof obj[key] === 'function')
```

`Object.keys`只获取对象的非继承字符串属性。

如果我们有:

```
const person = {
  name: 'james',
  eat() {},
  drink() {}
}
```

那么我们可以这样称呼`getMethods`:

```
getMethods(person)
```

并得到`[“eat”, “drink”]`。

# 为 DOM 元素设置样式

我们可以用各种方式来设计元素的风格。

我们可以在元素中添加或删除类。

此外，我们还可以向其中添加内嵌样式。

假设我们有以下元素:

```
const element = document.querySelector('#foo')
```

我们可以使用元素的`classList`属性添加和删除类:

```
element.classList.add('fancy')
element.classList.remove('fancy')
```

此外，我们还可以通过以下方式访问`style`属性。

我们可以写道:

```
element.style.color = 'green';
```

添加和删除类是首选，因为它比动态更改样式更快。

![](img/e71cf8f86fb661ba9939565d36495b3c.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`styles`属性为对象添加样式。

`classList`属性允许我们添加或删除元素中的类。

有字符串方法可以从字符串中获取我们需要的信息。

此外，我们可以替换和搜索子字符串。

## 简单英语中的 JavaScript

你知道我们有四个出版物和一个 YouTube 频道吗？在 [**找到他们。点击**](https://plainenglish.io/) 和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**