# 前端架构:分页

> 原文：<https://javascript.plainenglish.io/frontend-architecture-pagination-c71b5c2c8a41?source=collection_archive---------2----------------------->

## React.js 的完整分页指南

在前端应用程序中有许多正确的分页方法，但也有许多错误的方法。通常，前端的功能性分页实现和“好的”分页实现之间的差别非常微妙，但是可以很容易地解决。让我们快速了解一种在 React 应用程序中创建分页的有效方法。

![](img/be95c42e9c920e50a9d56654d33ca0d8.png)

Photo by [Daniel McCullough](https://unsplash.com/@d_mccullough?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 设置

让我们用最快的方式来引导 React 应用程序:

```
npx create-react-app sample-project-name
```

对于本指南，我想显示一个带有分页组件的神奇宝贝表，用户可以点击它在页面之间跳转。我想为 UI 逻辑而不是 CSS 创建一个指南，所以我使用了[语义 UI](https://react.semantic-ui.com/) 来实现。以下是如何将它添加到您的项目中:

```
yarn add semantic-ui-reactyarn add semantic-ui-css
```

现在我只是在我的`index.js`上添加了 semantic-ui-css 文件，看起来像这样:

# 初始实施

像我的大多数其他教程一样，我决定在这个项目中使用 [PokéApi](https://pokeapi.co/) 。最初的实现是简单地使用 Semantic-UI 的 [Table](https://react.semantic-ui.com/collections/table/) 和 [Pagination](https://react.semantic-ui.com/addons/pagination/) 组件来创建一个视图，在这个视图中，每一个页面更改都会重新触发一个带有新偏移值的对 PokéApi 的获取。

如果您不熟悉分页，那么“offset”值就是我们要获取记录的范围的起始编号。“limit”值是我们希望在页面上显示的记录数。例如，向`https://my-api.co/entities?offset=0&limit=2`发出请求将会返回一个示例响应，如下所示:

```
[
    { "name": "Dante", "id": 0 },
    { "name": "Vergil", "id" 1 }
]
```

向`https://my-api.co/entities?offset=2&limit=2`发出请求将返回如下示例响应:

```
[
    { "name": "Nero", "id": 2 },
    { "name": "Sparda", "id": 3 }
]
```

抱歉，如果您已经知道这一点，但是现在我们可以看看我最初的分页实现了:

这里我们可以看到，我们使用一个常量将页面上显示的神奇宝贝的总数限制为 10 个(这个常量应该在 UPPER_SNAKE_CASE 中，不过没关系😅).我们将页面初始化为 1，并且在每次页面改变时，我们使用来自`useState`的 setter 来改变我们的`page`变量。当然，我们也从 JSON 和 response 中的`results`属性接收`pokemon`,并将神奇宝贝的`total`号作为`count`属性。我们通过`results`映射以在我们的表上显示神奇宝贝，由于分页的语义 UI 实现将需要总页数，我们使用 total/ `count`并除以我们的页面限制，因为我们每页不显示一个神奇宝贝。为了避免小数，我们使用了`Math.ceil`。

这是一个非常标准的分页实现，很有效，很常见，但是还有很大的改进空间。我发现这个实现的最大问题是，如果我转到应用程序的第二页，然后重新加载页面，我会回到第一页。我们可以通过使用`localStorage`或`sessionStorage`来解决这个问题，但是如果一个用户与另一个用户共享这个列表，并且想要在某个页面向他们显示神奇宝贝的相同视图，他/她也必须与他们共享页码。如果我们能够将这种状态保存在 url 中，而不是使用本地或会话存储，这将会更好，这也将允许用户彼此共享。将它保存在 url 中还允许“超级用户”利用“高级”功能。一个技术“超级”用户，会了解一个 url 的搜索参数，如果你正在创建一个像 JIRA 这样的 B2B 软件，这样的功能可以派上用场。让我们来实现它:

# 改进的分页

下面是改进后的代码:

我们现在使用 [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) 来解析和编辑 URL 中的“页面”搜索参数，以便用户所在的活动页面可以在页面刷新或 URL 共享时得到维护。现在在页面改变时，我们用页码替换窗口的搜索参数，我们的`useEffect`钩子监听这个页面参数的改变，而不是`state`变量`page`。对于初始状态，由于参数可能不存在，我们默认使用`1`。简单地增加几行代码就已经极大地提高了我们 web 应用程序的功能！你认为你还能在这里做些什么改进吗？欢迎在下方回复。

如果你喜欢这样的内容，请跟我来这里吧！😁

## **用简单的英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**