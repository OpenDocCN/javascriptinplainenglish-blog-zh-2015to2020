# 有用的 JavaScript 技巧——时间戳、组合值和调用函数

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-timestamps-combining-values-and-calling-functions-70af74f583f4?source=collection_archive---------5----------------------->

![](img/cb01f8fcbb54b5370c9fe0695905485e.png)

Photo by [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 在 JavaScript 中提取 UNIX 时间戳

我们可以用`+`操作符获得一个`Date`实例的 UNIX 时间戳。

例如，我们可以写:

```
+new Date(2020, 0, 1)
```

然后我们得到 1577865600000。

同样，我们可以使用`getTime`方法来做同样的事情:

```
new Date(2020, 0, 1).getTime()
```

然后我们得到同样的回报。

如果日期是 YYYY-MM-DD 格式，也可以解析。

所以我们可以写:

```
new Date('2020-01-01').getTime()
```

我们也得到了同样的东西。

时间戳是从 UTC 时间 1970 年 1 月 1 日 12 点开始的毫秒数。

所以我们可以将返回的数字除以 1000，转换成秒。

# 将数组值组合成一个

我们可以使用`reduce`方法将数组条目减少到一个结果中。

例如，如果我们有:

```
const cart = [{price: 10}, {price: 40}, {price: 20}];
```

那么我们从`cart`得到的总价格可以写成:

```
const reducer = (total, item) => total + item.price;const total = cart.reduce(reducer, 0);
```

那么`total`就是 70，因为我们通过将所有`cart`条目的`price`属性的值相加来组合它们。

# 检测文档是否准备好

我们可以用`document.readyState`属性检测一个页面是否被完全加载。

例如，我们可以写:

```
if (document.readyState === 'complete') {
  //...
}
```

来检测一个页面是否被完全加载，然后做一些事情。

我们还可以使用以下方法定期检查状态:

```
let stateCheck = setInterval(() => {
  if (document.readyState === 'complete') {
    clearInterval(stateCheck);
    // document ready
  }
}, 100);
```

然后，我们可以每隔 100 毫秒检查一次页面的加载状态，直到它准备好。

在`if`块中，我们调用`clearInterval`来结束计时器，因此我们将停止检查`readyState`。

此外，我们可以通过以下方式监听`readystatechange`事件:

```
document.onreadystatechange = () => {
  if (document.readyState === 'complete') {
    // document ready
  }
}
```

这比创建一个计时器并运行它直到就绪状态改变要好。

# 计算数组的最大值或最小值

我们可以向`Math.max`或`Math.min`方法中传递尽可能多的值，以检查数组的最大值或最小值。

例如，我们可以写:

```
const max = Math.max(1, 2, 3);
```

返回最大值或:

```
const min = Math.min(1, 2, 3);
```

返回最小值。

要传入数组，我们可以使用 spread 运算符:

```
const max = Math.max(...[1, 2, 3]);
```

或者:

```
const min = Math.min(...[1, 2, 3]);
```

或者我们可以使用`apply`如下:

```
const max = Math.max.apply(undefined, [1, 2, 3]);
```

或者:

```
const min = Math.min.apply(undefined, [1, 2, 3]);
```

`apply`将`this`的值作为第一个参数，由于`Math.max`和`Math.min`是静态方法，所以第一个参数是`undefined`。

第二个参数是一个参数数组，我们希望将它作为参数传入数组。

# 函数参数中的析构

我们可以用析构语法来析构对象参数。

例如，我们可以写:

```
const greet = ({ name, lastName }) => {
  console.log(`hi, ${name} ${lastName}`);
};
```

我们通过析构赋值获得对象的`name`和`lastName`属性。

然后我们可以通过调用:

```
greet({ name: 'jane', lastName: 'doe' })
```

我们还可以为析构变量设置一个默认值。

例如，我们可以写:

```
const greet = ({ name = 'jane', lastName = 'smith' }) => {
  console.log(`hi, ${name} ${lastName}`);
};
```

那么当我们用文字称之为:

```
greet({})
```

那么`name`就是`'jane'`，而`lastName`就是`'smith'`，因为我们没有设置这两个属性的值。

# 防止改变内置原型

我们可以调用`Object.freeze`来阻止改变对象，包括内置的原型。

所以我们可以写:

```
Object.freeze(Object.prototype);
Object.freeze(Array.prototype);
Object.freeze(Function.prototype);
```

以防止改变任何内置原型。

![](img/389929cacaf63c2deaec9ccde8eb7961.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Object.freeze`防止改变内置原型。

此外，我们可以使用 spread 操作符或`apply`来调用带有一组参数的函数。

还有各种方法可以获得 UNIX 时间戳。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**