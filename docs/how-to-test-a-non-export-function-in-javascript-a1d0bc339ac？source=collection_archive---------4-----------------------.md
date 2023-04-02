# 如何在 JavaScript 中测试非导出函数

> 原文：<https://javascript.plainenglish.io/how-to-test-a-non-export-function-in-javascript-a1d0bc339ac?source=collection_archive---------4----------------------->

![](img/5bea81626738baf3c130b6727cb6bbf0.png)

Photo by [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章解释了如何用 [Rewire](https://github.com/jhnns/rewire) 测试 JavaScript 中的非导出函数，以及为什么不应该使用 House 隐喻导出每个函数。

# 问题背景

在讨论我遇到的问题之前，什么是非导出函数？

非导出函数是只能在该`.js`文件/模块中访问的内部函数。请参考下面的代码示例。

```
// payment.js
const calculateConversionRate = (amount, rate) => {
    return amount * rate;
};​const makePayment = () => {
    // Business logic ......
    const amountToPaid = calculateConversionRate(amount, rate);
    // Continue business logic .....
};​module.exports = {
    makePayment,
}
```

您可能熟悉这样的代码。`calculateConversionRate`是我们只在内部使用的函数。因此，为`calculateConversionRate()`编写单元测试很困难，因为我们无法访问它。

# 房子的比喻

然而，也有一个快速的方法来解决这个问题，我们可以导出`calculateConversionRate`，现在我们可以访问这个函数并为它编写单元测试。

我过去也采用这种方法。虽然这是一个快捷的方式，但它是一个欠考虑的行为。在`payment.js`中，我们希望揭露的是`makePayment`功能。相反，出于单元测试的目的，我们选择公开所有功能。

让我们用房子的比喻来解释这一点。如果你有一所房子，你建造了大门/入口，这就是你希望人们选择进入你的房子。你希望人们通过屋顶、窗户、后院等进入你的房子吗？

> *导出不打算对外使用的功能就像在窗户和屋顶上贴“欢迎”标签一样。*

这在计算机科学中也被称为封装。您正在包装内部函数，并防止它被任何其他模块访问。

现在，您大概了解了为什么不推荐这样做，让我们探索其他的可能性。

# 推荐的解决方案

虽然 Rewire 是作为单元测试库的简单猴子补丁引入的。然而，这个库也很适合用来测试非导出函数。

```
// payment.test.js
const rewire = require('rewire');const payment = rewire('../src/payment.js');
const calculateConversionRate = payment.__get__('calculateConversionRate');test('Calculate Conversion Rate return correct rate', () => {
  const amount = 100;
  const rate = 4.1;
  const expectedResult = amount * rate;
  expect(calculateConversionRate(amount, rate)).toBe(expectedResult);
});
```

使用 rewire 库，我们可以为我们的非导出函数编写单元测试，而不会违反计算机科学中的封装模式。

# 结论

以下是这篇文章的要点。

*   导出所有函数不是一个好主意。这就像你在你的屋顶、窗户、后院等地方贴欢迎标签。
*   讨论了如何利用 Rewire 库轻松测试非导出函数。

感谢您的阅读。下一篇文章再见。

这篇文章最初发表在我的博客上。

# 参考

*   [堆栈溢出](https://stackoverflow.com/questions/14874208/how-to-access-and-test-an-internal-non-exports-function-in-a-node-js-module)