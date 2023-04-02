# 如果您每天都在使用 JavaScript，您应该知道即将推出的新 JavaScript 特性

> 原文：<https://javascript.plainenglish.io/upcoming-new-javascript-features-you-should-know-if-you-use-javascript-everyday-8208b477b7a2?source=collection_archive---------0----------------------->

![](img/161162e397ab2e77a6bd645836050a2a.png)

自从 ECMAScript2015(也称为 ES6)发布以来，JavaScript 发生了广泛的变化和改进。这对所有 JavaScript 开发人员来说都是一个好消息。此外，每年都会发布新的 ECMAScript 版本。你可能没有注意到 2019 年 6 月发布的最新 ECMAScript 中添加了哪些功能。我简单给大家展示一下最新版本增加的新特性，说说未来版本的新特性。

> **免责声明**
> 我将向您展示的功能尚未决定是否会出现在下一个版本中。我将在这篇文章中谈论的所有内容目前都处于第三阶段。如果你想了解更多细节，请查看这份回购协议。

# 编辑(2019 年 12 月)—第四阶段

[无效合并操作符](https://github.com/tc39/proposal-nullish-coalescing)和[可选链接](https://github.com/tc39/proposal-optional-chaining)现在处于阶段 4，这是最终阶段！你可以从[这里](https://github.com/tc39/proposals/blob/master/finished-proposals.md)查看这条新闻。

# ECMAScript2019 (ES10)中的功能

## `1\. Array.prototype.flat`

一种创建新数组的方法，其中所有子数组元素递归连接到新数组中，直到指定深度。

```
const array = [1, 2, [3, 4]];
array.flat(); // [1, 2, 3, 4];
```

这是非常有用的，尤其是当你想展平嵌套数组的时候。如果数组的深度超过一个深度，调用一次`flat`不能完全展平数组。`flat`接受一个深度参数，它指的是你想要它进入多少深度来展平数组。

```
// Crazy example
const crazyArray = [1, 2, [3, 4], [[5], [6, [7,8]]]];
crazyArray.flat(Infinity); // [1, 2, 3, 4, 5, 6, 7, 8];// The parameter must be the number type
```

你想搜索的数组越深，展平它所需要的计算时间就越多。注意 ie 和 Edge 不支持这个功能。

## 2.Array.prototype.flatMap

方法首先使用映射函数映射每个元素，然后将结果展平到一个新数组中。

```
const arr = ["it's Sunny in", "", "California"];arr.flatMap(x => x.split(" "));
// ["it's","Sunny","in", "", "California"]
```

`flat`和`flatMap`的区别在于，你可以在`flatMap`中放一个自定义函数来操作每个值。此外，与`flat`不同的是，`flatMap`只展平一个深度数组。返回值应该是数组类型。当您应该在展平阵列之前做些事情时，这将非常有用。

ES10 增加了更多功能。如果你想了解更多，请点击这里。

# 第三阶段的新功能

在第三阶段，有一些有趣的功能建议。我将向你简单介绍其中的一些。

## 数字分隔符

当你把一个很大的数字赋给一个变量的时候，你不觉得这个数字有多大或者你写的对吗？这个提议允许你在数字之间加一条下划线，这样你就可以更容易地计算了。

```
1_000_000_000           // Ah, so a billion
101_475_938.38          // And this is hundreds of millionslet fee = 123_00;       // $123 (12300 cents, apparently)
let fee = 12_300;       // $12,300 (woah, that fee!)
let amount = 12345_00;  // 12,345 (1234500 cents, apparently)
let amount = 123_4500;  // 123.45 (4-fixed financial)
let amount = 1_234_500; // 1,234,500let budget = 1_000_000_000_000;// What is the value of `budget`? It's 1 trillion!
// 
// Let's confirm:
console.log(budget === 10 ** 12); // true
```

一旦这个特性发布，是否使用它将由每个开发者决定，但是有一点是肯定的，这个特性将减少你计算一个数字有多大的麻烦！

## 顶级等待

> 顶级`*await*`使模块能够充当大型异步函数:有了顶级`*await*`，ECMAScript 模块(ESM)可以`*await*`资源，导致其他模块`*import*`它们在开始评估自己的身体之前等待。

这个特性的动机是当你`import`一个有`async`功能的模块时，`async`功能的输出是`undefined`。

```
// awaiting.mjs
import { process } from "./some-module.mjs";
const dynamic = import(computedModuleSpecifier);
const data = fetch(url);
export const output = process((await dynamic).default, await data);
```

有两个文件。`output`可能是`undefined`如果它在承诺任务完成之前被调用。

```
// usage.mjs
import { output } from "./awaiting.mjs";
export function outputPlusValue(value) { return output + value }console.log(outputPlusValue(100));
setTimeout(() => console.log(outputPlusValue(100), 1000);
```

在`awaiting.mjs`中的`await`的承诺得到解决之前，`usage.mjs`不会执行其中的任何语句。

## JavaScript 的无效合并

这将是第三阶段提案中最有用的特征之一。我们经常编写这种代码。

```
const obj = { 
  name: 'James'
};const name = obj.name || 'Jane'; // James
```

如果`obj.name`为 falsy，则返回‘Jane’，所以`undefined`不会被返回。但问题是，在这种情况下，空字符串(`‘’`)也被认为是 falsy。那我们应该像下面这样重新写一遍。

```
const name = (obj.name && obj.name !== '') ? obj.name : 'Jane';
```

每次都像那样写代码是件痛苦的事。该建议仅允许您检查`null`和`undefined`。

## 可选链接

这个提议与 JavaScript 的 *Nullish 合并相一致，尤其是在 TypeScript 中。TypeScript 宣布他们将在下一个发布版本 3.7.0 中包含 JavaScript 的 Nullish 合并。*

```
const city = country && country.city; 
// undefined if city doesn't exist
```

请看示例代码。为了获取`country`对象中的`city`，我们应该检查`country`是否存在，以及`city`是否存在于`country`中。

使用*可选链接*，这段代码可以这样重构。

```
const city = country?.city; // undefined if city doesn't exist
```

对于这种情况，这个特性似乎非常方便和有用。

```
import { fetch } from '../yourFetch.js';(async () => {
  const res = await fetch(); // res && res.data && res.data.cities || undefined
  const cities = res?.data?.cities;
})();
```

## 承诺。任何

> `*Promise.any*`接受一系列承诺并返回由第一个要履行的给定承诺履行的承诺，或者如果所有给定承诺都被拒绝，则返回一系列拒绝原因。

用`async` - `await`，

```
try {
  const first = await Promise.any(promises);
  // Any of the promises was fulfilled.
} catch (error) {
  // All of the promises were rejected.
}
```

有了承诺模式，

```
Promise.any(promises).then(
  (first) => {
    // Any of the promises was fulfilled.
  },
  (error) => {
    // All of the promises were rejected.
  }
);
```

既然有承诺`all`、`allSettled`、`race`，就没有`any`。所以这个特性很简单，但是在需要的情况下很强大。

然而，这个提议还没有经过测试，所以这个提议可能需要更长的时间才能在 ECMAScript 的未来版本中被接受。

# 结论

第三阶段有很多有趣的提议。我迫不及待地想在 ES11 或 ES12 中见到他们。当然，我不需要全部，但是其中一些肯定会使我的代码更加优雅。

## 参考

*   [Array.prototype.flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
*   [array . prototype . flat map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)
*   [Github 中的 TC39 提案](https://github.com/tc39/proposals)
*   [数字分隔符](https://github.com/tc39/proposal-numeric-separator)
*   [顶级等待](https://github.com/tc39/proposal-top-level-await)
*   [JavaScript 的无效合并](https://github.com/tc39/proposal-nullish-coalescing)
*   [可选链接](https://github.com/tc39/proposal-optional-chaining)
*   [言而无信](https://github.com/tc39/proposal-promise-any)