# 创建 HTML5 整数输入

> 原文：<https://javascript.plainenglish.io/creating-an-html5-integer-input-aea73dec7058?source=collection_archive---------15----------------------->

## 当 HTML5 数字输入不够时

![](img/403571de9f8d340da3bac8fa5d479f15.png)

Photo by [James Orr](https://unsplash.com/@orrbarone?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我的旅程

在处理数字输入时，我经常需要一个整数输入。例如，您只能向购物车中添加整数数量的商品。想象一下，试图购买一部 iPhone 的一小部分！不幸的是，输入*数字*类型不支持开箱即用——但它可以。让我们深入了解一下。

# 整数输入

输入数字类型将我们带到那里，但不幸的是它也支持小数(例如 1.234)和科学记数法(例如 1e10)。幸运的是，有一种非常简单的方法可以使用`onKeyPress`来忽略这些不受欢迎的字符。该事件处理程序在`onChange`函数之前被触发。这意味着您可以使用`onKeyPress`来检查按下了什么键，如果输入的是不想要的字符，就忽略它。

```
<input
  onChange={e => onChange(e.target.value)}
  onKeyPress={e => {
    if (!/[-\d]/.test(e.key)) {
      e.preventDefault();
    }
  }}
  type="number"
  value={value}
/>
```

如果按下的键(通过`e.key`检查)不是数字或破折号(支持负整数)，默认行为将被阻止，并且`onChange`功能不会被触发。实际上，这将防止字符被输入到输入中。差不多就是这样！

# 可供选择的事物

`onKeyPress`方法不是创建整数输入的唯一方式。

**模式**允许您指定一个 regex 字符串，当表单被提交时，它将与输入匹配。如果输入与正则表达式不匹配，表单将不会被提交。对于整数输入，这应该设置为`[-\d]`，就像`onKeyPress`方法只允许破折号和数字。

```
<input
  onChange={e => onChange(e.target.value)}
  pattern="[-\d]"
  type="number"
  value={value}
/>
```

**步骤**允许您指定有效数字之间的间隔。如果输入不能被指定的步骤整除，则不会提交表单。对于整数输入，应该设置为 1。

```
<input
  onChange={e => onChange(e.target.value)}
  step="1"
  type="number"
  value={value}
/>
```

这两种方法虽然有效，但都要求用户首先提交表单。只有这样，用户才会被告知错误。我个人不喜欢这些方法，因为我更喜欢向用户提供即时反馈——要么首先不允许用户输入无效值，要么立即显示错误。稍后在提交过程中这样做将会使用户困惑。

# 最后的想法

鉴于有这么多常见的用例，我真的希望有一个针对 integer 的本地 HTML5。但是即使没有本地实现，自己实现也很容易。

# 资源

*   [本文 Github 回购](https://github.com/mjchang/medium/tree/master/integer-input)
*   [本文的 code sandbox](https://codesandbox.io/s/github/mjchang/medium/tree/master/integer-input)