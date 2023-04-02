# ES2019 的最佳功能—数组、字符串和符号

> 原文：<https://javascript.plainenglish.io/best-features-of-es2019-arrays-strings-and-symbols-448674bf7305?source=collection_archive---------16----------------------->

![](img/0aa924da2b4d47d07147bc21a84c7dd3.png)

Photo by [Aditya Wardhana](https://unsplash.com/@wardhanaaditya?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2019 的最佳特性。

# `Array.prototype.flat`

array 实例上的`flat`方法是 ES2019 中一个很有用的方法。

它让我们将数组展平一层或多层。

例如，我们可以写:

```
const arr = [1, 2, [3, 4],
  [
    [5, 6]
  ],
  [7, 8]
].flat(0)
```

那么数组没有变化。

如果我们有:

```
const arr = [1, 2, [3, 4],
  [
    [5, 6]
  ],
  [7, 8]
].flat(1)
```

然后我们得到:

```
[
  1,
  2,
  3,
  4,
  [
    5,
    6
  ],
  7,
  8
]
```

如果我们有:

```
const arr = [1, 2, [3, 4],
  [
    [5, 6]
  ],
  [7, 8]
].flat(2)
```

我们得到:

```
[
  1,
  2,
  3,
  4,
  5,
  6,
  7,
  8
]
```

1 是默认值。

# `Array.prototype.flatMap()`

`flatMap`将`map`和`flat`方法结合在一起。

例如，我们可以写:

```
const arr = [1, 2, 3].flatMap(x => x)
```

然后我们得到:

```
[
  1,
  2,
  3
]
```

如果我们有:

```
const arr = [1, 2, 3].flatMap(x => [x])
```

然后我们得到同样的东西。

如果我们有:

```
const arr = [1, 2, 3].flatMap(x => [[x]])
```

然后我们得到:

```
[
  [
    1
  ],
  [
    2
  ],
  [
    3
  ]
]
```

回调让我们将条目映射到我们想要的，并在每个条目上调用`flat`。

# `Object.fromEntries()`

`Object.fromEntries()`方法让我们从一个键值数组创建一个对象。

例如，我们可以写:

```
const obj = Object.fromEntries([
  ['a', 1],
  ['b', 2]
]);
```

那么`obj`就是`{a: 1, b: 2}`。

它执行与`Object.entries()`相反的动作:

```
const obj = {
  a: 1,
  b: 2,
};const arr = Object.entries(obj);
```

然后我们得到:

```
[
  [
    "a",
    1
  ],
  [
    "b",
    2
  ]
]
```

# `String.prototype.trimStart`

`trimStart` string 实例方法让我们修剪字符串开头的空白。

例如，我们可以写:

```
const str = '  foo  '.trimStart()
```

然后我们得到:

```
'foo  '
```

# `String.prototype.trimEnd`

`trimEnd`方法让我们修剪一个字符串的结尾。

例如，我们可以写:

```
'  foo'
```

`trimLeft`和`trimRight`分别相当于`trimStart`和`trimEnd`。

但这些都是遗留方法，因为我们希望与`padStart`和`padEnd`保持一致。

# 什么是空白？

以下字符是空格:

*   `<TAB>`(字符制表，U+0009)
*   `<VT>`(行制表，U+000B)
*   `<FF>`(表格进纸，U+000C)
*   `<SP>`(空格，U+0020)
*   `<NBSP>`(不间断空格，U+00A0)
*   `<ZWNBSP>`(零宽度 bo 分隔符，U+FEFF)
*   类别`Space_Separator` ( `Zs`)中具有属性`White_Space`的任何其他 Unicode 字符。

行结束符也是空白。它们包括:

*   `<LF>`(换行，U+000A)
*   `<CR>`(回车，U+000D)
*   `<LS>`(行分隔符，U+2028)
*   `<PS>`(段落分隔符，U+2029)

# `Symbol.prototype.description`

`Symbol.prototype.description`方法返回符号的描述。

例如，如果我们有:

```
const sym = Symbol('description');console.log(sym.description)
```

然后记录`'description’`。

![](img/b34b798d274aa4adb3f727764e6c109c.png)

Photo by [Victor Hernandez](https://unsplash.com/@victo_orh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

ES2019 引入了各种数组、字符串和符号方法。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**