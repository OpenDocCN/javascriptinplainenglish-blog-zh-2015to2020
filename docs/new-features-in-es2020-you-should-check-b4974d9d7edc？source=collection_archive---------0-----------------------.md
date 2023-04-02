# 您应该检查 ES2020 中的新功能

> 原文：<https://javascript.plainenglish.io/new-features-in-es2020-you-should-check-b4974d9d7edc?source=collection_archive---------0----------------------->

## Java Script 语言

## ES2020 有哪些新功能？

![](img/10f86050800da7664f51840d8f28126b.png)

Photo by [Patrick Ward](https://unsplash.com/@patrikward?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大家新年快乐！随着新的一年，新的 ECMAScript 版本已经发布！对于我这样热爱 ECMAScript 的人来说，这是个好消息。在之前的 ECMAScript 版本中，添加了许多有趣且有用的功能。这次等待我们的是什么？简而言之，让我们来看看吧！

# 动态导入

现在，您可以动态导入文件。

```
import { max } from '../math.js';const nums = [1, 2, 3];
max(...nums); // 3
```

这是我们导入文件的方式。JavaScript 引擎读取文件中的模块，并将它们放入调用这些模块的文件中。但是现在，你可以这样做。

```
const numbs = [1, 2, 3];if (numbs.length) {
  const math = '../math';
  import(math)
    .then(module => {
      module.max(...numbs);
    })
}
```

动态导入返回一个承诺。也就是说你也可以这样写。

```
const math = '../math.js';
const module = await import(math);
module.max(...numbs);
```

这个特性之所以好，是因为您可以在常规 JavaScript 代码中使用动态导入，就像上面的例子一样。

下面是浏览器对动态导入的支持。

*   铬合金:63
*   边缘:不支持
*   火狐浏览器:67
*   IE:不支持
*   歌剧:50
*   Safari: 11.1

# BigInt

当你不得不将两个大到足以导致溢出的数字相加时，你不痛苦吗？

```
Number.MAX_VALUE * 2 // Infinity
```

在这种情况下，BigInt 是一个救世主。

您可以通过调用带括号的`BigInt()`或带数字结尾的‘n’的`2n`来创建 BigInt。

```
const num = 2;
const bigNum = BigInt(num);bigNum; // 2n
bigNum === 2n; // true
```

也可以加减乘除。

```
const bigN = BigInt(10);bigN + bigN; // 20n
bigN * 3n; // 30n
bigN - BigInt('55'); // 45n
bigN / 3n; // 3n
```

注意，`bigN / 3n`返回的是`3n`，不是`3.33333n`。因为你也可以从它的名字中推断出，它只处理整数。所以`bigN / 3n`类似于`Math.floor(10 / 3)`。

但是，很遗憾，不能用浮点数做 BigInt。同样，你也不能同时使用 BigInt 和 Number。

```
BigInt(3.3); 
// Uncaught RangeError BigInt('3.3');
// Uncaught SyntaxErrorBigInt(1) + 1;
// Uncaught TypeError
// Cannot mix BigInt and other types
```

相反，唯一允许混合操作的情况是在比较操作的大小时。

```
BigInt(1) < 2 // true
```

如果在`if`条件下，BigInt 可以像数字一样被计算。

```
function print(n) {
  if (BigInt(n)) {
   console.log('hi');
  } else {
   console.log('bye');
  }
}print(1); // hi
print(0); // bye
```

下面是浏览器对 BigInt 的支持

*   Chrome: 67
*   边缘:不支持
*   火狐浏览器:68
*   IE:不支持
*   歌剧:54
*   Safari:不支持

# 承诺。都解决了

这与`Promise.all`非常相似，但它们之间有显著的区别。`Promise.all`等待所有承诺*被履行*或任何承诺*被拒绝*。另一方面，`Promise.allSettled`并不关心这个。它关心的是知道是否所有的承诺都完成了，无论它们的状态如何。所以每一个输入的承诺都可能被*实现*或者被*拒绝*，但是对`Promise.allSettled`来说都无所谓。只是所有的都要做。

The example is from [Tomas Dostal](https://medium.com/u/9cbdd3686b35?source=post_page-----b4974d9d7edc--------------------------------)

当您想在某个动作之前做一些工作时，这可能非常有用，例如，在用户看到列表页面之前获取所有需要的数据。但是用户可以看到空的项目，因为提取可能会失败。

下面是浏览器对`Promise.allSettled`的支持。

*   铬合金:76
*   边缘:不支持
*   火狐浏览器:71
*   IE:不支持
*   歌剧:未知
*   Safari:未知

# globalThis

这是点燃的。它非常简单易用。

`globalThis`是指您的运行上下文所在的全局`this`上下文。如果你在浏览器上，`globalThis`将是`this`，如果你在 Node 上，`globalThis`将是`global`。因此不再需要考虑不同的环境问题。

```
// worker.js
globalThis === self// node.js
globalThis === global// browser.js
globalThis === window
```

这就是它的工作原理，但是不要在你的代码中使用它！

```
var getGlobal = function () { 
  if (typeof self !== 'undefined') { return self; } 
  if (typeof window !== 'undefined') { return window; } 
  if (typeof global !== 'undefined') { return global; } 
  throw new Error('unable to locate global object'); 
};
```

下面是`gloablThis`的环境支持。

*   铬合金:71
*   边缘:不支持
*   火狐浏览器:65
*   IE:不支持
*   Opera:不支持
*   Safari: 12.1
*   节点:12

# 结论

ES2020 中有许多有趣的新功能。然而，它们中的大多数不支持旧的浏览器，并且它们并不总是在每个环境中都稳定。所以您应该考虑在您的服务中应该支持什么样的环境。

# 资源

*   [导入— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
*   [BigInt — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
*   [Promise.allSettled — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
*   [globalThis — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)
*   [阶段 4 — TC39 Github](https://github.com/tc39/proposals/blob/master/finished-proposals.md)
*   [标记为“ES2020”的功能](https://v8.dev/features/tags/es2020)