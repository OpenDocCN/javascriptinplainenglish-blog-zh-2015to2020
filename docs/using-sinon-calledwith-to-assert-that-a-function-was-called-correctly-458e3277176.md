# 使用 Sinon calledWith 断言函数被正确调用

> 原文：<https://javascript.plainenglish.io/using-sinon-calledwith-to-assert-that-a-function-was-called-correctly-458e3277176?source=collection_archive---------4----------------------->

![](img/c346bc1b515751b01de36c2bac373666.png)

loosely linked picture to spies, or something

这是一个单元测试示例，用于确认使用正确的参数调用了特定的函数。

# 代码

在上面的代码中，我们有两个函数`calculateTotal`和`updateTotal`。为了测试它们对于给定的输入是否正确工作，我们将测试用正确的参数调用了`databaseUpdater`函数。

# 使用 proxyquire 和 sinon 间谍

Proxyquire 是一个包，它允许您将需要的模块插入到代码中。

这里有一个如何使用 proxyquire 的简单例子

# 分解它

```
const databaseUpdaterSpy = sinon.spy();
const stub = {
  '../model/databaseUpdater': databaseUpdaterSpy,
};
```

这里，我使用`chai`作为断言库，`sinon`创建间谍，`proxyquire`存根外部`databaseUpdater`模块。

现在，当我的代码调用`databaseUpdater`时，它正在调用我的 sinon spy。这意味着我可以断言我的 spy 函数是用正确的参数调用的，如下所示:

```
databaseUpdaterSpy.getCall(0).calledWith()
```

`getCall(0)`函数给出了我的间谍第一次被调用的时间(因为我的间谍可能被调用多次)。然后我可以访问调用它的参数(用`calledWith`函数)。

最后，我们传入一个对象，我们希望用它来断言我们的 spy 函数。因为，在这种情况下，我们的间谍将被赋予一个对象，并且因为 JavaScript 中的对象是通过引用存储的，所以具有相同键和值的两个对象将不相等。为了帮助解决这个问题，sinon 给了我们`sinon.match()`来比较两个物体。

# 把所有的放在一起

```
expect(databaseUpdaterSpy.getCall(0).calledWith(sinon.match({ body: 10 }))).to.be.true;
```

^在这里，我们期待着`databaseUpdater`间谍，第一次被调用时，是用一个看起来和`{ body: 10 }`一模一样的 ab 对象来调用的。

希望这有用。如果你发现任何错误，请评论。