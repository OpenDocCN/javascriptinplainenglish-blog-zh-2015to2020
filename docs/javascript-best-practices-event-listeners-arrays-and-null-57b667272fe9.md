# JavaScript 最佳实践—事件侦听器、数组和 null

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-event-listeners-arrays-and-null-57b667272fe9?source=collection_archive---------0----------------------->

![](img/df565bb3e24aba923a2f16bd75d2879d.png)

Photo by [Alireza Attari](https://unsplash.com/@alireza_attari?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 不要使用`null`文字

使用`undefined`做任何事情都比使用`null`和`undefined`要好。

所以我们在任何事情上都坚持`undefined`。

例如，不写:

```
let foo = null;
```

或者:

```
if (bar == null) {}
```

我们写道:

```
let foo;
```

或者:

```
if (foo === null) {}
```

# `No process.exit()`

我们不想使用`process.exit`来退出代码。

它突然存在于程序中，没有给它清理的机会。

而不是写:

```
process.exit(0);
```

我们写道:

```
process.on('SIGINT', () => {
  console.log('Got SIGINT');
  process.exit(1);
});
```

# `Use Array#reduce()`和`Array#reduceRight()`

`reduce`和`reduceRight`使得代码难以阅读。

它们可以替换为`map`、`filter`或 for-of 循环。

例如，不写:

```
array.reduce(reducer, initialValue);
```

我们写道:

```
let result = initialValue;for (const element of array) {
  result += element;
}
```

# 没有不可读的数组析构

如果我们写一个数组析构代码，那么我们应该使它们可读。

例如，不写:

```
const [, , foo] = parts;
```

我们写道:

```
const [foo] = parts;
```

# 没有未使用的对象属性

如果我们有不使用的对象属性，那么我们可能想要删除它们。

例如，不写:

```
const obj = {
  used: 1,
  unused: 2
};console.log(obj.used);
```

我们写道:

```
const obj = {
  used: 1
};console.log(obj.used);
```

# 没有无用的`undefined`

我们不应该在没有用的地方使用`undefined`。

例如，不写:

```
let foo = undefined;const {
  foo = undefined
} = bar;const baz = () => undefined;function foo() {
  return undefined;
}function* foo() {
  yield undefined;
}function foo(bar = undefined) {}function foo({
  bar = undefined
}) {}foo(undefined);
```

我们写道:

```
let foo;const {
  foo
} = bar;const baz = () => {};function foo() {

}function* foo() {

}function foo(bar) {}function foo({
  bar
}) {}foo();
```

我们不需要返回`undefined`的函数。

并且应该返回或产生`undefined`，因为它们是自动完成的。

# 没有分数为零或悬挂点的数字文字

我们不想添加悬点或零分数，因为它们是多余的。

例如，不写:

```
const foo = 1.0;
const foo = -1.0;
const foo = 1.;
```

我们写道:

```
const foo = 1;
const foo = -1;
const foo = 1;
```

# 强制数值大小写正确

我们应该对数值有适当的大小写。

例如，不写:

```
const foo = 0XFF;
const foo = 0B10;
const foo = 0O76;
```

我们写道:

```
const foo = 0xFF;
const foo = 0b10;
const foo = 0o76;
```

# 在`on`功能上使用`.addEventListener()`和`.removeEventListener()`

我们可以使用`addEventListener`和`removeListener`来代替使用`on`函数来添加事件监听器。

使用`addEventListener`，我们可以添加和删除监听器。

我们也可以指定捕获模式而不是冒泡。

此外，我们可以注册无限数量的处理程序。

它还将逻辑从文档结构中分离出来。

并且消除了与正确事件名称的混淆。

属性不能正确响应错误。

我们可以给它赋值，但没有错误，这不好。

因此，与其写:

```
foo.onclick = () => {};
```

我们应该写:

```
foo.addEventListener('click', onClick);
```

并且:

```
foo.removeEventListener('click', onClick);
```

# 在 DOM 元素上使用`.dataset`。setAttribute

我们可以使用`dataset`属性来代替使用`setAttribute`属性来设置定制属性。

用它更难出错。

例如，不写:

```
element.setAttribute('data-foo', 'bar');
```

我们写道:

```
element.dataset.foo = 'bar';
```

# `Use KeyboardEvent#key`结束`KeyboardEvent#keyCode`

不推荐使用`keyCode`属性。所以我们应该使用`key`属性来检查键码。

例如，不写:

```
window.addEventListener('keydown', event => {
  console.log(event.keyCode);
});
```

我们写道:

```
window.addEventListener('keydown', event => {
  console.log(event.key);
});
```

![](img/991c8973e000d839809d71b33d65b62b.png)

Photo by [Alireza Attari](https://unsplash.com/@alireza_attari?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该用`undefined`而不是`null`。

同样，我们应该使用`addEventListener`和`removeEventListener`而不是使用`on`函数。

循环和其他数组方法比`reduce`要好，因为代码可读性更好。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**