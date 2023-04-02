# 反应上下文:为什么我得到了不必要的重新渲染？

> 原文：<https://javascript.plainenglish.io/react-context-why-am-i-getting-unnecessary-re-renders-b61836d224a7?source=collection_archive---------0----------------------->

![](img/7f27570abfcd0911b4d848a143572b60.png)

Photo by [Tim Dennert](https://unsplash.com/@tim_denn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

嘿，大家好，在过去的一周里，我一直在努力理解 React Context 的一些内幕。我想知道的是——为什么每次上下文变量更新时，我的组件都会重新呈现？

本文包括我的研究结果，以及避免这一问题的建议方法。

为了更好地理解这个问题，我们将使用下面的例子:

someContext.js

在这里，我们创建了一些东西:

*   `randomContext`
*   一个`useRandomContext`定制挂钩
*   一个`randomStore`，我们创建了两个状态变量和一个效果，当`something`变量内容改变时，更新`otherThing`变量。
*   一个`randomContextProvider`用于包装将使用上下文的组件。

现在让我们创建一个组件。

someComponent.js

在这个组件上，我们使用`useRandomContext`钩子来订阅我们的上下文并添加一个按钮，当点击这个按钮时会更新`something`状态变量。

现在，我们已经具备了找到手头“问题”的条件。通过将 console.log(“我刚刚渲染了”)添加到组件的根，我们将能够看到所有的渲染和重新渲染。

让我们按下那个按钮。现在我们期望在控制台上只看到一个`I just rendered`，因为我们只做了一个`setSomething`并更新了`something`变量。我们忘记的是，当`something`改变时，效果将更新`otherThing`，并因此更新我们的上下文，并导致第二次重新渲染和第二次日志。

# 为什么会这样？

每当 *React* 检测到其上下文中的某个变化时，它会触发对所有*上下文*订阅者的重新呈现，以便它们可以接收新的变化。

# 我能做些什么来避免这种情况？

## 拆分上下文

这是 ***首选的*** 方法，包括将经常变化的变量传递给另一个上下文。在这种情况下，我们将创建第二个上下文，在其中添加`otherThing`变量，并将其从另一个上下文中移除。

splitContexts.js

## 将组件一分为二，然后使用 memo

这种方法包括将第一个组件的返回转移到第二个组件。第二个组件将被用备忘录*包装，并接收所需的上下文变量作为道具。*

在这种情况下，第一个组件仍然会重新渲染，但它不会很昂贵，因为它不会做任何事情。

appWithMemo.js

## 在你的退货单上附上使用备忘录

如果我们不想分割我们的组件，我们可以用 *useMemo* 钩子来包装我们的返回。我们的组件仍然会重新呈现，但只有当 *useMemo* 观察到的变量发生变化时，才会返回新的变化。

appWithUseMemo.js

在写这篇文章的时候，有一个 [RFC](https://github.com/reactjs/rfcs/pull/119) 提议创建一个`useContextSelector`。在这个钩子中，我们观察选择器的变化，而不是整个上下文。

# 结论

尽管*上下文 API* 非常有用，但是如果使用不当，它也会造成一些混乱。如今，开发人员倾向于更早地考虑优化性能，而不是建立适当的准则并遵循建议。性能优化通常伴随着牺牲代码可读性的缺点，因此必须小心进行，因为它可能成为一把双刃剑。

当应用这里描述的方法时，开发人员必须总是尝试使用第一种方法，因为它不需要任何类型的救助或记忆。

作为 TLDR 的一名成员，你可以查看 twitter 上促使我写这篇文章的帖子。

我还添加了一个沙箱，里面有解决“问题”的方法。

我希望你喜欢这篇文章，并继续关注未来的文章。