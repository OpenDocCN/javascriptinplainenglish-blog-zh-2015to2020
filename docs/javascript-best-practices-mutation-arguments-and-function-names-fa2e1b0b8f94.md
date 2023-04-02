# JavaScript 最佳实践—变异、参数和函数名

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-mutation-arguments-and-function-names-fa2e1b0b8f94?source=collection_archive---------3----------------------->

![](img/aee661f76a8938d6b93ba77a4a2a0062.png)

Photo by [Shannon Richards](https://unsplash.com/@shannonkay?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 不要使用`arguments`

`arguments`是一个特殊的变量，它让我们获得函数的参数。

我们不应该使用它，因为有更好的选择。

它也不适用于箭头函数。

例如，不写:

```
function sum() {
  const numbers = Array.prototype.slice.call(arguments);
  return numbers.reduce((a, b) => a + b);
}
```

我们写道:

```
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b);
}
```

rest 运算符将参数作为数组返回。

# 使用`delete`

`delete`从对象中删除属性。

它使物体变异，所以我们可能不想使用它。

例如，不写:

```
delete foo.bar;
```

我们写道:

```
const _ = require('lodash/fp');const fooWithoutBar = _.omit('bar', foo);
```

返回一个没有`bar`属性的新对象。

# Object.assign()，将变量作为第一个参数

如果我们用`Object.assign`将一个变量作为第一个参数传入，那么我们将对该变量进行变异。

这意味着最好不要用变量设置第一个参数。

相反，我们把一个物体。

例如，我们可以写:

```
const foo = Object.assign({}, a, b);
```

而不是:

```
const a = { foo: 1, bar: 2 };
const b = { bar: 3 };
Object.assign(a, b);
```

# 突变方法

很容易做偶然突变，所以不要过多使用突变方法。

比如`push`、`pop`、`unshift`等数组方法。变异数组，所以我们可能要考虑不做变异的替代方案。

我们可以用 spread 代替`push`。

例如，我们可以写:

```
const bar = [...arr, 1];
```

而不是`push`。

`pop`可替换为`slice`:

```
const baz = arr.slice(-1);
```

`unshift`可以替换为价差:

```
const bar = [1, ...arr];
```

`sort`更难替换，所以我们可能想继续使用它，但我们停止使用原始数组。

它返回排序后的数组。

# 变异算子

我们应该小心变异算子。

像`+=`这样的操作符做赋值来更新一个现有的变量。

这意味着我们可以很容易地意外修改它们。

我们还应该小心使用`-=`、`*=`、`/=`、`%=`、`++`或`--`。

`++`或`--`的位置也很重要，所以我们应该小心。

它既赋值又返回值。

`++`后一个变量返回原来的值。

`++`在变量返回新值之前到来。

这和`--`是一样的。

# 使用`Proxy`

代理会带来副作用，所以我们可能不想使用它们。

例如，不写:

```
const handler = {
  get(target, key) {
    return Math.max(target[key], 0);
  }
};
const object = new Proxy(variable, handler);
object.a;
```

我们创建一个函数来代替:

```
const negativeProp = (target, key) => {
  return Math.min(target[key], 0);
}
negativeProp(object, 'a');
```

# 函数标识符和它们的调用之间的间距

我们可以在函数标识符和它们的调用之间增加间距。

例如，我们应该写:

```
alert('Hello');
```

而不是:

```
alert ('Hello');
```

额外的空格不是很有用。

# 函数名应该与它们被赋予的变量或属性的名称相匹配

如果我们要把函数名赋给一个变量，那么函数名是没有用的。

但如果我们有，至少名字应该匹配。

例如，如果我们有:

```
const foo = function bar() {};
```

然后我们用`foo`调用函数。

相反，我们应该写:

```
const foo = function foo() {};
```

或者:

```
const foo = function() {};
```

属性也是如此。

例如，不写:

```
obj['foo'] = function bar() {};
```

或者:

```
obj['foo'] = function() {};
```

我们写道:

```
obj['foo'] = function foo() {};
```

或者:

```
obj['foo'] = function() {};
```

我们应该移除它们或者使它们匹配。

![](img/870aa4203dd9b7466d99f8151068ecd3.png)

Photo by [Adam Grabek](https://unsplash.com/@agmakonts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们把一个函数赋给一个变量或属性，名字就是多余的。

不应该使用`arguments`对象。

我们应该小心变异的方法。

此外，我们应该小心变异操作符。

函数调用的间隔是多余的。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**