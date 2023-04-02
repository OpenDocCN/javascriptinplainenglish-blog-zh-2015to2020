# 现代 JavaScript 的精华——默认参数和 Rest

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-default-parameters-and-rest-d9ed49f46cc8?source=collection_archive---------15----------------------->

![](img/1b1d8264a838f3566e5a9fea4e6f55aa.png)

Photo by [Jan Padilla](https://unsplash.com/@janpadilla?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究如何使用参数和 rest 语法。

# 为什么`undefined`会触发默认值？

`undefined`表示某物不存在。

这与`null`不同，它表示一个空值。

因此，只有`undefined`会触发默认值的设置。

# 引用默认值中的其他参数

我们可以参考默认值中的其他参数。

例如，如果我们有:

```
function foo(x = 13, y = x) {
  console.log(x, y);
}
```

如果我们调用`foo();`，那么我们得到`x`和`y`都是 13。

如果我们调用`foo(7);`，那么我们得到`x`和`y`都是 7。

如果我们调用`foo(17, 12);`，那么我们得到`x`是 17，而`y`是 12。

# 在默认值中引用内部变量

如果我们有这样的代码:

```
const x = 'outer';function foo(a = x) {
  const x = 'inner';
  console.log(a);
}foo()
```

我们将外部变量赋值为参数的值，那么即使我们在内部定义了一个同名的变量，它也会引用外部变量。

我们将`a`的默认值赋给了`x`，所以即使我们用新值再次定义`x`，我们仍然得到`a`是`'outer'`。

如果函数上面没有`x`，我们将得到一个 ReferenceError。

当参数是函数时，这也适用于参数。

例如，如果我们有:

```
const bar = 2;function foo(callback = () => bar) {
  const bar = 3;
  callback();
}foo();
```

默认情况下，`callback`被分配给返回`bar`的函数，所以如果我们调用`callback`而没有回调函数传递给它，那么就会调用这个函数。

所以`callback`返回 2。

# 休息参数

Rest 参数让我们捕获没有设置任何参数的参数。

例如，如果我们有:

```
function foo(x, ...args) {
  console.log(x, args);
  //···
}
foo('a', 'b', 'c');
```

那么`x`就是`'a'`而`args`就是`['b', 'c']`。

如果没有剩余的参数，那么`args`将被设置为空数组。

这是对`arguments`对象的一个很好的替代。

在 ES5 或更早版本中，获取函数所有参数的唯一方法是使用`arguments`对象:

```
function logArgs() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}
```

对于 ES6 或更高版本，我们可以只使用 rest 参数来做同样的事情。

例如，我们可以写:

```
function logArgs(...args) {
  for (const arg of args) {
    console.log(arg);
  }
}
```

我们用 for-of 循环记录所有的参数。

# 组合析构和对析构值的访问

因为 rest 操作符给了我们一个数组，我们可以析构它。

例如，我们可以写:

```
function foo(...args) {
  let [x = 0, y = 0] = args;
  console.log(x, y);
}
```

我们将`args`的前 2 个条目分别设置为`x`和`y`。

我们将它们的默认值设置为 0。

析构也适用于对象参数。

例如，我们可以写:

```
function bar(options = {}) {
  const {
    x,
    y
  } = options;
  console.log(x, y);
}
```

我们有带`options`参数的`bar`函数。

我们将对象析构为`x`和`y`变量。

`arguments`是可迭代对象。

我们可以在 ES6 中使用 spread 操作符将其转换为数组。

例如，我们可以写:

```
function foo() {
  console.log([...arguments]);
}
```

那么数组将包含所有的参数。

![](img/9f1f0ed0372f026c369d787faab1001a.png)

Photo by [Brooke Cagle](https://unsplash.com/@brookecagle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 rest 参数将所有参数传递给函数。

我们也可以把其他值称为默认值。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**