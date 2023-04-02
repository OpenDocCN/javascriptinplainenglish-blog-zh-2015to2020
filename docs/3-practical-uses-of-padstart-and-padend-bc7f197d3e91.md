# 的 3 个实际用途。padStart()和。padEnd()

> 原文：<https://javascript.plainenglish.io/3-practical-uses-of-padstart-and-padend-bc7f197d3e91?source=collection_archive---------6----------------------->

## 使用这些 ES8 方法将字符串填充到所需的长度

![](img/e1424507ac6de00582cba413e70f16c5.png)

Photo by [Matthew Henry](https://unsplash.com/@matthewhenry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/fluff?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

就像 CSS 让你添加填充到你的盒子来对齐内容一样，ES8 让你填充你的字符串直到它们达到你想要的长度。

假设您有一个在线商店，并且您希望允许用户将他们自己的商品编号添加到您的产品 SKU 中。显然，您可以告诉他们文章编号需要 8 个字符长，或者您可以让他们添加他们想要的任何字符串，然后将这些字符串填充到所需的长度。

下面是实际情况:

```
'1234'.padStart(5); // The output of this is "     1234", so JS adds 5 empty spaces in front of the string. In other words, it pads the beginning (start) of my string with 5 blank characters.
```

同样，我们可以填充字符串的末尾，例如:

```
'Some text'.padEnd(10, 'random')
```

这个方法查看所需的字符串长度——在本例中为 10——以及我想添加作为填充的字符:在本例中为单词“random”中的字母。

由于我的字符串已经有 9 个字符长，填充将只添加“random”的第一个字母，所以最终结果将是“Some textr”。

因此，这两种方法是。 **padStart()** 和**。padEnd()** 。

# 字符串填充的三个实际用途

为什么或什么时候你会使用这些方法？

## 显示信用卡号码

一个实际的例子是只显示信用卡号的一部分，并用*填充剩余的字符:

```
const cardNumber = '2012 4434 1121 2342'; // This is the user inputconst lastDigits = cardNumber.slice(-4); 
```

绳子。 **slice()** 方法提取 beginIndex 和 endIndex 之间的字符，所以在我们的例子中，。slice(-4)从字符串中提取 4 个字符，从末尾开始向后。因此:

```
console.log(lastDigits) // Outputs: 2342
```

现在，如果我们想提醒用户在进行网上支付时使用的是哪种信用卡，我们不想显示完整的卡号。

相反，我们可以用*填充 lastDigits 常量的开头，同时保持显示的掩码数字的长度等于原始卡号的长度:

```
const maskedNumber = lastDigits.padStart(cardNumber.length, '*');console.log(maskedNumber); // Outputs: "************2342"
```

## 保持菜单和收据中的项目对齐

字符串填充方法的另一个实际应用是在菜单列表中显示价格。例如，您希望在一行的开头对齐菜名，在末尾对齐价格，同时保持字符串的长度不变。

```
const dish1 = 'Cheesecake';const dish1Padded = dish1.padEnd(20, '.'); // Outputs 'Cheesecake..........'
```

为此，我们也可以使用串联，但是。padEnd()方法更直接。

## 在线商店中个性化商品参考号

最后，另一种可能需要使用字符串填充的情况是开头提到的:允许用户在网上商店的商品上添加他们自己的参考号。

例如，您的产品具有 10 个字符长的唯一 id。当然，用户可以在搜索栏中输入想要的产品，但是更好的体验是能够将他自己的标识符添加到您的 id 中。

如果您希望您的设计在用户添加太多字符作为参考号(或参考字符串)时不会中断，您可以将输入的长度限制为 10 个字符。

但是，如果您希望用户的参考数字总是 10 个字符长，这样一切看起来都很好，并且对齐，该怎么办呢？然后，您可以用“00000”填充用户参考号，直到达到 10 个字符。

与此类似，如果您销售**多个品牌**，并且希望轻松检索单个品牌的商品，您可以使用特定字符串个性化商品参考号:

```
const itemIkea = '123234234324';const itemIkeaPadded = itemIkea.padStart(16, 'Ikea'); // Outputs 'Ikea123234234324'const itemJysk = '15434324';const itemJyskPadded = itemJysk.padStart(16, 'Jysk****'); // Outputs 'Jysk****15434324'
```

请注意，填充不会改变原始字符串的值。

你用这些方法的目的不同吗？请在下面评论，我很好奇你觉得它们有多有用/实用！

## **用简单英语写的 JavaScript 的一个注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)