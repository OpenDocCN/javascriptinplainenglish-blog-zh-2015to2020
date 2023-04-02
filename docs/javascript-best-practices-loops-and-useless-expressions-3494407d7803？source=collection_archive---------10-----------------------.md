# JavaScript 最佳实践—循环和无用的表达式

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-loops-and-useless-expressions-3494407d7803?source=collection_archive---------10----------------------->

![](img/c362b8aae270527bbabb014be4842946.png)

Photo by [Grant Durr](https://unsplash.com/@blizzard88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在这篇文章中，我们将看看我们不应该有的循环，无用的标签，`catch`子句，以及无用的`call`和`apply`调用。

# 没有未修改的循环条件

如果我们有一个未修改条件的`while`或`do...while`循环，该循环将永远运行，因为循环条件没有用新值更新。

这通常不是我们想要的，因为它会因为无限循环而导致程序崩溃。

然而，无限循环有时适用于像生成器函数这样的函数，当从函数创建并调用生成器时，这些函数会返回最新的值。

因此，在常规的`while`循环中，我们可能不应该编写无限循环:

```
while (condition) {
    doSomething();
}
```

但是，它适用于返回无限个条目的生成器函数:

```
function* num() {
  let i = 1;
  while (true) {
    yield i;
    i++;
  }
}const iterator = num();
const val = iterator.next().value;
const val2 = iterator.next().value;
```

在上面的代码中，我们有一个`num`生成器函数，在我们通过调用它创建了一个`iterator`之后，它返回一个新的整数，然后对它调用`next`一次一次地获取一个值。

对于常规的`while`或`do...while`循环，我们应该总是有一个变化的条件。

例如，我们可以编写以下内容:

```
let x = 1;
while (x <= 5) {
  console.log(x);
  x++;
}
```

上面的代码有一个条件，当我们将`x`加 1 直到`x`为 5 时，该条件结束循环。

# 没有未使用的表达式

我们代码中的一些 JavaScript 表达式是无用的，因为它不做任何事情。例如，如果我们有以下代码:

```
x + 1;
```

然后它什么也不做，因为我们没有把它赋给变量，也没有在函数中返回它。

有了构建工具，像这样的表达式可能会被消除并破坏应用程序逻辑。

此外，像`x = 1, y = 2`这样的序列赋值表达式总是没有用，除非我们返回值，将它们赋给其他地方的另一个变量，或者稍后对它进行一些操作。

# 没有未使用的标签

未使用的标签是无用的，因为它们在任何地方都不会被使用。因此，这是死代码，只是占用空间。

例如，如果我们有以下内容:

```
loop:
  for (let i = 1; i <= 5; i++) {
    console.log(i);
  }
```

要么我们应该使用它，要么我们应该删除它。我们可以如下使用它，例如:

```
loop:
  for (let i = 1; i <= 5; i++) {
    console.log(i);
    if (i === 2) {
      break loop;
    }
  }
```

当`i`为 2 时，我们用`break loop`标记`loop`的循环。

# 没有不必要的`.call()`和`.apply()`

`call`和`apply`方法只有在我们需要改变函数中`this`的值，并用各种参数调用它的时候才有用。

它的以下用法不是很有用:

```
function greet(greeting) {
  console.log(`${greeting} ${this.name}`);
}greet.call(undefined, 'hi');
```

我们传入`undefined`使`this`设置为`undefined`。所以和直接调用`greet('hi')`是一样的。

因此，我们不需要调用`call`方法。

同样，下面的`apply`调用也是无用的:

```
function greet(greeting) {
  console.log(`${greeting} ${this.name}`);
}greet.apply(undefined, ['hi']);
```

也是直接调用`greet('hi')`。

相反，我们应该以有用的方式用`call`或`apply`调用`greet`。

例如，我们应该这样称呼它们:

```
function greet(greeting) {
  console.log(`${greeting} ${this.name}`);
}greet.apply({
  name: 'jane'
}, ['hi']);greet.call({
  name: 'jane'
}, 'hi');
```

这样，我们将函数内部的`this`的值设置为一个具有`name`属性的对象，这样函数内部的`this.name`就可以用`this.name`填充。

我们实际上需要使用`call`和`apply`来设置上面例子中`this`的值。

![](img/4049511a3b3c62a00950dd94d386efa0.png)

Photo by [Icons8 Team](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

未修改的循环条件在大多数情况下是不好的，因为它可能会因为无限循环而导致崩溃。

然而，无限循环在用返回的迭代器不断返回值的生成器函数中很有用。

未使用的循环标签是无用的，因此应该删除死代码。

`call`和`apply`是没有用的，就像那些把`this`的值设置为`undefined`的，因为它们和直接调用函数没什么区别。

最后，我们也不应该在代码中有未使用的表达式，因为这只是我们可以删除的更多无用的代码。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)找到所有这些信息——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**