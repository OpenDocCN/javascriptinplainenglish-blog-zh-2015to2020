# 使用流类型转换的 JavaScript 类型检查

> 原文：<https://javascript.plainenglish.io/javascript-type-checking-with-flow-type-casting-bbceb5d4221f?source=collection_archive---------1----------------------->

![](img/df18a4f61d8a87a6a9e328a78f5d2989.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Flow 是一个由脸书开发的类型检查器，用于检查 JavaScript 数据类型。它有许多内置的数据类型，我们可以用它们来注释变量和函数参数的类型。

在本文中，我们将了解如何将值从一种类型转换为另一种类型。

# 句法

要将值赋给给定的类型，我们可以使用以下语法:

```
(value: Type)
```

它可以用在变量、对象或数组中。在变量赋值中，它可以如下使用:

```
let foo = (value: Type);
```

对于对象，我们可以通过编写以下内容来转换属性值:

```
let obj = { prop: (value: Type) };
```

此外，我们可以通过编写以下内容来转换数组条目:

```
let arr = ([(value: Type), (value: Type)]: Array<Type>);
```

我们也可以将表达式转换成类型。例如，我们可以写:

```
(1 + 1: number);
```

来抛数字。

# 类型断言

我们只能在类型有意义的时候对它们进行造型。例如，如果我们有:

```
let x = 1;
```

然后我们可以写:

```
(x: 1);
```

或者:

```
(x: number);
```

因为`x`是具有值`1`的数字。转换为不是该值类型的类型将会失败。因此，将变量`x`转换为 s 字符串的方法是:

```
(x: string)
```

会给出一个错误，因为`x`是一个数字。

# 铅字铸造

我们可以将一个值转换成比变量的原始类型更广泛的类型。例如，如果我们有一个数字变量:

```
let x = 1;
```

然后，我们可以将`x`转换为`1`文字类型或`number`类型，如下所示:

```
(x: 1); 
(x: number);
```

当我们把一个变量赋给一个新的变量时，我们也可以把它转换成同样广泛的类型:

```
let y = (x: number);
```

但是将其转换为更窄的类型会失败，所以如果我们写:

```
(y: 1);
```

我们会得到一个错误，因为`y`应该是一个不确定值的`number`。

# 类型铸造通过任何

我们可以将任何东西投射到`any`，然后让我们将它投射到其他任何东西。

例如，如果我们想把一个数字转换成一个字符串，我们可以从上面的例子中看到我们不能直接这样做，因为我们不能把任何东西转换成一个不相关的类型。

然而，我们可以通过先将它造型为`any`来绕过它，然后我们可以将它造型为我们想要的任何类型。

所以我们可以写:

```
let x = 1;
(x: 1); 
(x: number);
let y = ((x: any): string);
(y: string);
```

不建议这样做，但是在某些情况下，当我们需要将一种类型的东西转换成不相关的类型时，这样做可能会很方便。

![](img/e76e2b0cd81990a81bdc5689349e63d3.png)

Photo by [William Bayreuther](https://unsplash.com/@wbayreuther?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 类型断言

当我们在函数中对对象进行操作时，我们可以通过强制转换对象的类型来设置对象的类型。例如，如果我们有一个克隆对象的函数，我们可以写:

```
function foo(obj) {
  const cloneObj = {...(obj: { [key: string]: mixed })};
  return cloneObj;
}
```

然后当我们调用它时，Flow 足够智能地确定属性的类型:

```
let cloneObj = foo({
  a: 1,
  b: true,
  c: 'three'
})
(cloneObj.a: 1);
(cloneObj.b: true);
(cloneObj.c: 'three');
```

这比将类型放在参数中更有用:

```
function foo(obj: { [key: string]: mixed }) {
  const cloneObj = {...obj};
  return cloneObj;
}
```

既然流不会这样知道`cloneObj`的结构。

我们也可以使用带有`$Shape`关键字的泛型来断言`obj`参数的类型，如下所示:

```
function foo<T: { [key: string]: mixed }>(obj: T): $Shape<T> {
  const cloneObj = {...obj};
  return cloneObj;  
}
```

代码`<T: { [key: string]: mixed }>`将让 Flow 知道`obj`参数接受一个对象。

使用 Flow，我们可以通过使用`:`和其后的类型标识符，将类型转换为给定对象值的相关类型。

此外，我们可以通过先将某个东西转换为`any`来将其转换为不相关的类型。

最后，为了让流程知道变量或参数是一个动态对象，我们可以使用`<T: { [key: string]: mixed }>`和`(obj: T)`签名来确保参数是一个动态对象。

然后，我们将`$Shape<T>`设置为返回类型，以强制该函数返回的对象类型是一个动态对象。