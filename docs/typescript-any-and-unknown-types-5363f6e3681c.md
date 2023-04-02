# TypeScript 任何未知类型

> 原文：<https://javascript.plainenglish.io/typescript-any-and-unknown-types-5363f6e3681c?source=collection_archive---------3----------------------->

![](img/284a6c0d5ee90ffadf0bb40e27898931.png)

Photo by [Aniket Bhattacharya](https://unsplash.com/@aniket940518?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 TypeScript 中，有两种数据类型可以保存任何内容。

他们是`any`和`unknown`类型。

因为他们有不同的名字，他们是不同的。

在本文中，我们将看看它们之间的区别以及我们可以用它们做什么。

# 任何类型

`any`类型变量允许我们给它赋值。

如果它被用作参数，那么我们可以传入任何东西。

例如，我们可以写:

```
function func(value: any) {
  const foo = 5 * value;
  const bar = value[1];
}
```

TypeScript 编译器没有限制我们可以对具有`any`类型的变量或参数做什么。

如果我们有一个变量，我们可以给它赋值。

例如，我们可以写:

```
let bar: any;bar = null;
bar = true;
bar = {};
```

我们可以给一个`any`类型的变量赋值。

此外，我们可以将一个`any`变量赋给任何类型的变量:

```
function func(baz: any) {
  const a: null = baz;
  const b: boolean = baz;
  const c: object = baz;
}
```

一个真实的例子是`JSON.parse`。它在 TypeScript 的类型定义中的签名是:

```
JSON.parse(text: string): any;
```

`unknown`类型在 TypeScript 中尚不存在，它被添加到类型定义中，因此`any`类型被用作返回类型。

对于输入没有已知结构的东西来说，`unknown`类型是`any`的更好的替代。

# `The unknown Type`

`unknown`型是`any`的更安全版本。

这是因为`any`让我们做任何事情，但是`unknown`有更多的限制。

在我们对一个`unknown`类型的值做任何事情之前，我们必须首先通过使用类型断言、等式、类型保护或断言函数来使类型为人所知。

要添加类型断言，我们可以使用`as`关键字。

例如，我们可以写:

```
function func(value: unknown) {  
  return (value as number).toFixed(2);
}
```

因为我们用`as`将`value`参数转换为一个数字，所以我们可以用它调用`toFixed`方法来舍入这个数字。

TypeScript 编译器还可以通过相等比较来确定数据类型。

例如，我们可以写:

```
function func(value: unknown) {
  if (value === 123)
    const rounded =  value.toFixed(2);
  }
}
```

我们检查一下`value`是不是 123。

这样，如果是，那么 TypeScript 编译器知道它是一个数字。

所以我们可以在上面调用`toFixed`。

我们可以用类型守卫做同样的事情。

为了使用它们，我们使用`typeof`操作符来检查类型。

所以我们可以写:

```
function func(value: unknown) {
  if (typeof value === 'number')
    const rounded =  value.toFixed(2);
  }
}
```

而且编译器也会知道`value`是一个数字。

我们也可以使用断言函数来做同样的事情。

例如，我们可以写:

```
function func(value: unknown) {
  assertNum(value);
  const rounded = value.toFixed(2);    
}function assertNum(arg: unknown): asserts arg is number {
  if (typeof arg !== 'number') {
    throw new TypeError('not a number');
  }
}
```

我们创建了一个`assertNum`函数来检查`arg`是否是一个数字。

如果不是，则会引发异常。

然后，在进行任何操作之前，我们在`func`函数中调用它。

这样，编译器也知道`value`是一个数字。

![](img/40ba64d69f1078a7a4c48dcb5fc6f9a1.png)

Photo by [Ian Kuik](https://unsplash.com/@imiankuik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`any`类型在大多数情况下都太灵活了。

`unknown` type 可以让我们存储任何东西，但是我们必须在做任何事情之前确定它的类型。

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容吧！**