# JavaScript 最佳实践—间距、行和回调错误

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-spacing-lines-and-call-be6ab9f63eeb?source=collection_archive---------16----------------------->

![](img/6123fa23d0c103304fd805b17b92dc86.png)

Photo by [Dan Wayman](https://unsplash.com/@theamazingdjw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 命名 F `unction`表达式

函数表达式中的名字不是很有用。

如果我们将函数设置为一个变量，我们就不能用那个名字来使用它。

我们在生活中也不太需要它。

所以我们可以写:

```
Foo.prototype.bar = function() {};
```

而不是:

```
Foo.prototype.bar = function bar() {};
```

此外，我们可以写:

```
(function() {
    // ...
}())
```

而不是:

```
(function bar() {
    // ...
}())
```

除非我们需要函数内部的名字，否则它没那么有用。

# F `unction`声明或表达式的一致使用

为了保持一致性，我们可以保持一种函数风格。

所以要么我们坚持:

```
function doWork() {
  // ...
}
```

或者:

```
const doWork = function() {
  // ...
};
```

箭头函数只能使用第二种样式。

# 函数调用的参数之间换行

我们只需要参数换行符，函数调用只需要长表达式或参数列表。

例如，我们可以写:

```
foo(
  "foo",
  "bar",
  "three"
);
```

或者:

```
foo("one")
```

我们只是将行长度保持在 100 个字符左右或更少。

# 函数括号内的一致换行符

我们应该在函数括号内有一致的换行符。

例如，我们可以写:

```
function foo(
  bar,
  baz
) {}
```

或者:

```
const foo = function(
  bar, baz
) {};
```

对于短函数，我们可以将签名放在一行中。

除此之外，我们把它分成多行。

# 生成器函数中*周围的间距

我们应该在发生器函数中保持`*`周围的间距。

例如，我们可以写:

```
function* generator() {
  yield "foo";
  yield "bar";
}
```

或者:

```
function * generator() {
  yield "foo";
  yield "bar";
}
```

或者:

```
function *generator() {
  yield "foo";
  yield "bar";
}
```

我们只需要坚持一个。

# Return 语句应该出现在属性 Getters 中

getters 的全部目的是返回一些东西。

因此，我们应该确保这是在 getters 中完成的。

例如，我们应该写:

```
const obj = {
  get name() {
    return "james";
  }
};
```

或者:

```
Object.defineProperty(obj, "age", {
   get() {
     return 100;
   }
 });
```

而不是:

```
const obj = {
  get name() {
    return;
  }
};
```

或者:

```
Object.defineProperty(obj, "age", {
   get() {

   }
 });
```

或者:

```
class P {
  get name() {

  }
}
```

# 将 require()放在顶级模块范围内

我们应该把我们的`require`调用放在模块作用域的顶层。

例如，我们应该写:

```
const fs = require("fs");
```

而不是:

```
function foo() {
  if (condition) {
    const fs = require("fs");
  }
}
```

动态需求令人困惑，

所以我们不应该这么做。

# 在对象文字和类中需要成组的访问器对

拥有没有 getters 的 setters 并没有多大用处，因为我们无法获得被设置的值。

因此，我们应该为任何 setters 设置 getters。

例如，我们应该写:

```
const obj = {
  get a() {
    return this.val;
  },
  set a(value) {
    this.val = value;
  },
  b: 1
};
```

而不是:

```
const obj = {
  set a(value) {
    this.val = value;
  },
  b: 1
};
```

我们可以用 getter 得到`obj`中`a`的值。

# 保护 for-in

如果用 for-in 循环，我们应该检查继承的属性，因为它枚举继承的属性。

而不是写:

```
for (key in foo) {
  doWork(key);
}
```

我们写道:

```
for (key in foo) {
  if (Object.prototype.hasOwnProperty.call(foo, key)) {
    doWork(key);
  }
}
```

或者:

```
for (key in foo) {
  if ({}.hasOwnProperty.call(foo, key)) {
    doSomething(key);
  }
}
```

我们使用`Object.prototype.hasOwnProperty.call`或`{}.hasOwnProperty.call`代替`foo.hasOwnProperty`，这样即使`hasOwnProperty`被覆盖，它仍然可以工作。

`hasOwnProperty`检查属性是否为非继承属性。

# 回调错误处理

如果回调中存在错误，我们应该检查它们。

例如，我们可以写:

```
function foo(err, data) {
  if (err) {
    console.log(err.stack);
  }
  doWork();
}
```

如果这是一个节点样式的回调，`err`将被错误填充，我们应该在它被抛出的情况下处理它。

![](img/da2d6919e5b1a0a6b29aaa5ab12ebe5a.png)

Photo by [mandy zhu](https://unsplash.com/@mandymonday?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该在回调中处理错误。

命名函数表达式是多余的。

换行符和间距应该一致。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**