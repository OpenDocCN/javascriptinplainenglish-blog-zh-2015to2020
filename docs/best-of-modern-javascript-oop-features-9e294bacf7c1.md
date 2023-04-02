# 现代 JavaScript 的最佳特性——OOP 特性

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-oop-features-9e294bacf7c1?source=collection_archive---------13----------------------->

![](img/8aaaaa86e8fdf062f61723b03faffa3e.png)

Photo by [Jelleke Vanooteghem](https://unsplash.com/@ilumire?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 中新的 OOP 特性。

# 方法定义

在 ES6 或更高版本中，可以用更短的方式定义方法。

而不是写:

```
var obj = {
  foo: function(x, y) {
    //..
  }
};
```

就像我们对 ES5 或更早版本所做的那样，我们可以写:

```
const obj = {
  foo(x, y) {
    //...
  }
};
```

它短得多，也干净得多。

我们可以对 getters 和 setters 使用相同的简写:

```
const obj = {
  get foo() {
    return 123;
  },

  set bar(value) {
    this.val = value;
  }
};
```

我们有由关键字`get`和`set`指示的`foo` getter 和`bar` setter。

对于生成器方法，我们可以写:

```
const obj = {
  * gen() {
    //...
  }
};
```

# 财产价值短缺

属性值也可以用更短的方式书写。

例如，我们可以写:

```
const x = 2;
const y = 1;
const obj = { x, y };
```

这是以下内容的简写:

```
const x = 2;
const y = 1;
const obj = { x: x, y: y };
```

我们也可以将简写和解构一起使用。

例如，我们可以写:

```
const obj = {
  x: 14,
  y: 1
};
const {
  x,
  y
} = obj;
```

那么`x`是 14 而`y`是 1。

# 计算属性键

我们可以向对象添加计算属性键。

它们用括号表示。

只要表达式返回一个字符串或符号，我们就可以在括号内使用它。

例如，我们可以写:

```
const obj = {
  foo: true,
  ['b' + 'ar']: false
};
```

然后我们在`obj`对象中有了`foo`和`bar`属性。

这种语法也适用于方法定义。

例如，我们可以写:

```
const obj = {
  ['h' + 'ello']() {
    return 'hello';
  }
};
```

创建`hello`方法。

我们可以将一个符号传入方括号中。

例如，要创建一个 iterable 对象，我们必须创建`Symbol.iterator`方法来创建对象。

我们可以写:

```
const obj = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
  }
};
```

然后我们可以通过写下:

```
for (const x of obj) {
  console.log(x);
}
```

# `Object`新方法

`Object`对象接收了几个新方法。

其中之一就是`Object.assign`法。

例如，我们可以写:

```
const obj = {
  foo: 123
};const newObj = Object.assign(obj, {
  bar: true
});
```

然后我们将`bar`属性放入`obj`并返回新的对象。

所以`obj`和`newObj`都是:

```
{ foo: 123, bar: true }
```

`Object.assign()`知道引导字符串和符号是属性键。

`Object.assign`只添加可枚举的自身属性。

其他种类的属性会被忽略。

这些值是通过一个简单的 get 操作读取的:

```
const value = source[prop];
```

写入值是通过一个简单的集合完成的:

```
target[prop] = value;
```

![](img/ac47a10c2b14a11476c39706aa23badd.png)

Photo by [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES6 带有许多新的面向对象编程特性。

它们包括方法速记和`Object.assign`。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码得到更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**