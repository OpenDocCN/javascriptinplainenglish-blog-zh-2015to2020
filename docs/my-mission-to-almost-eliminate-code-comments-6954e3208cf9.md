# 我的任务是消除所有代码注释(几乎)

> 原文：<https://javascript.plainenglish.io/my-mission-to-almost-eliminate-code-comments-6954e3208cf9?source=collection_archive---------10----------------------->

最近我开始注意到我写的大部分注释要么是多余的，要么是我代码中其他错误的借口。

意识到这一点后，我一直在有意识地努力从我写的代码中删除大部分注释。

我发现评论通常可以归为几类。以下是我处理这些问题的方法:

# 应该是方法的注释

我经常发现自己在使用注释，而将代码提取到方法中会更好。

例如:

```
if (customersFirstOrder) {
    // Apply free shipping
    orderCost = orderCost - shipping;
}
```

我发现这更容易阅读:

```
if (customersFirstOrder) {
    ApplyFreeShipping();
}
```

现在我不需要了解*如何应用*免运费，只需要知道当这个条件满足时*应用*。

# 解释条件句的注释

我写了很多这样的陈述:

```
// This order has an outstanding payment
if (order.invoiced && !order.paid) {
    SendReminderEmail();
}
```

第一行合并了两个问题；付款是否到期，以及到期时该做什么。

将谓词移到一个新变量中，使用适当的名称，可以使代码更容易阅读，并且不需要注释:

```
var orderPaymentDue = order.invoiced && !order.paid;
if (orderPaymentDue) {
    SendReminderEmail();
}
```

*注意:我甚至可以更进一步，将谓词封装在订单对象中:* `*order.PaymentDue*`

# 注释是糟糕命名的支柱

这通常发生在我声明一个变量，然后过一段时间再使用它的时候。

如果这个变量命名不当，我会本能地写一个注释解释使用它的代码在做什么。

```
var outstanding = GetOutstandingOrders();
...
// Process all oustanding orders
Process(outstanding);
```

这可以通过首先使用更具描述性的名称来解决:

```
var allOutstandingOrders = GetOutstandingOrders();
...
Process(allOutstandingOrders);
```

*注意:较短的函数，在声明和使用变量之间没有太大的间隙，当然也会有所帮助*

# 注释是不重构的借口

当我写代码的时候，我通常会加上这些注释，因为我脑子里还没有完全成型。

我知道我需要代码做什么，所以我写了第一个能完成工作的东西。

这种代码通常伴随着一个注释，解释*代码做什么*，而不是*为什么做*。

我*应该*做的是返回并重构代码，使其自文档化，使注释没有必要。

# 解释原因的注释

这些评论可能是我唯一会继续写下去的了。

有时候，我会想到我不久前写的代码，却不知道为什么它会以如此奇怪的方式写出来。

通常，这是因为我对它进行了分析，发现它明显更快，或者因为它抓住了一个更直观的方法做同样的事情可能做不到的边缘情况:

```
// When profiled, running two seperate queries and combining the results in 
// memory is 2x faster that doing the join in SQL
var users = sql.GetUsers();
var tasks = sql.GetAllTasks();
... etc ...
```

像这样的评论会留下来。

# 结论

我写的大部分注释都是由懒惰的编码产生的，可以通过简单地命名更好的东西或添加更多的抽象来消除。

我发现当我专注于从代码中移除大部分注释时，它实际上变得更加可读。例外情况是注释解释为什么，而不是如何解释。

如果你喜欢这类内容，我会在 Twitter 上谈论这些东西