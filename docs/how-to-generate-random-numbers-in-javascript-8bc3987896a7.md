# 如何在 JavaScript 中生成随机数

> 原文：<https://javascript.plainenglish.io/how-to-generate-random-numbers-in-javascript-8bc3987896a7?source=collection_archive---------4----------------------->

## 使用内置的`Math.random()`方法

## 无论是为了性能测试还是任何其他目的，生成伪随机数有时都是有用的。

![](img/e6542f408bc4a0cd1100b83792766f50.png)

Photo by [Kristopher Roller](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> `**Math.random()**`函数返回一个范围为 0 到小于 1(包括 0，但不包括 1)的浮点伪随机数，在该范围内近似均匀分布，然后您可以将其缩放到所需的范围— [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

J avaScript 有一个有用的内置`[Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)`函数，可以在必要时生成[伪随机](https://en.wikipedia.org/wiki/Pseudorandomness) [浮点数](https://floating-point-gui.de/languages/javascript/):

```
Math.random() // A number 0 <= x < 1, e.g. 0.6185746631788063
```

要生成特定范围内的整数，请使用以下代码:

```
Math.floor(Math.random() * 1 - Low + High)) + Low // Low <= x < High
```

例如，从 1 到 10 的随机整数是:

```
Math.floor(Math.random() * (1 - 1 + 10)) + 1 // 1 <= x <= 10
```

从 1 到 10 的随机浮点数是:

```
Math.random() * (9) + 1 // 1 <= x < 10 e.g. 3.260141980146828
```

从 0 到 1 的 2 位数精度的随机浮点值为:

```
Math.floor(Math.random()*100)/100 // A number 0 <= x < 1 e.g. 0.07
```

# 可以用`Math.random()`设定种子吗？

P 假随机数有一个[种子](https://en.wikipedia.org/wiki/Random_seed)，如果可配置，可用于重复复制同一组“随机”数。

这将有助于在大型随机数据集上对算法进行自动化测试，而您不希望将该数据集存储为模型数据。

不幸的是，`Math.random()`不允许你设定种子。

> 该实现为随机数生成算法选择初始种子；用户不能选择或重置它。— [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

因此，如果你需要可再生的种子来测试数据，你将需要一个第三方库，比如开源的 [seedrandom.js](https://github.com/davidbau/seedrandom) (可以在 GitHub 上获得)。

# `Math.random()`有多随机？

T 根据熵的[位，由`Math.random()`生成的精确数学随机性因平台而异。](https://en.wikipedia.org/wiki/Entropy_(computing))

这是因为 ECMAScript 规范简单地指定了`Math.random()`应该返回什么，现在应该如何实现它。

> “尽管`Math.random()`能够产生[0，1]以内的数字，但这样做也有一些缺点:
> 
> 引擎之间关于多少位的随机性是不一致的:
> 
> Internet Explorer: 53 位
> 
> Mozilla Firefox: 53 位
> 
> 谷歌 Chrome/node.js: 32 位[*现在是 128 位，见下文]
> 
> 苹果 Safari: 32 位”GitHub 上的 [random-js 库](https://github.com/ckknight/random-js)

*驱动 Chrome 和 Node 的 V8 引擎在 2015 年更新为 128 位`Math.random()`实现，根据 V8 博客[。](https://v8.dev/blog/math-random)

因此，如果你需要比这更随机的东西，你可能想要搜索另一个库，尽管 32 位(即 [4294967295](https://en.wikipedia.org/wiki/4,294,967,295) )可能已经足够了。

熵的程度在平台之间不一致的事实比 32 位限制更不利。

# `Math.random()`不是密码安全的

答另外，`Math.random()`不适合加密，MDN 文档推荐你使用类似 [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Crypto) 的替代品。

> `“Math.random()` *不*提供密码安全随机数。不要将它们用于任何与安全相关的事情。使用 Web Crypto API，更准确地说是使用`[window.crypto.getRandomValues()](https://developer.mozilla.org/en-US/docs/Web/API/RandomSource/getRandomValues)`方法。”— [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

当在安全环境中工作时(即通过 HTTPS/TLS)，内置的 Web 加密 API 可以提供加密安全的数字生成。

Medium 作者 [Daz](https://medium.com/u/ad8b0a0a140e?source=post_page-----8bc3987896a7--------------------------------) 在其博客的一篇文章中给出了一个使用 Web Crypto API 的`[.getRandomValues()](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/getRandomValues)`方法[的简单例子。](https://medium.com/@dazcyril/generating-cryptographic-random-state-in-javascript-in-the-browser-c538b3daae50)

GitHub 上也有免费的加密安全数字库，比如 [secure-random](https://github.com/jprichardson/secure-random) 或🤓表情符号友好的🧐。

但是对于你日常不安全的随机数，比如当学习 [JavaScript 排序算法](https://medium.com/coding-at-dawn/sorts-in-60-seconds-speedy-javascript-interview-answers-on-sorting-acb72bdea8a2)或者[如何对数组进行数字排序](https://medium.com/coding-at-dawn/how-to-sort-an-array-numerically-in-javascript-2b22710e3958)，`Math.random()`就可以了。**快乐编码！**🤠🥳😎

![](img/f9ed803fdb075a8c8f6670181af6a8a4.png)

Photo by [Carson Arias](https://unsplash.com/@carsonarias?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

德里克·奥斯汀博士是《职业规划:如何在 6 个月内成为成功的 6 位数程序员 的作者，该书现已在亚马逊上架。