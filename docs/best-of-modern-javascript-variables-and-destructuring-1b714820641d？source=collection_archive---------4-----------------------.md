# 现代 JavaScript 精华——变量和析构

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-variables-and-destructuring-1b714820641d?source=collection_archive---------4----------------------->

![](img/ec513ee9fed441ef3a9eab5635fc4ea8.png)

Photo by [Ramiz Dedaković](https://unsplash.com/@ramche?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 变量和析构。

# 全局对象

全局对象在 Node.js 中，浏览器端 JavaScript 更多的是一个 bug 而不是一个特性。

它的性能很差。

全局对象是全局变量。

在全局作用域中，`var`声明声明全局对象属性。

函数声明也声明全局对象属性。

当我们在全局作用域中创建属性时，`let`、`const`和类声明不会给全局对象添加属性。

# 函数声明和类声明

像`let`一样，函数声明是块范围。

它们像`var`一样在全局范围内创建全局对象的属性。

它们也是托管的，所以就可用性而言，它们独立于它们在代码中的位置。

例如，如果我们有:

```
{ 
  console.log(foo()); 
  function foo() {
    return 'hello';
  }
}
```

然后我们可以调用`foo`而不出错，因为`foo`被提升了。

类声明是块范围的，不会在全局对象上创建属性，也不会被提升。

他们在引擎盖下创造功能，但他们仍然没有被提升。

例如，如果我们有:

```
{
  const identity = x => x;
  const inst = new Foo();
  class Foo extends identity(Object) {}
}
```

然后我们会得到一个`ReferenceError`，因为`Foo`在被调用之前没有被定义。

# `const`对`let`对`var`

声明变量的最好方法是先用`const`。

它们不能通过重新分配来改变，所以用它们更难出错。

然而，我们仍然可以改变分配给它的内容。

例如，我们可以写:

```
const foo = {};
foo.prop = 123;
```

我们可以在循环中使用`const`:

```
for (const x of ['a', 'b']) {
   console.log(x);
 }
```

因为`x`是为循环的每次迭代创建的，所以我们可以在循环中使用`const`。

否则，我们可以在以后需要更改变量时使用`let`。

例如，我们可以写:

```
let counter = 0;
counter++; let obj = {}; 
obj = { foo: 'bar' };
```

我们不应该使用`var`，它们应该只出现在遗留代码中。

# 解构

析构是从数组和对象中提取多个值的一种便捷方式。

数组和对象可以嵌套。

# 对象析构

我们可以通过书写来破坏对象:

```
const obj = {
  firstName: 'james',
  lastName: 'smith'
};
const {
  firstName: f,
  lastName: l
} = obj;
```

我们创建了一个对象`obj`，然后将它赋给第二条语句的表达式。

这将获得`firstName`属性并将其分配给`f`。

它将对`lastName`做同样的事情，并将其分配给`l`。

析构也有助于处理返回值。

例如，我们可以写:

```
const obj = {
  foo: 'bar'
};const {
  writable,
  configurable
} =
Object.getOwnPropertyDescriptor(obj, 'foo');console.log(writable, configurable);
```

我们称之为`Object.getOwnPropertyDescriptor`方法，它接受一个对象和一个属性键。

它返回一个具有`writable`和`configurable`属性的对象。

所以我们可以使用析构来获取属性并将它们赋给变量。

![](img/11091217bb8ef2ac9129dc8b5bb9e7f7.png)

Photo by [Alain Pham](https://unsplash.com/@alain_pham?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以遵循一些声明变量的最佳实践。

此外，析构对于从对象和数组中获取变量也很方便。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**