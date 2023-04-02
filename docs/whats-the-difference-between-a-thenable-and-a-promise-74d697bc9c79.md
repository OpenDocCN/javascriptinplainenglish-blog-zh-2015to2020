# 可有可无和承诺有什么区别？

> 原文：<https://javascript.plainenglish.io/whats-the-difference-between-a-thenable-and-a-promise-74d697bc9c79?source=collection_archive---------4----------------------->

## 又名“假”承诺与“真”承诺——在猫鼬查询的上下文中解释

![](img/b144d8de0f3559eabad08497db5e155c.png)

Photo by [Dušan Smetana](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我很高兴地做了一些后端编码，在我必须编写一个 mongoose 查询来返回一个项目列表的部分，我搞不清楚是应该在`myModel.find()`方法后面链接`.exec()`还是`.then()`。所以我最终搜索了他们的文档，试图找出我应该使用哪一个。这是他们文件中的一段:

> [猫鼬的询问](http://mongoosejs.com/docs/queries.html)是**不是**的承诺。为了方便起见，它们具有用于 [co](https://www.npmjs.com/package/co) 和异步/等待的`.then()`功能。如果你需要一个完整的承诺，使用`.exec()`功能。

那一刻，我就像…什么是“成熟的承诺”？在这种情况下这意味着什么？

试图弄清楚这个短语的意思的旅程让我找到了这个[链接](https://promisesaplus.com/)，我发现他们的意思是猫鼬的查询是可有可无的，而不是承诺。根据该链接，

> “promise”是一个具有`then`方法的对象或函数，其行为符合该规范。
> 
> “thenable”是定义一个`then`方法的对象或函数。

很自然，现在的问题是“*谁的行为符合本规范*”是什么意思？为了回答这个问题，让我们仔细看看“真正的”又称“完全成熟的承诺”是什么意思(然后我们再看看为什么它与后面的那些不同)。

传统上，`then()`方法返回一个`[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)`。它需要两个参数:回调函数用于`Promise`的成功(onFulfilled)和失败(onRejected)情况。一个承诺可以有三种状态——待定、完成或拒绝。然而，标签并不是这样工作的。我们不能像对待“成熟的承诺”那样传递两个回调函数。用一些代码来说明这一点:

```
UserModel.find().exec((err, users) => { if (err) return res.status(400).send(err); res.status(200).json({ success: true, users, });});
```

这样做很好，因为我们为成功和失败案例传递的两个回调函数被称为 err(失败案例),成功/结果案例被称为 users。

注意，Mongoose 中的所有回调都使用模式:`callback(error, result)` —这不同于 [MDN web 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)中指定的回调函数的顺序。现在，如果我们运行同样的代码，但是像这样用`then()`代替`exec()`:

```
UserModel.find().then((err, users) => { if (err) return res.status(400).send(err); res.status(200).json({ success: true, users, });});
```

这将返回 400 错误请求状态。这是因为 Mongoose 查询不是“完全成熟的承诺”,所以我们不能在查询后面加上一个`.then()`,然后期望它表现得像一个真正的承诺。然而，如果我们真的想使用`.then()`而不是`.exec()`，我们仍然可以在`.then()`之后链接一个`.catch()`用于错误处理:

```
// {_id:1} is included to make this fail
UserModel.find({ _id: 1 }) .then((users) => { res.status(200).json({ success: true, users, }); }) .catch((err) => { res.status(400).send(err); });
```

包含{_id:1}将强制它返回一个 400 错误请求。如果我删除它，用户会得到很好的回报。

最后，记住所有的承诺都是可有可无的，但并不是所有的可有可无的都是承诺。

真心希望这能帮到某个人！感谢您阅读本文:)