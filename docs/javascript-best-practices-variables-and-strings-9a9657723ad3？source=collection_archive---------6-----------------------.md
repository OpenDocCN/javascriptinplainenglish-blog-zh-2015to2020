# JavaScript 最佳实践—变量和字符串

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-variables-and-strings-9a9657723ad3?source=collection_archive---------6----------------------->

![](img/a5797951971ad554b506048e5edc5f43.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 变量和函数命名约定

我们应该有清晰的变量名和函数名。

在 JavaScript 中，除了常量和构造函数之外，名字通常是 camelCase。

例如，我们可以写:

```
const firstName = 'james';
```

我们用以下方式命名常数:

```
const MAX_RETRIES = 50;
```

我们用 PascalCase 命名构造函数和类:

```
class Dog {}
```

# 使用全局变量

我们希望避免创建全局变量。

它们随处可见，因此可以被任何东西修改。

这导致了难以追踪的问题。

所以要像模块一样用其他方式共享变量。

# 功能

函数应该一次做一件事。

如果它必须做更多的事情，那么我们应该把它分离成一个助手函数。

# 渐进增强

我们不应该假设每个人都启用了 JavaScript。

为了向没有启用 JavaScript 的用户显示一些东西，我们可以在`noscript` 标签中为他们放置一些东西。

# 初始化后不要改变变量类型

为了使我们的生活更容易，我们不应该在变量初始化后改变变量类型。

例如，我们不应该写:

```
let status = "Success";
status = 1;
```

更改类型会增加调试的难度。

相反，我们可以将不同类型的值赋给不同的变量。

# 使用内嵌注释

我们可以在代码中添加行内注释来进行注释。

我们应该把它们保持在一行。

例如，我们可以写:

```
addtoCart(order) // add to cart
```

# 控制变量范围

为了控制变量范围，我们可以使用`let`和`const`。

它们都是块范围的，所以不会对它们在哪里可用产生任何混淆。

例如，我们可以写:

```
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

我们使用`let`来定义`i`，这样它只在循环中可用。

# JavaScript 是区分大小写的

我们应该记住 JavaScript 是一种区分大小写的语言。

所以`foo`和`Foo`不一样。

这适用于一切。

所以我们应该确保我们的代码有正确的大小写。

# 从右端遍历数组

我们可以用数组实例的`slice`方法从右端遍历数组。

例如:

```
const arr = ['a', 'b', 'c', 'd', 'e', 'f'];
console.log(arr.slice(-1));
```

返回`['f']`和:

```
console.log(arr.slice(-2));
```

返回`['e', 'f']`以此类推。

`slice`不修改被调用的数组。

# 用 map()替换循环

如果我们想将数组条目从一个东西映射到另一个东西，我们不必使用循环。

相反，我们使用`map`实例方法。

例如，我们可以写:

```
const itemsIds = items.map((item) => {
  return item.id;
});
```

从`items`数组中的项目获取 id。

# 替换所有出现的字符串

我们可以用`g`标志调用`replace`来替换一个字符串的所有出现。

例如，我们可以写:

```
const str = "foo foo bar baz";  
console.log(str.replace(/foo/g, "qux"));
```

然后`‘qux qux bar baz’`又回来了。

![](img/87a2fb13e012e2ec35958a39564b0d6b.png)

Photo by [Nikolay Tchaouchev](https://unsplash.com/@nxn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该知道许多技巧来改进我们的 JavaScript 代码。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**