# 现代 JavaScript 的精华——迭代器和可迭代对象

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-iterators-and-iterables-488d4b5bb2b0?source=collection_archive---------13----------------------->

![](img/09849e5bdd9534b1196b98d026f1d260.png)

Photo by [Angela Loria](https://unsplash.com/@a_lo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 可迭代对象。

# 可迭代的迭代器

如果我们在`Symbol.iterator`方法中返回`this`，我们可以将`next`函数移到它自己的对象方法中。

例如，我们可以写:

```
function iterate(...args) {
  let index = 0;
  const iterable = {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      if (index < args.length) {
        return {
          value: args[index++]
        };
      } else {
        return {
          done: true
        };
      }
    },
  };
  return iterable;
}
```

在我们返回的`iterable`对象中有`next`。

`args`包含了我们想要迭代的条目。

当`index`小于`args.length`时，我们返回带有`value`的对象。

如果没有什么需要迭代的，我们返回一个设置为`true`的对象。

我们在`Symbo.iterator`方法中返回`this`，这样迭代就可以完成了。

for-of 循环只适用于 iterables，而不直接适用于迭代器。

Iterables 总是有`Symbol.iterator`方法，所以我们必须把迭代器放在 iterable 对象上，使它成为 iterable。

# `return()`和`throw()`

有两个迭代器方法是可选的。

如果迭代器提前结束，给我们一个机会清理迭代器。

`throw`让我们将方法调用转发给通过`yield*`迭代的生成器。

# 关闭迭代器

我们可以使用`return`来关闭一个迭代器。

例如，如果我们有之前的`iterate`函数。

然后我们可以调用`break`来干净利落地结束 for-of 循环:

```
for (const x of iterate('foo', 'bar', 'baz')) {
  console.log(x);
  break;
}
```

`return`必须返回一个对象。

这是因为生成器如何处理`return`语句。

一些构造函数关闭了没有完全清理干净的迭代器。

它们包括:

*   `for-of`
*   `yield*`
*   解构
*   `Array.from()`
*   `Map()`、`Set()`、`WeakMap()`、`WeakSet()`
*   `Promise.all()`，`Promise.race()`

# 组合子

组合子是组合现有的可重复项来创建新的可重复项的函数。

例如，我们可以通过编写以下内容来创建一个:

```
function combinator(n, iterable) {
  const iter = iterable[Symbol.iterator]();
  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      if (0 < n) {
        n--;
        return iter.next();
      } else {
        return {
          done: true
        };
      }
    }
  };
}
```

我们创建一个函数来返回 iterable 对象中的第一个`n`项。

然后我们可以通过写来使用它:

```
const arr = ['foo', 'bar', 'baz', 'qux'];
for (const x of combinator(3, arr)) {
  console.log(x);
}
```

这将记录`arr`数组的前 3 项。

# 无限迭代

Iterable 可以返回无限多的值。

例如，我们可以写:

```
function evenNums() {
  let n = 0;
  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      return {
        value: (++n) * 2
      };
    }
  }
}
```

创建返回偶数的 iterable 对象。

然后我们可以通过写下:

```
const nums = evenNums()
console.log(nums.next());
console.log(nums.next());
console.log(nums.next());
```

我们调用`evenNums`函数来创建迭代器。

然后我们在每个迭代器上调用`next`来生成数字。

![](img/ac23d054f927b7bd0eb011ba8b551f0e.png)

Photo by [Anthony Fomin](https://unsplash.com/@aginsbrook?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以创建返回有限和无限数量的值的可迭代对象。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**