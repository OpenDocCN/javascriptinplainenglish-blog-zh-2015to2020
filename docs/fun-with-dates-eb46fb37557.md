# 约会的乐趣

> 原文：<https://javascript.plainenglish.io/fun-with-dates-eb46fb37557?source=collection_archive---------8----------------------->

## 让 JavaScript 日期更像 Ruby

![](img/d295156f587eaa7208cf952c3a2a4b46.png)

本帖最初发表于 [***TK 的博客***](https://leandrotk.github.io/tk/2020/04/fun-with-dates/index.html) ***。***

作为一名前 Ruby 专家，我一直喜欢使用 Ruby 日期(注意:不是时区部分)。我喜欢 Ruby 和 Rails 提供方法来处理 Date 对象的人性化方式。

在 Ruby 中，我们可以通过执行以下操作来获取当前日期:

这太酷了！我可以通过调用`today`方法向日期对象发送一条简单的消息“嘿，给我提供`today date`”。

或者干脆弄了`year`、`month`、`day`。

使用 Rails，也可以调用`yesterday`方法。

Rails 还提供了其他有趣的 API:`beginning_of_month`、`minutes.ago`、`days.ago`。

所以在使用 Ruby 和 Rails 很长时间后，我开始越来越多地使用 JavaScript。但是 JavaScript Date 对象对我来说真的很奇怪。我想使用所有的 Ruby/Rails 日期 API，但是使用 JavaScript 和 Typescript。

我不想在 JavaScript Date 对象中模仿补丁或构建新方法。我可以只提供一些简单的功能，并在内部处理数据。

# 约会日期

首先:我想更好地理解日期对象。我们如何创造它？

只需实例化日期对象。我们得到了`now`(当前日期)的表示。

我需要尝试的其他 API 是:`getDate`、`getMonth`和`getFullYear`。这些都是处理日期的方法。

我们可以在这里试验一大堆其他方法，但我认为我们可以进入下一部分。

# 约会的乐趣

在这一部分，我们将构建函数！我想尝试创建这个 API:

*   天
*   月
*   年
*   今天
*   昨天
*   假日开始
*   开始月份
*   开始财年
*   get(1)。达雅戈
*   get(2)。戴萨戈
*   get(1)。蒙塔戈
*   get(2)。monthsAgo
*   get(1)。一年前
*   get(2)。几年前

# 年、月、日

在这种情况下，我们提供一个日期，它将返回我们提供的这个日期的日期。

我们可以像这样使用它:

# 今天和昨天

使用`today`函数，我们只需返回`new Date()`就可以了。但是这会返回包含“时间”的`now`的表示。

但是如果能回到那天的开始就太好了。我们可以简单地将日、月、年传递给`Date`，它会为我们生成这个。

太好了。`yesterday`函数的工作方式非常相似。减去这一天，我们就可以走了。

但是如果这一天是一个月的第一天，我们减去这一天会发生什么呢？

如果是一年的第一天会发生什么？

是的，JavaScript 也可以很聪明！

有了这两个新函数，我们还可以重构逻辑，将分离的日期放入一个单独的函数中。

让我们改进这一点！这个返回的类型可以是 Typescript `type`。

现在不那么冗长了:

在这种情况下，我们总是返回当前日期的`day`、`month`和`year`属性。但是如果我们想传递不同的日期呢？拯救行动的新论据:

现在我们有了一个可以接收新日期的函数，但是如果没有，它就使用默认值:`now`的表示。

我们的函数`today`和`yesterday`现在看起来怎么样？

这两个函数都使用`getSeparatedDate`函数来获取日期属性并返回适当的日期。

# 一切的开始

为了构建`beginningOfDay`，它看起来与`today`函数完全一样，因为我们希望是当前日期，但却是一天的开始。

这里没什么特别的。

但是如果你没有注意到的话，请稍加注意:首先，我构建这个函数是为了获取当天的开始时间。但我想让它足够灵活，以便在其他日子也能开始一天的工作。

所以“论证”，对吧？现在该函数接收一个日期，但也可以不接收。我只是用当前日期的默认值来处理它。

对于`beginningOfMonth`，它看起来几乎一样，但是我们没有使用`day`，而是将其设置为`1`。

你猜对了，`beginningOfYear`也差不多。而且还改变了`month`属性。

# 回到过去

现在是`get(1).dayAgo` API。我们可以构建一个`get`函数来接收一个`number`并返回一个对象，比如:

对于这个对象的每个属性，它将是我们期望的返回值。

一个`DateAgo`型怎么样？

现在使用新的类型:

我们通过处理已知的日期对象来构建每个属性:`dayAgo`、`monthAgo`和`yearAgo`。

但是现在我们还需要实现复数形式的对象:`daysAgo`、`monthsAgo`和`yearsAgo`。但仅适用于大于 1 的数字。

对于这些新属性，我们不需要再次创建一个全新的日期。我们可以从单个属性中使用相同的值。

我们还需要处理收到的`number`。

*   如果大于 1:返回具有多个属性的对象
*   否则:返回具有单数属性的对象

*   在本例中，我还创建了`DatesAgo`类型并使用了 Typescript `Union Type`特性。
*   我们重用奇异值。
*   并做一个简单的三元处理收到的数字。

但是如果我们传递一个`0`或者负值呢？我们可以抛出一个错误:

约会也可以很有趣。学习基本概念，只是玩一玩，你会喜欢的！我希望这篇文章对你有价值！

# 资源

*   [日期— JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
*   [Ruby on Rails 日期 API](https://api.rubyonrails.org/classes/Date.html)
*   [红宝石日期 API](https://ruby-doc.org/stdlib-2.7.1/libdoc/date/rdoc/Date.html)
*   [约会图书馆](https://github.com/leandrotk/dating)
*   [打字稿学习 001:对象析构](https://leandrotk.github.io/tk/2020/04/typescript-learnings/001-object-destructuring.html)
*   [理解 JavaScript 中的日期和时间](https://www.digitalocean.com/community/tutorials/understanding-date-and-time-in-javascript)