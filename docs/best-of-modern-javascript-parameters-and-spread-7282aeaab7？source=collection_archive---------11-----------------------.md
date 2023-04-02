# 现代 JavaScript 的精华——参数和传播

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-parameters-and-spread-7282aeaab7?source=collection_archive---------11----------------------->

![](img/94ffc4b75dbfd8eb2a239c4ef615a738.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何使用对象和数组参数析构以及 spread 操作符。

# 可选参数

我们可以通过将参数设置为`undefined`来创建可选参数，以表明它是可选的。

例如，我们可以写:

```
function foo(x, optional = undefined) {
  //···
}
```

我们将`optional`设置为`undefined`,表示它是可选的。

# 必需的参数

如果我们有必需的参数，没有好的方法来确保它们适合 ES5。

例如，我们可能需要做一些事情，比如:

```
function foo(required) {
  if (required === undefined) {
    throw new Error();
  }
  //···
}
```

或者我们可以写:

```
function foo(required) {
  if (arguments.length < 1) {
    throw new Error();
  }
  //···
}
```

它们不太优雅。

但是，我们可以通过用 ES6 编写:

```
function checkRequired() {
  throw new Error();
}function foo(required = checkRequired()) {
  return required;
}
```

我们给参数分配了一个函数调用，这样当`required`为`undefined`时，它就会运行。

它抛出一个错误，所以当它是`undefined`的时候就很明显了。

这样，我们可以强制要求参数有一个值。

# 强制实施最大数量的参数

JavaScript 无法控制传递给函数的参数数量。

然而，在 ES6 中，我们可以通过检查 rest 操作符传入的参数数量来轻松做到这一点。

例如，我们可以写:

```
function foo(...args) {
  if (args.length > 2) {
    throw new Error();
  }
  //...
}
```

rest 操作符返回一个数组，所以我们可以用`length`属性检查它的长度。

如果参数比我们想要的多，那么我们可以抛出一个错误。

我们也可以写:

```
function foo(x, y, ...empty) {
  if (empty.length > 0) {
    throw new Error();
  }
}
```

以确保我们没有额外的参数。

# 扩展运算符

我们可以使用 spread 操作符将数组条目作为函数调用的参数进行传播。

例如，我们可以写:

```
Math.max(...[-1, 2, 3, 4])
```

这与以下内容相同:

```
Math.max(-1, 2, 3, 4)
```

我们可以用`push`方法做同样的事情。

例如，我们可以写:

```
const arr1 = [1, 2];
const arr2 = [3, 4];arr1.push(...arr2);
```

`arr2`作为`push`的自变量展开。

spread 运算符也适用于构造函数。

例如，我们可以写:

```
new Date(...[2020, 11, 25])
```

将参数传播到`Date`构造函数中。

spread 运算符也适用于数组。

例如，我们可以写:

```
[1, ...[2, 3], 4]
```

我们得到了返回的`[1, 2, 3, 4]`。

我们可以用它把数组合并成一个。

例如，我们可以写:

```
const x = [1, 2];
const y = [3];
const z = [4, 5];const arr = [...x, ...y, ...z];
```

我们将`x`、`y`和`z`数组展开成数组。

那么`arr`就是 `[1, 2, 3, 4, 5]`，因为条目被分散到新的数组中。

![](img/53cd4f60555b4478ff00f502eccffd06.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过各种方式添加可选的和必需的参数。

此外，我们可以使用 spread 操作符将数组分布在不同的位置。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**