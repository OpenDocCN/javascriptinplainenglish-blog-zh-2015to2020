# 普通 JavaScript 中提供的更多 Lodash 特性

> 原文：<https://javascript.plainenglish.io/more-lodash-features-that-are-available-in-plain-javascript-894c060fc26e?source=collection_archive---------1----------------------->

![](img/9eb2993bbbf1df0e2f300480558679da.png)

Photo by [Gary Bendig](https://unsplash.com/@kris_ricepees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

近年来，JavaScript 中的新特性一直在快速发展。之前由其他库填补的不足，已经成为普通 JavaScript 的内置特性。

在本文中，我们将看看现在普通 JavaScript 中可用的 Lodash 方法，比如函数 currying、部分应用的函数等等。

Lodash 的一些特性更好，但是对于其他特性，普通的 JavaScript 就足够了。

# 咖喱菜肴

Lodash 中的`curry`方法返回一个函数，该函数有一个或多个最初传入的参数。我们可以如下使用它:

```
const subtract = (a, b) => a - b;
const currySubtract = _.curry(subtract);
const subtract1 = currySubtract(1);
const diff = subtract1(5);
console.log(diff);
```

在上面的代码中，我们定义了 subtract 函数，它返回第一个参数减去第二个参数。

然后我们调用传递了 subtract 方法的`curry`方法来创建一个新方法，该方法生成一个参数并返回带有由该参数设置的第一个参数的`subtract`函数。那就是`currySubtract`功能。

然后我们调用`currySubtract`来设置`subtract`函数的参数，并返回带有第一个参数集的函数。最后，我们用第二个参数`subtract`调用`subtract1`函数得到最终结果。

我们可以通过编写以下代码对普通 JavaScript 做同样的事情:

```
const currySubtract = a => b => a - b;
const subtract1 = currySubtract(1);
const diff = subtract1(5);
console.log(diff);
```

它做完全相同的事情，但是不调用`curry`方法。

# 部分的

Lodash 也有一个部分应用函数的方法，它与 curry 不同，因为函数的一些参数被直接传递到函数中，并返回新的函数。

例如，我们可以这样写:

```
const add = (a, b) => a + b;
const add1 = _.partial(add, 1);
const sum = add1(2);
console.log(sum);
```

`partial`方法传入第一个参数，并返回传入第一个参数的函数。这给了我们`add1`函数。

然后 when 可以用第二个参数调用`add1`函数，在上面的代码中是 2，我们为`sum`得到 3。

在普通的 JavaScript 中，我们可以写:

```
const add = (a, b) => a + b;
const add1 = b => add(1, b);
const sum = add1(2);
console.log(sum);
```

同样，我们可以像对待`curry`方法调用一样跳过 Lodash `partial`方法调用。

# 情商

Lodash 有`eq`方法来比较值。例如，我们可以写:

```
const equal = _.eq(1, 1);
```

它的功能与`Object.is`相同，所以我们可以直接使用它。

![](img/3c3150f747d9395e3b614946bef0aa85.png)

Photo by [Dominik Lange](https://unsplash.com/@the_real_napster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 增加

它还有`add`方法，我们可以像在下面的代码中一样使用它:

```
const sum = _.add(1, 1);
```

我们看到值是 2。它和`+`操作符做同样的事情，所以我们可以用它来代替。

# 嵌套运算符

好的一面是，我们可以将这些方法直接传递给其他 Lodash 方法，如`map` 和`reduce`，如下所示:

```
const mult = _.map([1, 2, 3], n => _.multiply(n, 2));
```

我们从上面的代码中得到`[2, 4, 6]`，我们从:

```
const sum = _.reduce([1, 2, 3], _.add);
```

# 在

`at`方法让我们通过索引来访问对象的属性值或数组的条目。

例如，给定以下对象，我们可以编写以下内容:

```
const obj = { a: [{ b: { c: 2 } }, 1] };
```

我们可以通过写下以下内容来获得带有`at`的`c`属性的值:

```
const c = _.at(obj, ["a[0].b.c"]);
```

那么对于`c`我们得到 2。

此外，我们可以通过向上面的数组传递更多路径来访问一个对象的多个属性:

```
const vals = _.at(obj, ["a[0].b.c", "a[0].b"]);
```

然后我们 et:

```
2
{c: 2}
```

在 JavaScript 中，我们可以直接访问路径:

```
const vals = [obj.a[0].b.c, obj.a[0].b];
```

但是，对于可能不存在的访问路径来说，这很好。例如，给定同一个对象，如果我们写下:

```
const vals = _.at(obj, ["a[0].b.c", "d.e"]);
```

然后我们得到第二个条目的`undefined`,而不是崩溃应用程序。

正如我们所看到的，Lodash 在对象路径访问方面仍然有一些优势。然而，其他操作符，如 add、multiply、curry 和 partial，我们可以自己用普通的 JavaScript 轻松定义，所以 Lodash 仍然有一些价值。