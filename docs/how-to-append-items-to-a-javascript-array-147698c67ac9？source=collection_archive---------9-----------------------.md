# 如何将项目追加到 JavaScript 数组中？

> 原文：<https://javascript.plainenglish.io/how-to-append-items-to-a-javascript-array-147698c67ac9?source=collection_archive---------9----------------------->

![](img/47ace1b2dcf1ac59df3585c030b5b890.png)

Photo by [Aleksandr Eremin](https://unsplash.com/@notevilbird?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，有几种方法可以将项目追加到数组中。

在这篇文章中，我们将看看如何以不同的方式做到这一点。

# 数组.原型.推送

`push`方法将一个条目追加到一个数组中。

我们可以通过书写来使用它:

```
arr.push('foo');
```

将`'foo'`加到`arr`上。

此外，我们可以用它向一个数组添加多个元素:"

```
arr.push('foo', 'bar');
```

然后`'foo'`和`'bar'`都以同样的顺序加到`arr`上。

这也意味着我们可以使用 spread 操作符将 then 条目添加到带有`push`的数组中:

```
arr.push(...['foo', 'bar']);
```

它的作用与前面的例子相同。

# 传播算子

我们可以使用 spread 操作符创建一个新的数组，并在其中添加新的条目。

例如，我们可以写:

```
arr = [...arr, 'foo', 'bar'];
```

我们使用 spread 操作符将现有的条目`arr`扩展到一个新的数组中。

然后我们在最后给它加了`'foo'`和`'bar'`。

然后我们将新数组赋回给`arr`。

# 数组.原型.拼接

同样，我们可以使用 array 实例的`splice`方法来追加到一个数组中。

它需要 3 个或更多的参数，即开始索引、删除计数和我们想要添加的项目。

要将项目添加到数组中，我们可以编写:

```
arr.splice(arr.length, 0\. 'foo', 'bar');
```

在上面的代码中，我们用`arr.length`表示我们想从数组+ 1 的最后一个索引开始。

0 表示我们不删除任何内容。

`'foo'`和`'bar'`是我们添加的项目。

# 将项目设置为长度索引

JavaScript 数组不是固定长度的，所以我们可以给任何索引分配一个条目。

如果我们想将一个条目添加到一个数组中，那么我们可以使用括号符号来设置它，如下所示:

```
arr[arr.length] = 'foo';
```

然后，我们可以将`'foo'`附加到`arr`上，因为`arr.length`是原始数组结束索引之后的一个索引。

# 数组.原型.串联

我们可以使用数组实例的`concat`方法将一个或多个条目添加到数组中。

例如，我们可以写:

```
arr = arr.concat('foo', 'bar');
```

此外，我们可以写:

```
arr = arr.concat(...['foo', 'bar']);
```

无论哪种方式，除了现有的参数之外，我们都要为要添加到新数组中的项目传入一个或多个参数。

`concat`返回一个新数组，所以我们必须将它赋给一个变量来访问它。

# 变异与否

我们最好不要改变数组，这样我们就可以保持现有的值。

测试和跟踪不可变数据更容易。

因此，如果可能的话，最好不要改变数据。

![](img/4e35a1c7346fccf94d242c9ea7cc5cc8.png)

Photo by [Meriç Dağlı](https://unsplash.com/@meric?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 JavaScript 中，有很多方法可以将一个项目附加到一个数组中。

我们可以使用 spread 操作符，也可以使用各种数组方法来做同样的事情。

有些操作符或方法会变异，有些不会，所以我们在使用它们时必须小心。

## 坦白地说

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**