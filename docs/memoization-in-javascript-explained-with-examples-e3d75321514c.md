# 用例子解释 JavaScript 中的记忆化

> 原文：<https://javascript.plainenglish.io/memoization-in-javascript-explained-with-examples-e3d75321514c?source=collection_archive---------6----------------------->

## 了解 JavaScript 中记忆化的概念

![](img/72ef4d061874f00ccfa06e385eee5fe1.png)

Photo by [Tudor Baciu](https://unsplash.com/@baciutudor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

就性能而言，JavaScript 中的函数有时会非常昂贵。因此，你的代码可能会非常慢，这会降低你的网站或应用程序的体验。记忆化是一种常用的技术，可以用来显著提高代码速度。它使用缓存来存储结果，以便后续调用耗时的函数时不会在另一时间执行相同的工作。

在本文中，我们将通过例子介绍 JavaScript 中记忆化的概念。让我们开始吧。

# 什么是记忆化？

记忆是建立一个函数的过程，这个函数能够记住它以前计算的结果或值。这是一种优化技术，我们缓存函数结果。

我们使用一个*记忆*函数来计算这个函数，如果它在最后的计算中已经用相同的参数执行过。这样可以节省时间，但是也有不好的一面，那就是我们会消耗更多的内存来保存之前的结果。

# 实现记忆功能

JavaScript 中的内存化概念用于缓存函数结果，以加速执行缓慢或耗时的函数调用。

如果结果没有被缓存，我们将执行函数并缓存结果。让我们举一个求一个数的平方的例子。

```
const square = () => {
    let cache = {}; // set cache return (**value**) => {
        // if exists in cache return from cache
        if (value **in** cache) {
            console.log("Fetching from cache");
            return **cache[value]**;
        } else {
            // If not in cache perform operation
            console.log("Performing expensive query");
            const result = value * value;
            cache[value] = result; // store the value in cache
            return **result**; // return result
        }
    }
}

const sq = square();
console.log(sq(21)); // Performing expensive query, 441
console.log(sq(21)); // Fetching from cache, 441
```

如上所述，我们创建了一个函数`square`，然后添加了一个变量`cache`，并将其设置为一个空对象，以便在其中缓存结果。

在函数方块中，我们有一个内部函数来检查缓存中的结果。如果它在缓存中可用，它将返回它。如果不可用，它将把它添加到缓存中，然后返回它。

如果您不习惯 ES6 语法，这里还有一个例子:

```
function memoize(fn) {
  const cache = {};return function (param) {
    // if exists in cache return from cache
    if (cache[param]) {
      console.log('cached');
      return cache[param]; // If not in cache perform operation
    } else {
      let result = fn(param);
      cache[param] = result;
      console.log(`not cached`);
      return result;
    }
  }
}

const toUpper = (str ="")=> str.toUpperCase();

const toUpperMemoized = memoize(toUpper);

console.log(toUpperMemoized("abcdef")); //not cached ABCDEF
toUpperMemoized("abcdef"); // cached ABCDEF
```

在上面的例子中，我们有一个缓存变量来存储结果。该函数在缓存中进行检查。如果结果可用，它将以大写字母返回。如果没有，它会将它添加到缓存中，然后以大写字母返回结果。

# 为什么或何时使用记忆？

JavaScript 中的记忆化用于:

1.  昂贵的函数调用。例如，执行输出可能不会改变的网络调用的函数，或者具有大量计算的函数。

2.纯函数(给定相同的输入，函数输出保持不变)。

3.输入范围有限但经常出现的函数。

# 结论

记忆化将结果存储在内存中，因此应该避免在完全不同的情况下多次调用同一个函数。

感谢您阅读本文，希望您觉得有用。如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**

# 更多阅读

[](https://medium.com/javascript-in-plain-english/the-methods-some-and-every-in-javascript-8b303a36eaae) [## JavaScript 中的一些和所有方法

### 了解一些高阶函数。

medium.com](https://medium.com/javascript-in-plain-english/the-methods-some-and-every-in-javascript-8b303a36eaae)