# React Query 让缓存变得简单

> 原文：<https://javascript.plainenglish.io/react-query-fbefc608ed95?source=collection_archive---------1----------------------->

## 管理服务器状态就是这么简单。

嗨，大家好，我写这篇文章是想和大家分享一个库，它让管理 React 应用程序上的服务器状态变得简单而有趣——React Query。

在这篇博文中，我将做一个简单的介绍，解释是什么导致了它的产生，然后介绍一下`useQuery`钩子是如何工作的。在这篇文章的最后，我将展示一些我们可以进行的定制配置，以及我们如何进行缓存失效、数据重取和乐观更新。

![](img/75c1ce258cdd50c861caeee8d97b7cbb.png)

Photo by [Fachy Marín](https://unsplash.com/@fachymarin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

到目前为止，许多 React 应用程序一直依赖全局状态作为在组件之间共享数据和避免正确钻探方法。

尽管有这个明显的优势，但是全局数据经常被不恰当地使用，并且经常不考虑我们在应用程序中添加的内容是否应该是全局的。另一件经常发生的事情是混合服务器状态和客户端状态。那么客户端状态和服务器状态有什么区别呢？

## 附庸国

这是我们的应用程序所拥有的状态类型。这种状态是临时的、局部的，并且通常在会话之间是不持久的。它是通过没有任何延迟的同步 API 来访问的。这种状态更可靠，因为它通常总是最新的。

## 服务器状态

这种状态是远程持久化的，这意味着我们可以与其他应用程序共享它的所有权。它是异步的，所以这意味着我们需要使用异步 API 来访问它。由于这些情况，这意味着我们不能保证这个状态在我们的应用程序上是最新的。

通过将我们的全局状态混合在服务器和客户端状态中，我们可能最终会做出权衡，当与另一种状态相比时，更倾向于一种类型的状态。

服务器状态有着非常特殊的挑战，这是客户端状态所没有的。这些挑战包括缓存、后台更新、重复数据删除请求、处理过时请求等。

为了应对这些挑战并把我们的服务器状态从我们的客户机状态中分离出来，React Query 应运而生。

# 什么是 React 查询？

React Query 是 React 中用于获取、缓存和更新异步状态的钩子的集合。这是一个简单而小巧的 API，无需配置即可开箱即用。

它是协议不可知的，所以这意味着我们可以使用 REST、GraphQL 或任何用例，并且它支持自动缓存和开箱即用的重新提取。

# 贮藏

React Query 的伟大之处在于，缓存是在“幕后”完成的，我们几乎不需要担心它。

让我们看看`useQuery`钩。

`useQuery` usage

对`useQuery`的每个调用都必须使用**唯一键**和用于解析数据的函数来完成。

密钥必须符合以下类型:

`String | [String, Variables: Object] | falsy | Function => queryKey`

此查询关键字有一些注意事项:

*   它应该是唯一的，因为它将在内部用于重新提取、缓存和重复数据消除相关的查询。
*   在内部，它将被转换成一个数组。因此，如果你只提供一个字符串，它将在内部被转换成['key']。
*   它们是确定性序列化的。这意味着在我们使用['key '，{page，status}]作为键的情况下，顺序是无关紧要的。
*   键按照它们在键数组中出现的顺序作为参数传递给查询函数。
*   如果 falsy 值作为查询键的一部分被传递，那么查询函数将不会被调用。这在我们希望进行串行查询的情况下非常有用(当我们需要来自一个查询的数据来执行下一个查询时)。

查询功能必须考虑以下几点:

`Function(variables) => Promise(data/error)`

这最后一部分令人兴奋，因为这意味着你可以使用 *fetch* 、 *axios* ，或者你应用程序上的任何东西，只要它能解析你需要的数据。

## 一些缓存注意事项

*   解析后，呈现的查询结果将变得陈旧。这意味着，如果 refetchOnWindowFocus 属性处于活动状态，它们将在每次新的装载或页面焦点时自动重新提取。为了避免这种情况，我们应该指定 *staleTime* 属性。
*   当用户重新聚焦浏览器窗口时，过时的查询将被重新提取。为了避免这种情况，我们应该改变 *refetchOnWindowFocus* 属性。

# 缓存失效、数据重新提取和乐观更新

如果我们需要执行服务器副作用，比如创建/更新/删除数据，我们有一个`useMutation`钩子。这个钩子真的很有用，如果我们因为一些 POST 而需要数据重取的话。

我们可以利用它的 onSuccess 选项来访问`queryCache`对象，并执行一些查询重取和当前缓存失效。这可以从下面的例子中看出

on addBlogPost mutation success refetch all queries that have “posts” as key.

如果我们愿意，我们还可以利用`onMutate`选项并执行乐观更新。这将允许我们在服务器端发生突变之前更新我们的 UI，因此必须小心使用，如果突变失败，我们可能最终向用户显示不同步的数据。为了避免这最后一个用例，我们可以回滚我们的更改。

When an optimistic update is performed, a reference to the previousPosts is returned in case we have to rollback onError.

# 配置

如果您经常定义 staleTime 属性或任何其他属性，您可以利用 ReactQueryConfigProvider 来包装您的应用程序，并设置 React 查询挂钩将使用的默认值。

Setting some defaults using the ReactQueryConfigProvider

# 结论

如您所见，使用 React Query 管理服务器状态要简单得多。

如果您想了解更多，我邀请您阅读[官方文档](https://github.com/tannerlinsley/react-query)并观看 [Tanner Linsley 在 React 峰会](https://www.youtube.com/watch?v=Wyk01ySxg0A&feature=youtu.be&t=12192)上的演讲，他在演讲中介绍了 React Query 并展示了一个更实用的示例。

我希望你喜欢，并期待下一集。

祝你周末愉快，下次再见！

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个关注来表达爱意吧:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****