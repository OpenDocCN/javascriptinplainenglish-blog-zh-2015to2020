# JavaScript 提示—类型脚本接口检查、显示/隐藏元素等等

> 原文：<https://javascript.plainenglish.io/javascript-tips-typescript-interface-checks-showing-hiding-elements-and-more-b8ebe77a2b8c?source=collection_archive---------6----------------------->

![](img/f508e0c60e74e5f9ea917e6341dd3cd5.png)

Photo by [redcharlie](https://unsplash.com/@redcharlie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# NPM 包装上“at”(@)前缀的含义

前缀`@`意味着这个包是一个作用域包。

这意味着带有`@`前缀的作品是包的名称空间。

例如，`@angular/http`的名称空间是`angular`。

限定范围的包不会出现在公共搜索中。

其中许多是作为私有包创建的。

# 用 JavaScript 编写内联 if 语句

我们可以通过使用 Javascript 中的`?`来编写内联`if`语句。

例如，我们可以写:

```
(a < b) ? 'bigger' : 'smaller'
```

`a < b`是布尔表达式。

如果是`true`，那么我们返回`'bigger'`。

否则，我们返回`'smaller'`。

# 使用 Typescript 进行接口类型检查

我们可以用`in`操作符检查一个对象是否匹配一个接口。

例如，我们可以写:

```
interface A {
  name: string;
}
```

然后我们可以创建以下函数:

```
function instanceOfA(object: any): object is A {
  return 'name' in object;
}
```

我们把返回类型设为`instanceOfA` `object is A`。这样，我们返回`object`是否匹配一个接口。

然后我们可以调用如下函数:

```
if (instanceOfA(obj)) {
  //...
}
```

# 在 JSON 中处理换行符

如果我们的 JavaScript 对象中有换行符，那么我们需要在 JSON 字符串中对它们进行转义。

否则，我们会得到一个未终止的字符串文字错误。

例如，不写:

```
const data = '{ "count" : 1, "foo" : "text\n\n" }';
```

我们写道:

```
const data = '{ "count" : 1, "foo" : "text\\n\\n" }';
```

# 创建 ID 为的元素

我们可以通过在`createElement`方法之后使用`setAttribute`方法来创建一个带有 ID 的元素。

例如，我们可以写:

```
const div = document.createElement('div');
div.setAttribute("id", "foo");
```

# 用 JavaScript 隐藏或显示元素

要隐藏一个元素，我们可以将元素的`style.display`属性设置为`'none'`来隐藏一个元素。

例如，我们可以写:

```
document.getElementById('foo').style.display = 'none';
```

隐藏 ID 为`'foo'`的元素。

如果我们想显示一个元素，我们可以将`style.display`设置为`'block'`。

所以我们可以写:

```
document.getElementById('foo').style.display = 'block';
```

# 对于某些文字，instanceof 返回 false

如果我们使用`instanceof`来检查原始文字的类型，我们将得到所有原始文字的`false`。

例如，如果我们有:

```
"foo" instanceof String
```

然后那个会返回`fale`。

同样，`false instanceof Boolean`也返回`false`。

为了检查原始文字的类型，我们应该使用`typeof`操作符。

例如，我们可以写:

```
typeof str === 'string'
```

检查`str`是否为原始字符串。

我们可以对数字和布尔这样的原始类型做同样的事情。

# 将多个元素推入数组

我们可以用`push`将多个项目推送到一个数组中。

例如，我们可以写:

```
const arr = [];
arr.push(1, 2, 3);
```

我们用 3 个数字调用 push，它们都会被相加。

同样，如果我们有一个数组，我们可以使用`apply`方法。

例如，我们可以写:

```
arr.push.apply(arr, [1, 2, 3]);
```

或者:

```
Array.prototype.push.apply(arr, [1, 2, 3])
```

做同样的事情。

此外，我们可以使用 spread 运算符:

```
arr.push.apply(...[1, 2, 3]);
```

它们都将数组条目作为参数传入。

# 制作一个 HTML 反向链接

要创建一个指向前一个 URL 的 HTML 链接，我们可以写:

```
<a href="javascript:history.back()">Go Back</a>
```

当链接被点击进入前一页时，我们调用`history.back()`。

此外，我们可以写:

```
<a href="#" onclick="history.go(-1)">Go Back</a>
```

做同样的事情。

# currentTarget 属性和 Target 属性之间的差异

由于事件冒泡，`currentTarget`和`target`属性之间存在差异。

`target`是触发事件的元素。

`currentTarget`是事件监听器附加到的元素。

![](img/248e8cf46c5743e6d71979e68bb938f2.png)

Photo by [Mitchell Hollander](https://unsplash.com/@mitchures?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用前缀`@`创建命名空间包。

同样，我们可以用多个参数调用`push`。

`instanceof`不能用于检查原始值。

显示和隐藏项目可以用 JavaScript 来完成。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**