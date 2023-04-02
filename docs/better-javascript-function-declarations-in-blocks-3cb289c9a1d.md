# 更好的 JavaScript——块中的函数声明

> 原文：<https://javascript.plainenglish.io/better-javascript-function-declarations-in-blocks-3cb289c9a1d?source=collection_archive---------12----------------------->

![](img/3696981609d010c77461ed41f9100c23.png)

Photo by [nicolas reymond](https://unsplash.com/@nicolasreymond?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 块函数声明的不可移植范围

我们不应该在一个块中有函数声明。

这不是有效的 JavaScript，但它仍然被大多数 JavaScript 引擎所允许。

这是因为我们对它产生的结果有疑问。

如果我们有:

```
function foo(x) {
  function f() {
    return "bar";
  }
  var result = [];
  if (x) {
    result.push(f());
  } result.push(f());
  return result;
}
console.log(foo(true));
console.log(foo(false));
```

然后我们得到:

```
["bar", "bar"]
["bar"]
```

就像我们期望的那样，因为我们将`f`的返回结果推送到`result`数组。

但是当我们有这样的事情:

```
function f() {
  return "global";
}function foo(x) {
  var result = [];
  if (x) {
    function f() {
      return "bar";
    }
    result.push(f());
  }
  result.push(f());
  return result;
}
console.log(foo(true));
console.log(foo(false));
```

那么第二个`foo`调用将不会运行，因为`f`被认为是内部调用。

这是一件会让很多人犯错的事情。

JavaScript 引擎仍然允许 code 函数，但是我们会得到这样奇怪的结果。

函数声明不应该在另一个块或函数中，所以我们应该避免这种情况。

我们应该做的是要么把内部函数放在外部，要么把它赋给一个局部变量。

例如，我们可以写:

```
function f() {
  return "global";
}function test(x) {
  var result = [];
  if (x) {
    const g = f;
    result.push(g());
  }
  result.push(f());
  return result;
}
```

那么范围就不会有任何问题。

# 不要用 eval 创建局部变量

`eval`由于其安全性和性能问题，绝对不应该使用。

另一个不好的地方是用它创建局部变量。

例如，如果我们有:

```
function foo(x) {
  eval("var y = x;");
  return y;
}console.log(foo("bar"));
```

然后我们看到`'bar'`被记录。

我们通过在传递给`eval`的字符串中给变量`x`赋值来创建变量`y`。

如果我们让`eval`调用有条件，问题就出现了。

例如，如果我们有:

```
var y = "global";function foo(x) {
  if (x) {
    eval("var y = 'local';");
  }
  return y;
}
console.log(foo(true)); 
console.log(foo(false));
```

然后根据我们在`foo`中的条件进行范围界定。

这肯定是一个问题，因为我们必须检查代码，而代码是一个字符串。

另一个不好的方面是我们有:

```
var y = "global";function bar(src) {
  eval(src);
  return y;
}
console.log(bar("var y = 'bar';")); 
console.log(bar("var z = 'bar';"));
```

一个字符串被传入`bar`函数，我们用它运行`eval`。

这用外部定义的额外变量污染了`bar`函数的范围。

这就更混乱了。

这些是我们不应该出于任何目的使用`eval`的更多原因。

![](img/03b50ca782c9cdece85b9f892edb72eb.png)

Photo by [Meriç Dağlı](https://unsplash.com/@meric?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该在另一个块中有函数声明。

这不是有效的 JavaScript，但仍然是允许的。

此外，我们不应该在任何情况下使用`eval`。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**