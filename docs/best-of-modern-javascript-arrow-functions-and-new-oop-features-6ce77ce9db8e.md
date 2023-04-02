# 现代 JavaScript 的精华—箭头函数和新的 OOP 特性

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-arrow-functions-and-new-oop-features-6ce77ce9db8e?source=collection_archive---------11----------------------->

![](img/deaea591378a7fe2eb9b6c8f0a5763cc.png)

Photo by [Sean](https://unsplash.com/@seaninuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中的箭头函数和新的 OOP 特性。

# 用箭头函数返回对象文字

如果我们想用一个单一的声明箭头函数返回对象文字，我们必须用括号将返回的对象括起来。

这样就不会和积木混淆了。

例如，我们可以写:

```
const foo = x => ({ bar: 'baz' });
```

然后当我们调用`foo()`时，我们得到返回的`{ bar: 'baz' }`。

# 立即调用的箭头函数

我们可以定义立即调用的箭头函数。

例如，我们可以写:

```
(() => {
  return 'foo'
})();
```

定义一个箭头函数并调用它。

我们仍然使用分号来确定立即调用的箭头函数。

我们必须用括号将箭头函数括起来。

# 带表达式体的圆括号箭头函数

我们用一个表达式包装我们的 arrow 函数体:

```
const value = () => (foo());
```

这和写作是一样的:

```
const value = () => foo();
```

# 箭头函数和绑定

我们可以使用箭头函数来代替`bind`，这样我们就不必设置自己的`this`。

如果我们只想从外部引用`this`，这很有用。

例如，不要写:

```
obj.on('click', this.handleEvent.bind(this));
```

我们可以写:

```
obj.on('anEvent', event => this.handleEvent(event));
```

然后我们从外部取`this`的值。

我们也可以使用箭头函数作为回调函数。

例如，我们可以写:

```
const set1 = new Set([1, 2, 3]);
const set2 = new Set([3, 9, 2]);
const intersection = [...set1].filter(a => set2.has(a));
```

我们通过将`set1`展开到一个数组中，然后调用`filter`返回也在`set2`中的项目，来计算交集。

那么`intersection`就是`[2, 3]`。

# 部分评估

`bind`让我们对一个函数进行部分求值并返回它。

使用箭头函数，我们可以写:

```
const add1 = y => add(1, y);
```

而不是写:

```
function add(x, y) {
  return x + y;
}
const add1 = add.bind(undefined, 1);
```

部分应用两个数相加的功能。

# 箭头函数与普通函数

箭头函数和普通函数有很大的区别。

箭头功能没有自己的值`arguments`、`super`、`this`或`new.target`。

它们也不能用作构造函数。

它没有隐藏的`[[Construct]]`属性和`prototype`属性。

所以我们不能使用`new`操作符。

# 新的 OOP 特性

现代 JavaScript 有许多新的 OOP 特性。

ES6 引入了方法定义简写:

```
const obj = {
  foo(x, y) {
    //...
  }
};
```

我们还可以添加属性值简写。

例如，我们可以写:

```
const firstName = 'mary';
const lastName = 'smith';const obj = {
  firstName,
  lastName
};
```

代码

```
const obj = {
  firstName,
  lastName
};
```

是以下内容的简写:

```
const obj = {
  firstName: firstName,
  lastName: lastName
};
```

从现在开始我们可以用速记了。

此外，我们有计算属性键语法。

例如，我们可以写:

```
const prop = 'bar';
const obj = {
  [prop]: true,
  ['b' + 'ar']: 123
};
```

我们将键值传入方括号来添加它们。

![](img/5ce18ad7fe75d7f8c96206ebbf11c527.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

箭头函数在各种上下文中都很有用。

现代 JavaScript 还有一些有用的新特性，让我们的生活变得更加轻松。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**