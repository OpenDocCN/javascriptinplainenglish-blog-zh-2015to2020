# 更好的 JavaScript —函数调用

> 原文：<https://javascript.plainenglish.io/better-javascript-function-calls-3c94143ba624?source=collection_archive---------14----------------------->

![](img/6ef0acd055c269895f1d528e2f552885.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 使用 apply 调用具有不同数量参数的函数

`apply`方法让我们调用一个值为`this`的函数和一个带参数的数组。

例如，我们可以写:

```
const min = Math.min.apply(null, [1, 2, 3]);
```

第一个参数是`this`的值，它可以是任何值，因为`Math.min`是一个静态方法。

剩下的就是论据了。

这让我们可以调用一个参数数量可变的函数。

我们也可以通过从函数中获取值来使用它。

例如，我们可以写:

```
const buffer = {
  state: [],
  append(...args) {
    for (const a of args) {
      this.state.push(a);
    }
  }
};
```

我们得到了`args`,其中有参数。

然后，我们将项目推入`this.state`数组。

然后我们可以通过写来使用它:

```
buffer.append.apply(buffer, [1, 2, 3]);
```

# 使用 Rest 运算符创建变量函数

我们可以使用 rest 操作符来获取函数的所有参数。

例如，我们可以写:

```
function average(...args) {
  let sum = 0;
  for (const a of args) {
    sum += a;
  }
  return sum / args.length;
}
```

我们从 rest 操作符生成的`args`数组中获取参数数组。

然后我们可以循环遍历这些项目并得到总和。

然后我们用它除以`args`数组的长度。

这比使用`arguments`对象来获取参数要好。

# 不要使用参数来创建变量函数

rest 操作符替换`arguments`对象以获得可变数量的参数。

所以我们不应该写:

```
function average() {
  let sum = 0;
  for (const a of arguments) {
    sum += a;
  }
  return sum / args.length;
}
```

相反，我们应该使用 rest 操作符。

它也只能与传统的功能，而不是箭头功能，所以它的使用是有限的。

# 不要修改 arguments 对象

`arguments`对象看起来像数组，但它不是数组。

它没有任何数组方法。

然而，使用`call`方法，我们可以像这样调用数组方法:

```
const shift = [].shift;
shift.call(arguments);
```

但是如果我们有:

```
const obj = {
  add(x, y) {
    return x + y;
  }
};function call(obj, method) {
  const shift = [].shift;
  shift.call(arguments);
  shift.call(arguments);
  return obj[method].apply(obj, arguments);
}call(obj, 'add', 1, 2, 3)
```

这将导致错误“未捕获的类型错误:无法读取 undefined 的属性“apply”。

这意味着我们不能改变`arguements`对象来调用带有参数的对象。

# 使用绑定到库里函数

`bind`方法让我们返回一个内部带有值`this`的函数。

它还可以用于返回一个新函数，并应用现有函数的一些参数。

例如，如果我们有以下函数:

```
function add(x, y) {
  return x + y;
}
```

然后，我们可以使用`bind`创建一个函数，通过编写第一个参数:

```
const add2 = add.bind(null, 2);
```

第一个参数是`this`的值。

后面的参数是函数的参数。

然后我们可以通过写来使用它:

```
const sum  = add2(3);
```

而`sum`将是 5。

![](img/9952b2a8959c48e1648c5860482dc8a3.png)

Photo by [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`apply`和`bind`对于改变函数的`this`值和应用参数很有用。

`apply`调用该函数，`bind`返回一个新函数，并应用这些值。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**