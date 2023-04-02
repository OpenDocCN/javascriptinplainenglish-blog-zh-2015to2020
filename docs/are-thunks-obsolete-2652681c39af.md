# Thunks 过时了吗？

> 原文：<https://javascript.plainenglish.io/are-thunks-obsolete-2652681c39af?source=collection_archive---------2----------------------->

为什么 React-Redux 挂钩可以取代 React 项目中对 Thunks 的需求。

![](img/a39180bff06d364cf3cc9c069fe0ac6b.png)

Photo by [Daniel Schludi](https://unsplash.com/@schluditsch?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

“铛”被定义为一种沉闷、沉重的声音，如物体落地时发出的声音。哦，等等…那是拟声词的字面解释。虽然，如果在本文结束时您同意我的前提，对 thunk 的文字描述可能是合适的，因为在 React 环境中，Thunk 很可能已经过时了。

React land 中的 Thunk 本质上是一种变通方法，允许调用异步函数，这些函数随后可以将操作分派给 Redux 存储。Thunks 必须作为中间件连接起来才能完成这个工作，这很不幸也很麻烦，幸运的是我有启动我的项目的生成器，这样我就不用每次都自己动手了。考虑将清单 1 作为将 Thunk 作为中间件应用于`configure-store.ts`文件的例子。

Listing 1 — Adding ThunkMiddleware to a configure-store function.

这个存储现在被连接起来使用调度函数，这些函数生成正常的 Redux 操作或异步 Thunk 操作。清单 2 演示了一个异步的 Thunk 动作，该动作可以被分派到一个存储区，该存储区由清单 2 中作为默认导出导出的函数生成。

Listing 2 — An asynchronous action that dispatches an action upon completion of an http call.

正如您所看到的，为了用 Thunk 创建一个异步动作，您的动作生成器必须返回一个将 dispatch 函数作为唯一参数的函数。在该函数中，您可以通过访问主体中的 dispatch 函数来执行任何想要的异步函数。因此，在这个例子中，当一个组件调用`dispatch(getDataWithThunk())`时，它将向一个未指定的 URL 发出一个 HTTP 请求(假设代码示例中有一个真实的 URL ),然后这个 HTTP 调用返回的数据将作为一个参数提供给`setData`动作生成器，并发送到`Provider`组件在 React 应用程序中配置的 Redux 存储。这很有用，而且在 React 挂钩之前，这是用 Redux 完成异步操作的首选方式。

然而，你也可以很容易地用一个反模式编写这个函数，并直接从你提供给`Provider`组件的存储中导入调度函数。这将被认为是单例模式，在大多数情况下是不被允许的。这是因为在大多数情况下，您希望您的组件、动作和 reducers 都是模块化的，并且可以单独测试。通过使用 singleton 模式(如清单 3 所示),您可以在单个商店和使用该商店的组件之间创建紧密耦合。

Listing 3 — The anti-pattern of dispatching directly from a singleton store.

虽然它是一个反模式，但它确实展示了如何在不使用 Thunk 的情况下，在异步调用后完成向存储分发数据。那么，我们能做得更好吗？我们能在不需要使用单例模式的情况下从我们的项目中消除 Thunks 吗？这就是钩子的用处。现在 React 已经引入了钩子，React-Redux 库也开始使用它们，我认为 Thunks 已经没有必要了。考虑清单 4，其中我使用 React-Redux 库中的`useDispatch`钩子创建了一个定制钩子。

Listing 4 — Creating a data service custom hook.

在这个例子中，我创建了一个可以在组件内部实例化的数据服务钩子。这个钩子返回一个`getData`函数，当在一个组件中使用时，这个函数可以被析构。它被认为是一个自定义钩子，因为它在 in 中使用一个钩子，并且它返回可以在组件中使用的值。`useDispatch`钩子将从调用组件中推断哪个存储被配置到最近的`Provider`父组件，并针对该存储分派动作，从而消除了使用单例模式的需要。清单 5 展示了如何在功能组件中使用这个定制钩子。

注意`useDataService`定制钩子如何将`getData`函数作为一个析构变量返回，然后可以在组件内部的任何地方使用。在这个例子中，当组件挂载时,`useEffect`钩子调用`getData`,然后将 HTTP 请求返回的数据发送到 redux 存储。然后，React-Redux 库中的`useSelector`钩子从存储中提取数据，供组件使用，并在选择器返回的数据发生变化时触发组件重新呈现。这演示了如何使用钩子而不是 Thunks 来完整地创建和使用一个完整的异步动作。

那么，Thunks 过时了吗？我很乐意在下面的评论中听到你的意见。

杰森·李·霍奇斯是软件工程团队的领导者，也是《从零开始的软件工程》一书的作者你可以在这里 *了解更多关于这本书的信息或者在线订购* [*。*](http://bit.ly./JHODGES)

*更多内容请看*[***plain English . io***](http://plainenglish.io/)