# JavaScript 最佳实践——减少麻烦

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-reducing-headaches-98269bf83e53?source=collection_archive---------4----------------------->

![](img/cf8ce7bb681229179593836051f8948b.png)

Photo by [asoggetti](https://unsplash.com/@asoggetti?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 使用===测试相等性

`===`是检验等式的最佳算子。

检查类型和值以测试是否相等。

`==`在进行比较之前，根据自己的规则转换值。

例如，我们可以写:

```
1 === '1'
```

返回`false`。

但是:

```
1 == '1'
```

由于自动数据类型转换，返回`true`。

同样，我们可以使用`!==`来检查不平等。

例如，我们可以写:

```
1 !== '1'
```

返回`true`。

并且:

```
1 != '1'
```

基于同样的原因返回`false`。

用`!==`和`===`更容易。

# 包括所有分号

我们应该将分号放在语句的末尾，以避免 JavaScript 解释器自动为我们插入分号。

这减少了出错的机会，因为我们处于控制之中。

# 使用 ESLint

ESLint 是 JavaScript 最流行的 linter。

我们应该使用它来 lint 我们的代码，以捕捉错误并找到我们代码的格式问题。

它会发现错误的代码，这样我们就可以修复它们。

此外，它还附带了许多 TypeScript 插件和各种风格指南。

# 使用名称空间

我们应该命名我们的变量以避免名称冲突。

而不是写:

```
let price = 5;
```

我们写道:

```
let namespace = {};
namespace.price = 5;
```

这样，我们通过将值放入名称空间来分离变量。

# 避免使用 Eval

`eval`肯定不好，因为它从字符串中运行代码。

这是一个安全问题，会阻止任何类型的优化。

调试也非常困难，因为代码是在一个字符串中。

# 谨慎使用小数

我们应该小心十进制运算。

例如，如果我们有:

```
0.1 + 0.2
```

它不会像我们预期的那样返回 0.3。

相反，我们得到:

```
0.30000000000000004
```

这是因为浮点数是用二进制数表示的。

两种数字系统之间的转换意味着浮点运算不会精确。

因此，我们应该将它们四舍五入到我们想要返回的小数位数。

# 在同一行开始块

块应该与关键字在同一行开始。

例如，不要写:

```
if(environment === 'testing') 
{
   console.log('testing');
} 
else 
{
   console.log('production');
}
```

我们应该写:

```
if (environment === 'testing') {
  console.log('testing');
} else {
  console.log('production');
}
```

在某些情况下，我们可以避免错误，避免使用以下语句:

```
return
{
   age: 15,
   name: 'james'
}
```

该对象不会被返回，因为它被认为是独立于`return`语句的。

相反，我们写道:

```
return {
  age: 15,
  name: 'james'
}
```

所以我们返回这个对象。

# 使用显式块

我们应该用花括号明确地划分块。

这样，我们就不会对一个块的起点和终点有任何混淆。

例如，不要写:

```
if (i > 3)
  doWork();
```

我们写道:

```
if (i > 3) {
  doWork();
}
```

现在我们不会对积木有任何混淆。

![](img/91661528001c2af7aa192dd863ad27e9.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以遵循一些基本的最佳实践来减少头痛。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**