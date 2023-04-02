# 测试反应挂钩

> 原文：<https://javascript.plainenglish.io/testing-react-hooks-be74ec3fc0c4?source=collection_archive---------1----------------------->

React 中的钩子是一种新的、流行的、可扩展的方式，用来组织 React 组件中的副作用和状态。通过组合 React 提供的基本挂钩，开发人员可以[构建他们自己的定制挂钩](https://reactjs.org/docs/hooks-custom.html)供他人使用。

[Redux、](https://react-redux.js.org/next/api/hooks) [ApolloClient、](https://www.apollographql.com/docs/react/api/react-hooks/)和 [Callstack](https://github.com/callstack/react-theme-provider) 已经分发了自定义钩子，使用`useContext`钩子访问你的应用商店、客户端和主题。你也可以组合`useEffect`和`useState`来包装 API 请求[或者包装时间的概念。](https://medium.com/javascript-in-plain-english/usetime-react-hook-f57979338de)

很强大。很简单。这两种说法也不是巧合:力量在于简单。

这同样适用于测试定制的 React 钩子。在这篇文章结束时，您会认为测试 React 钩子也很简单。我们将了解:

*   自动化测试的哲学(在任何应用中！)
*   React(挂钩、测试、呈现的异步特性、React 上下文 API)
*   以及一些有用的单元测试工具(Sinon.js clocks，react context，react-hooks-testing-library，mocking in Jest)，

# 密码在哪里？

不要惊慌，这一切都在 GitHub 中。克隆前给回购打个星。

# 运行 React 挂钩

React 钩子不是很正常的函数。它们需要在 React 渲染环境中运行，否则，它们会给你一个恼人的错误。关于这个主题的其他文献建议在测试中构建一个 React 组件，并使用像 Enzyme 这样的组件测试工具来测试钩子的功能。我发现这种方法很脆弱(谁愿意为每次钩子的变化维护一个未使用的组件？)和不必要的。

尝试使用 react-hooks-testing-library。它使测试反应钩子行为、参数和返回值变得轻而易举。例如，比处理酶要容易得多。

关键概念是`renderHook`和`act`实用程序。首先是在哪里指定 React 钩子、上下文和参数。例如，下面是如何设置和测试 React 钩子的参数和返回值。

```
const { result } = renderHook(() => useTime(100));expect(result.current).toBe('mockNow');
```

这个`act`工具是用来触发你的钩子做出反应的副作用，比如事件或者改变道具。与 React 提供的`act` [相同。当我们学习(剧透警告)时间控制时，我们将看到它的作用。](https://reactjs.org/docs/test-utils.html#act)

# 什么是嘲讽？(以此类推)

嘲讽就像印第安纳·琼斯电影的开头。这就像在袋子里装满沙子，这样当我们把沙袋和宝藏交换时，探测器就不会触发陷阱。

![](img/9b55dad4c79158955b1f63c03d7588d4.png)

*   这个包叫做模拟包，是一个类似于依赖项(treasure)的对象或函数
*   探测器是我们正在尝试测试的单元。对于本文，它是我们的 React 钩子
*   陷阱是意想不到的副作用，就像向后端服务器发送 HTTP 请求

检测器必须看不出仿制品和真品之间的区别。例如，您可以模仿`axios`,这样您的钩子就可以完成发送 HTTP 请求的所有步骤，而无需实际发送请求。另外，因为您控制了`axios`的模拟，所以您可以决定何时解决或拒绝承诺。

在 React 中，依赖项可以是您的 package.json 依赖项、通过 import/requires 导出的其他源模块、通过上下文提供的或者在文件中的其他地方定义的。如果你的钩子*依赖*才能工作，那么它就是*依赖*的。

# 测试的核心斗争:嘲讽光谱

自动化测试的价值似乎显而易见。我们希望验证更改不会破坏现有的功能。**它们有助于在变革中保持商业价值**。然而，测试可能会陷入某些陷阱，这些陷阱会增加成本或者无法提供价值。因此，**测试策略首先是关于经济的**。最好的测试维护起来很便宜，也很有价值:它们提供信心，而不会阻碍新的开发。

当涉及到测试任何东西时，从一个 web 应用程序到——是的——一个 React 钩子，我们可以在不同的层次上进行测试，每一个层次都涉及到成本。自动化测试的核心问题是:我们应该嘲笑多少？

如果我们模仿太多的依赖，我们会对系统作为一个整体工作失去信心，并且不得不为更多的覆盖面写更多的东西。低级测试是第一批被主要重构者视为无用的测试。在最坏的情况下，低级测试可能会渗入测试实现而不是行为，当实现改变而不是功能破坏时失败。“单元”测试是嘲讽光谱的极端。想想“笑话”。

然而，如果我们嘲笑的太少，运动部件的数量会导致我们的测试更慢、更不可靠(“剥落”)，并且更难调试/查明故障。然而，它们比单元测试更有信心，覆盖面更广。“端对端”或“系统”测试拥有这种极端的嘲弄谱。想想“硒”。

在嘲笑的范围内，测试单元和端对端测试之间的一切都被称为“集成测试”，或者有时在 API 上下文中称为“功能测试”。这个范围有很多种色调，从单元测试框架中的多单元测试到带有几个模型的端到端测试(对于后者来说, [cypress.io](http://cypress.io) 提供了很多选择。试试吧！).光谱是思考这个问题的更有用的方法。

普遍的观点被称为“左移”:当高层次和低层次测试都是可行的选择时，选择低层次的选项，因为它们更可靠，更有能力测试特定的问题。我要强调的是，这并不意味着你必须有更多的单元测试(就像“测试金字塔”的想法一样)。**测试应该放在商业价值所在的地方。**我发现这通常是在集成测试中，除了关键业务逻辑或算法的详尽测试(单元测试)和登录测试以及一些常见的用例(端到端)。

如果您想了解更多关于软件质量管理和自动化测试的信息，请查看谷歌工程 2014 年的幻灯片。

# 等等，这篇文章不是关于测试 React 钩子的吗？

您的钩子依靠依赖关系来完成几乎任何事情。或许您的钩子从 Redux 存储中读取状态，或许您的钩子触发 HTTP 请求或 GraphQL 查询。当单元测试反应挂钩时，您希望避免依赖于 UI 代码之外的任何东西，比如后端或浏览器 API。这样，您的测试失败意味着问题出在您的钩子上，而不是别的什么地方。

如果您只在一个组件中使用钩子，那么您应该通过在该组件中测试钩子来进行低级集成测试，而不是对钩子进行单元测试。这应该会让你的测试更有价值和信心。**将钩子单元测试集中在重复使用的和关键的钩子上**。

# 我如何嘲笑我的钩子的依赖性？

测试框架提供了创建模拟函数的能力。我发现[用于存根](https://sinonjs.org/releases/v7.5.0/stubs/)的 Sinon.js 文档(与模拟相同，只是术语不同)在教授这个概念和展示例子方面做得很好。在笑话中，嘲弄的方法使用`jest.fn()`或`jest.spyOn()`。

出于我们的目的， *mocking 允许我们在不调用真正的实现的情况下构造一个假的函数或对象*，避免不必要的副作用或不稳定性。

例如，如果我的应用程序中有一个控制警报噪音的类，我可以调用`jest.spyOn(NoiseService, 'playLoudHorn')`让这个函数在测试时不发出真正的噪音。如果该方法应该返回一个布尔值来指示成功/失败，那么您可以在模拟结束时执行`.mockReturnValue(true)`,让模拟的函数为这个测试返回 true。这里内置了很多功能(承诺、模拟实现等)。)，所以我建议多看看 Jest 文档。

关于 stubbing/mock 的困难部分是获得对依赖项的访问，这样我们就可以`spyOn`它，或者我们可以用一个 mock 完全替换它。

本文的其余部分是关于将模拟注入 React 钩子的几种方法。一些原则也适用于钩子之外。

# 使钩子变纯，通过参数传递模拟

在纯函数中，所有依赖项都作为参数传递给函数。这种方法的好处是它使得在测试过程中直接提供(注入)依赖关系。在测试之外，如果依赖项被赋予默认值，那么原始接口可以被保留。

在这个`useTime()`钩子中([阅读关于实现细节的中间文章](https://medium.com/javascript-in-plain-english/usetime-react-hook-f57979338de))，一个`_getTime`参数被暴露以允许指定一个不同的函数来获得当前时间。([我用 Luxon 处理日期时间](https://moment.github.io/luxon/)，我喜欢它胜过 momentjs)。下面是签名(警告，前面有些打字稿):

假设我想控制我的`useTime()`钩子认为现在是什么时间。我可以简单地传入一个模仿函数作为`_getTime`，而不是模仿`Date`对象或监视`DateTime.local`。这里有一个测试，我们就是这样做的:

(For this test, the actual return value of _getTime is not critical, so I used a string)

当然，通过指定默认函数，我可以避免在组件中使用钩子时指定 getTime 选项。下面是 useTime 钩子的一个示例用法，没有指定任何 _getTime。

Notice how the `useTime` usage does not specify _getTime.

# 嘲弄进口

控制/监视/模仿导入的另一种方法是使用 Jest 模块模仿工具(或类似于`proxyquire`的工具)通过模块系统注入模仿。只需指定用于要求依赖关系的确切字符串，然后在导入被测单元之前提供您自己的模拟。

I don’t need to import `getTime` to run this test, since `useTime` imports it. However, I import it for some assertions.

虽然这个例子是可行的，但是我发现随着测试文件的增长，这种方法并不能很好地适应复杂性(并且很容易出现测试内部的突变，这会导致竞争条件和测试代码的不稳定性)。测试应该很容易。请注意。记住，为同一个单元启动另一个测试文件总是一个选项。

# 模拟反应上下文

React 中的依赖关系可以通过 React 上下文 API 提供。例如，这就是连接到 Redux store 的组件能够访问状态的方式。让我们来看一个普通的钩子，它访问一个上下文来构建一个基于配置对象的 URL 字符串。

This hook uses context to get the URL to someone’s resume. Default development values are used, but ConfigurationContext.Provider can be used to set different values in our app.

下面是一个用法示例:

Notice there is no direct way to tell useResumeURL what context values to use here.

在我们的测试中，我们可以依赖默认上下文，或者我们可以使用来自`@testing-library/react-hooks`的`wrapper`选项来指定一个新的上下文，以循环上下文的提供者来注入一个模拟上下文值。让我们双管齐下:

创建了一个快速测试工厂来构建包装器，这样将来的测试就不会绑定到一组特定的值上。这保持了测试的独立性，这是一个最佳实践。

# 非常规的嘲讽

有些依赖关系可以用非常规的方式来控制。`setTimeout`和`setInterval`(一般是时间的概念)可以用 Jest 来控制([和 Sinon.js，本例中用的是](https://sinonjs.org/releases/v7.5.0/fake-timers/))。通常，这些仪器工具被称为“时钟”。

Through experimentation, I found the passage of time has to occur inside `act` callbacks for the hook to register the effects properly.

另一个常见的非常规依赖模仿包装了 XMLHttpRequest (XHR)对象，这样您就可以阻止和断言传出的网络请求，并控制响应和测试结果行为(非常适合测试不常见的 API 失败场景)。然而，这可以是特定于框架的(看着你，$httpBackend)或者被嘲讽函数所覆盖。