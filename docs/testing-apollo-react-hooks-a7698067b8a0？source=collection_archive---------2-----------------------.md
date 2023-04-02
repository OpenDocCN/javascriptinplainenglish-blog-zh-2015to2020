# 测试阿波罗反应钩

> 原文：<https://javascript.plainenglish.io/testing-apollo-react-hooks-a7698067b8a0?source=collection_archive---------2----------------------->

测试你的软件是最重要的步骤之一，不幸的是，这也是最难的步骤之一。每当你在项目中引入新技术时，你必须考虑如何测试它们。您还需要确保这些测试健壮、灵活，并为您的客户带来卓越的用户体验。

![](img/a94d6c62d8b6819b4e62188221c4f1b3.png)

阅读本指南后，你将知道如何使用 [Jest](https://jestjs.io/docs/en/getting-started) 和 [React 测试库](https://testing-library.com/docs/react-testing-library/intro)来测试 [@apollo/react-hooks](https://www.apollographql.com/docs/react/api/react-hooks/) 。

Apollo 的文档不是已经教你如何使用@apollo/react-hooks 来[测试 React 组件了吗？Apollo 文档中的测试方法可能导致不完整的测试用例。MockProvider 要求您模拟函数输入和结果。结果应该来自 GraphQL 服务器，所以可以随意模仿。输入应该提供给应用程序的变异或查询，而不是模仿。](https://www.apollographql.com/docs/react/development-testing/testing/)

来自`[@apollo/react-testing](https://www.apollographql.com/docs/react/api/react-testing/)`的 MockedProvider 组件迫使您模拟查询的输入和输出，这并没有为我们提供我们正在寻找的测试覆盖。我们想做的是:从我们的表单元素动态地提供输入，并模拟结果。

为什么？React 组件负责查询的输入，而不是输出。服务器负责输出。当我们动态提供输入时，我们可以断言查询是用表单中提供的输入调用的。

## React 测试库？

如果你已经熟悉 React 测试库，请跳到*测试登录表单。*

你可能想知道为什么你会选择使用 React 测试库而不是酶。虽然 Enzyme 很棒，也达到了它的目的，但 React 测试库已经与测试理念保持一致，为您的客户带来更强大的测试和更可靠的软件。这个库是由 Kent C. Dodds 提供的，他可能是最著名的 JavaScript 测试权威。

TL；为什么你应该在酶之前到达 React 测试库的原因是 React 测试库使你难以测试实现细节。相反，它试图强迫你以用户使用的方式测试你的软件。

> 你想为你的 React 组件编写可维护的测试。作为这一目标的一部分，您希望您的测试避免包含组件的实现细节，而是专注于使您的测试给您预期的信心。作为其中的一部分，您希望您的测试库在长期内是可维护的，这样您的组件的重构(对实现而不是功能的更改)就不会中断您的测试并减慢您和您的团队的速度。——【https://github.com/testing-library/react-testing-library 

## 测试登录表单

为了测试包含 GraphQL 查询或变体的组件，我们想要模拟 Apollo 客户端，这将允许我们创建返回模拟值的处理程序，并查看该处理程序调用了什么。

让我们从任何应用程序中最常见的表单之一开始，即登录表单。

在上面的登录表单中，useLoginMutation 来自一个生成文件，由 [GraphQL 代码生成器](https://graphql-code-generator.com/)创建。生成文件的输出是用 [useMutation](https://www.apollographql.com/docs/react/api/react-hooks/#usemutation) 创建的@apollo/react-hook。

**为了测试我们想要的登录表单:**

1.  断言使用表单中的数据调用 loginMutation
2.  如果突变返回一个错误，断言显示一个错误

您最初的想法可能是嘲笑 useLoginMutation。不幸的是，这很复杂，也没有必要。问题是:useLoginMutation 是一个钩子生成器，它返回 LoginMutation，以及关于函数调用的元数据，如数据、调用、错误和加载。

测试表单更容易也更合适的方法是使用@apollo/react-hooks 包中的 ApolloProvider，并用 [mock-apollo-client](https://www.npmjs.com/package/mock-apollo-client) 模拟客户端。

在 devDependencies 中安装所需的模块:

```
npm i mock-apollo-client --save-dev
```

或者

```
yarn add mock-apollo-client -D
```

**以上测试:**

1.  初始化模拟客户端
2.  定义了登录变异的模拟响应
3.  呈现登录表单
4.  用定义的数据填充表单
5.  单击提交按钮，触发 loginMutation
6.  断言 loginMutation 是用表单中的数据调用的

## 测试重新提取查询

在调用了一个突变之后，通常使用 [refetchQueries](https://www.apollographql.com/docs/react/data/mutations/#options) 获取数据，或者使用 awaitRefetchQueries 异步获取数据。

我们的测试应该断言 refetchQueries 被调用，并且是用正确的输入调用的。

登录表单有一个对“Me”的 refetch 查询，该查询返回当前登录的用户。

将原始查询导入到您的测试中，并向模拟客户端添加另一个处理程序。

我们现在可以断言 currentUserQueryHandler 被调用了一次。

上述解决方案将帮助您测试大多数应用程序逻辑。可能有一些场景没有在这里讨论。如果你在 React 中测试 GraphQL 挂钩需要帮助，请在评论区告诉我。

## 🌎让我们保持联系

[在 YouTube 上订阅](https://www.youtube.com/TomDoesTech)
[不和](https://discord.gg/4ae2Esm6P7)
[推特](https://twitter.com/tomdoes_tech)
[抖音](https://www.tiktok.com/@tomdoestech)
[脸书](https://www.facebook.com/tomdoestech)
[insta gram](https://www.instagram.com/tomdoestech)
[给我买杯咖啡](https://www.buymeacoffee.com/tomn)