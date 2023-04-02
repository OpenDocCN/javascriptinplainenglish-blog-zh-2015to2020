# JavaScript 最佳实践—代码组织

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-code-organization-e49e78132f66?source=collection_archive---------5----------------------->

![](img/9dc4ec113dd330191ce262abbac56cd7.png)

Photo by [Jeff Sheldon](https://unsplash.com/@ugmonk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但存在问题的代码很容易。

在本文中，我们将看看我们应该如何组织我们的 JavaScript 代码。

# 必须按特定顺序排列的语句

我们应该组织必须以特定顺序运行的代码，这样依赖性就很明显。

例如，我们可以通过编写如下代码来编写需要首先运行的代码:

```
const subtotal = calcSubtotal();
const taxes = calcTaxes(subtotal);
const total = subtotal + taxes;
```

通过上面的代码，我们知道我们需要计算小计，然后才能使用它来计算税金。

然后我们需要两者来计算总数。

另外，`calcTaxes`的参数是`subtotal`，这样我们就知道`subtotal`是`calcTaxes`的依赖项。

# 用注释记录不清楚的依赖关系

如果依赖关系不清楚，那么我们需要用注释来澄清它们。

例如，如果我们没有`calcTaxes`的参数，那么我们可能必须在注释中解释。

我们可以写:

```
/*
 1\. calculate subtotal
  2\. calculate taxes
  3\. calculate total
 */
const subtotal = calcSubtotal();
const taxes = calcTaxes();
const total = subtotal + taxes;
```

# 检查断言或错误处理代码的依赖性

如果我们的代码序列有依赖关系，那么我们希望在继续下一步之前检查它们。

例如，我们可能想要编写断言或错误处理代码，以在继续之前检查所需的值是否可用。

我们可以这样写:

```
const length = calcLength();
const width = calcWidth();
if (length >= 0 && width >= 0) {
  let area = length * width;
}
```

在通过将`length`和`width`相乘来计算面积之前，我们检查`length`和`width`是否为 0 或更大。

如果我们正在编写一个节点应用程序，我们也可以使用`assert`模块来检查我们想要检查的条件。

# 使代码从上到下阅读

如果我们有顺序很重要的代码，那么我们可以从顶部和底部读取代码。

这样，我们可以轻松地阅读代码，而不用在页面上上下跳动。

例如，我们可以这样写:

```
travelData.calcQuarterlyRevenue();
salesData.calcQuarterlyRevenue();
marketingData.calcQuarterlyRevenue();travelData.calcAnnualRevenue();
salesData.calcAnnualRevenue();
marketingData.calcAnnualRevenue();
```

这样，我们可以通过将季度计算和年度计算组合在一起，按顺序读取它们。

![](img/83017e00c6b51c5933bfd3bb77dc9ed0.png)

Photo by [Dan Azzopardi](https://unsplash.com/@dannyazzo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 分组相关语句

将相关语句分组也很重要。把相关的陈述放在一起让我们一起阅读。

阅读相互重叠的不相关代码是很困难的，因为我们不得不跳来跳去地阅读。

# 结论

我们应该以一种方式组织我们的代码，这样我们就不必在页面上跳来跳去地阅读我们的代码。

此外，我们应该将相关的语句分组在一起，以便我们可以一起阅读相关的语句。

还应该添加断言和错误处理代码，以便我们知道运行断言之后的代码需要什么。

## **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击 点击 [**查看我们，并确保订阅该频道😎**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)