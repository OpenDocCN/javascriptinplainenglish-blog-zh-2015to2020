# 带 React 路由器的谷歌分析

> 原文：<https://javascript.plainenglish.io/google-analytics-with-react-router-and-hooks-16d403ddc528?source=collection_archive---------1----------------------->

即使你正在建立一个没有“盈利战略”的有趣的副业，你也应该收集相关的分析。最简单的方法仍然是 **Google Analytics** 但是如果你正在构建一个单页面应用程序，比如一个带有 [React Router](https://reacttraining.com/react-router/web/guides/quick-start) 的 [Create React 应用程序](https://github.com/facebook/create-react-app)，你会发现建议的复制&粘贴解决方案有一个缺口！

![](img/92be164932e25cfb0d424497e9f3ddd0.png)

Photo by [Alex Radelich](https://unsplash.com/@alexradelich?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你将会追踪用户登陆的地方，但是你不会追踪他们导航到不同的路线！如果说客户端路由的好处是极快的导航速度，那么这就是它的坏处:你并不是真的**在服务器上点击一个新的页面，你总是得到相同的`/index.html`服务，并在整个会话过程中保持在那里。**

React 路由器通知你的浏览器你进入了一个新的页面，但这都是假装的！这是为了避免与服务器之间的往返，让你的应用程序感觉更快，响应更快。因此，即使你的浏览器的历史记录认为你从`/`，到`/about`，到`/contact`，它只对服务器做了一个请求，第一个是对`/`。

这就是为什么如果你只遵循谷歌分析的基本设置指南，你只会跟踪最初的请求到`/`，这可能会让你认为你的应用程序中的所有其他路线都是完全无用的，没有人能到达那里。

所以，让我向你展示在客户端 React 应用程序中添加谷歌分析的简单方法，使用一个你自己编写的**钩子**;没有要安装的定制库！

## 谷歌分析片段

如果你[按照流程在 Google Analytics](https://analytics.google.com/analytics/web) 中为你的新应用程序设置一个新账户/资产，你最终会进入[一个页面，要求你“在你网站的每个页面上的< head >标签之后立即粘贴一段代码”](https://developers.google.com/analytics/devguides/collection/gtagjs):

```
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

如上所述，如果你用 React 路由器创建了一个 React 应用程序，你的网站上实际上只有一个页面！因此，将上述内容添加到您的`public/index.html`中，用您的跟踪键替换`GA_MEASUREMENT_ID`是有意义的:

如上所述，这是可行的，但只能追踪用户最初登陆的位置。

![](img/927f60b7ee4919aa9260f9a1ac7673bf.png)

Photo by [Joshua Earle](https://unsplash.com/@joshuaearle?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

但是如果你的应用程序看起来像这样呢:

看来除了回家的`/`路线，你还有两条，`/about`和`/contact`。大概在`Header`的某个地方，你有用户可以点击导航的链接。那么，你如何追踪它呢？

眼尖的读者可能会注意到文档中确实有一小段叫做[单页应用](https://developers.google.com/analytics/devguides/collection/gtagjs/single-page-applications)，在那里他们指示我们当“网站动态加载新页面内容而不是整页加载”时再次调用`gtag`，就像这样:

```
gtag('config', 'UA-1234567-89', {'page_path': '/new-page.html'});
```

但是我们如何知道“何时”，我们如何知道何时我们已经导航到一个新的“客户端”路线？

## 倾听的钩子

幸运的是，React Router 现在给你[一个钩子来获取它的历史对象](https://reacttraining.com/react-router/web/api/Hooks/usehistory)，并且这个历史对象有一个方法来在它改变时附加一个监听器！这是我们将在自己的钩子中使用的:

这可能需要解析很多内容，所以让我们一节一节地分解它:

## 文件名:`useTracking.ts`

对于钩子来说，即使这样也很重要！为了让 React 知道某个东西是一个钩子，需要将其定义为一个以`use`开头的方法。如果不这样做，它就不知道如何运行它所需要的钩子魔法。

有点像为了让 React 知道某个东西是 React 组件，它需要以大写字母开头。我们`import App`，不是`import app`，同样在这里，我们需要`import useTracking`。这不是惯例。我们必须做这件事！

## 进口我们需要的东西

所以，在`useTracking.ts`中，我们将会使用一些有副作用的东西！我们将关注路线历史的变化，并将这些变化通知谷歌。

在 React hooks 的时代，你在`useEffect`中包装这些东西，所以我们把它带进来:

```
import { useEffect } from 'react'
```

`useHistory`是钩子，它将为我们获取路由器在`App.tsx`中使用的历史对象。我们想听听它的变化！

```
import { useHistory } from 'react-router-dom'
```

## 通知打字稿我们已经在`window`中添加了`gtag`

这些例子是用 Typescript 编写的，这样做是为了阻止我们运行不存在的方法。

通常它会在分析我们的源代码后“自动”解决问题。但是 Typescript 如何知道我们在`public/index.html`中内嵌粘贴的代码片段将一个`gtag`方法添加到全局变量中呢？它不知道我们所有的源代码只打算由那个`public/index.html`导入，在它注入那个方法之后。

所以我们必须自己声明`window`中的可能是什么*，就像这样:*

```
declare global {
  interface Window {
    gtag?: (
      key: string,
      trackingId: string,
      config: { page_path: string }
    ) => void
  }
}
```

## 在我们的钩子签名中接受一个`trackingId`

谷歌为这个被美化的`apiKey`起了几个名字；在上面的例子中，我把它记为`UA-YOUR1KEY-HERE`，你当然应该用你自己的、实际的键/项目 id / `GA_MEASUREMENT_ID`来替换它。

实际上，你不太可能想在整个应用程序中用不同的`trackingId`调用这个方法。你通常想在每个应用程序中使用一次，用一个`apiKey`，所以你也可以直接在这个方法体中硬编码它，或者使用`process.env.GA_MEASUREMENT_ID` ，或者任何你创建的环境变量。

然而，我**确实**认为接受一个论点会让发布这个钩子更容易，如果你愿意的话！或者，更有可能的是，当你将它粘贴到下一个应用程序中时，你会记得更新到正确的值！

```
export const useTracking = (
  trackingId: string | undefined = process.env.GA_MEASUREMENT_ID
) => {
```

## 将方法更改为`listen`

如前所述，我们将使用`useHistory`钩子来获取我们的应用程序的`Router`将要使用的历史对象。我们只对它的一个方法感兴趣，你可以用它来附加一个回调来运行浏览器历史记录的改变。实际上，任何时候路线改变！该方法被恰当地命名为`listen`，我们不妨直接将其析构:

```
const { listen } = useHistory()
```

## 额外学分阅读:效果清理

我们还没有真正看到我们设置了什么，但是这一行首先出现，所以它可能会使您出错:

```
useEffect(() => {
    const unlisten = listen((location) => {
```

![](img/c332a63ab49b38136e90ba1fb30b4088.png)

Photo by [Johnny McClung](https://unsplash.com/@johnnymcclung?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这一部分非常专业，所以请随意跳过/略读，不要太担心理解它！你可能以后还会回来。

每当你`useEffect`的时候，你都应该考虑这个效果是否需要清理。如果您正在附加事件监听器或设置超时，答案几乎总是“ **yes** ”。

即使你不这样做，你的应用程序也很有可能像预期的那样工作，但是你将为难以调试的性能问题和错误打开大门。

在我们没有清理的情况下，我们将继续添加侦听器，这些侦听器将继续向 google analytics 发送`page_path`更新，因此“double/triple/n-where-n-equals-how-times-our-App-component-re-renders”——计算我们的页面浏览量。

根据你如何设置你的`App`和你实际使用这个钩子的地方，有可能它永远不会重新运行，并侥幸躲过那颗子弹。但考虑一下，如果你为 Instagram 这样的应用程序设置一个类似的挂钩，每当用户点击时间轴中每条帖子末尾出现的“心脏”按钮时，它就会运行。这需要几十个处理程序，每次心脏从轮廓变为充满颜色时，每个处理程序都可能被重新添加！不要介意把有价值的用户数据变成垃圾，这将是一个明显的 UX 性能打击。

在任何情况下，如果您不想躲避子弹，那么您需要设置删除事件侦听器的指令。有帮助的是，history 的`listen`方法返回了另一个方法，这个方法删除了您刚刚设置的事件监听器！这就是为什么我们编写了`const unlisten = listen(...)`，我们将保留那个`unlisten`方法，并在我们想要移除事件监听器时运行它。

但是我们想什么时候运行它呢？在钩子的清理阶段。您传递给`useEffect`的方法可以有一个您想要在其清理期间运行的方法的返回值。这就是为什么后来我们`return unlisten`。这指示在钩子的清理阶段对`unlisten()`作出反应，这将删除我们设置的“路由改变事件监听器”。

咻，好一段！如果你成功了，干得好！如果你没有通过，*不管怎样*做得很好。这篇文章更多的是关于使用 React 钩子设置 Google Analytics，你可以在不知道`useEffect`的来龙去脉的情况下完成。所以，让我们继续…更多的细节！

## 设置处理程序

所以在上面的超级技术部分，我们已经设置了一个效果和它的清理。如果你跳过了它，你不需要知道细节，因为现在我们将放大到我们正在用`listen`方法做的事情！

那么`listen`是如何工作的呢？好吧，你用你想要的处理程序调用它，当浏览器历史有变化时你想要运行的方法。本质上，每当用户导航到不同的路线！因此，在一个带有 React Router 的应用程序中，每当他们点击一个`Link`组件，或者任何我们已经设置为可编程`history.push('/somewehere-else')`的东西。

`listen`将以一个`location`对象作为第一个参数，以一个`action`作为第二个参数来调用给定的方法。我们不需要`action`在我们的钩子里，所以不要再想它了。

所以我们创建了一个匿名的胖箭头方法，这个方法只使用它的第一个参数`location`。

```
listen((location) => {
```

## 健全性检查

然后，我们检查窗口中是否真的存在一个`gtag`方法，如果不存在，就提前退出。

```
if (!window.gtag) return;
```

在实践中，我们知道情况总是这样，因为我们在`public/index.html`中的内联脚本在这个钩子之前运行。但是*我们真的了解这个*吗？如果我们以稍微不同的方式实现`gtag`注入会怎么样？如果页面中的一个冲突脚本出于某种原因删除了那个全局方法，该怎么办？如果我奶奶有轮子呢？

撇开不太可能的 if 不谈，使用全局作用域永远不会太安全，所以不妨做一个健全性检查。

![](img/e72033e3800f956517f1e99dedc68038.png)

Photo by [Rob Schreckhise](https://unsplash.com/@robschreckhise?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

接下来，我们有一个更需要的健全性检查，因为它可以防止人为错误:

```
if (!trackingId) {
  console.log(
    "Tracking not enabled, as `trackingId` was not given and there is no `GA_MEASUREMENT_ID`."
  )
  return
}
```

我喜欢帮助开发人员找出问题所在的聊天式/有用的日志记录。React 团队是我发现的典范，所以我想在这里尝试一下！

因此，如果我们调用这个钩子而没有使用`trackingId`，并且没有默认的`process.env. GA_MEASUREMENT_ID`，我们将得到这个日志来提示我们为什么在我们的仪表板中除了`/`之外，我们仍然看不到任何页面视图。

然后我们`return`甚至不尝试注册`page_path`更新，因为没有`trackingId`它就不能工作。如果我们的`trackingId` **被**设置，但是**无效**，例如，如果我们使用上面例子中粘贴的字符串`'UA-YOUR1KEY-HERE'`，谷歌的脚本会告诉我们有猫腻。我们可以自己进行打字防范，但我认为那会是太多的工作，几乎没有任何好处。

## 追踪新路线！

最后，我们成功地向谷歌发出信号说“实际上，我们现在在一条新的路线上”！这类似于 React Router 告诉浏览器的历史，就像我们在一条新的路线上一样，只是我们自己编写了这段代码！

```
window.gtag('config', trackingId, { page_path: location.pathname })
```

嗯，我们已经把它从上面提到的文档中粘贴进来，并调整它使用参数…仍然有效！

## 依赖数组

我们以`useEffect`的第二个参数结束，它的依赖项数组:

```
}, [trackingId, listen])
```

关于`useEffect`，我们已经讲了很多；大概太多了吧！所以对于它的依赖数组，我们只能说，我们告诉 React，如果`trackingId` `useTracking`把它作为一个参数，或者`useHistory`的`listen`方法改变了，它就应该重新运行这个效果。

在我们的用例中,`trackingId`和`listen`永远不会改变，所以它只会在第一次渲染时运行一次，并且永远不会重新运行。即使有，也没有问题，因为我们已经在上面的部分做了适当的清理。

钩子就这样了！如果我们设法使用它，我们就完成了！

## 使用钩子

我们的钩子已经准备好了，但是因为它从 React 路由器调用了`useHistory`，所以它只能在一个组件或者其他钩子中运行，这个组件或者钩子在`Router`中。我们上面定义的`App`是路由器内部的**而不是**。这是我们第一次使用路由器，通过渲染`<BrowserRouter>`！

![](img/afdafd2b0da254bbbbfe22f50f6ed90f.png)

Photo by [Javier García](https://unsplash.com/@javigabbo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

因此，让我们进行重构，让事情变得简单一点:

## 重构`App.tsx`

最大的变化是我们创建了命名的导出，`export const App` **返回没有提供者的 JSX**。我们的供应商，`ApolloProvider`和`BrowserRouter`去哪里了？

它们现在在我们的默认导出中，我们已经从仅仅是`export default App`改变为返回包装在我们所有提供者中的应用程序！

```
export default () => (
  <ApolloProvider client={apolloClient}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </ApolloProvider>
)
```

我发现这对测试很有帮助，因为我们现在可以使用我们的命名导出，并在我们自己的定制提供者中呈现它:比如 React Router 的内存路由器，或者 [Apollo](https://www.apollographql.com/docs/react/development-testing/testing/) 的`MockedProvider`。

但是这是另一篇文章的主题！在我们的例子中，它不仅仅是有用的，而且是必要的，因为现在我们可以在`App`中使用我们的钩子，因为`const`现在应该在`BrowserRouter`中呈现；而不是负责渲染`BrowserRouter`本身。一个小小的转变可以让我们…

## `useTracking.ts`中的`App.tsx`

剩下的就是添加我们新钩子的导入，并在`App`的主体中调用它！

```
import { useTracking } from './useTracking'
...
export const App = () => {
  useTracking('UA-YOUR1KEY-HERE')

  return (
    <Container>
      ...
    </Container>
  )
}
```

就这样！`public/index.html`中的片段将首先跟踪“整页加载”，我们的钩子将跟踪每一个后续的路由改变。`App.tsx`这里是完整的、最终的:

![](img/492d1c930f6302b2388b675928764096.png)

Photo by [Nghia Le](https://unsplash.com/@lephunghia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 引人深思的事

如果您对在`public/index.html`中粘贴老派的谷歌代码感到不确定，并且有两个地方可以在您的`GA_MEASUREMENT_ID`中键入，您可以*将*修改为加载`gtag`脚本并进行初始“整页加载”调用的东西。

如果你想了解更多关于这个更“程序化”的解决方案的细节，一定要告诉我；目前，我认为上面讨论的代码是谷歌文档指导你做的事情的更自然的扩展。

说到让我知道，我最近几个周末都在做的“有趣的小项目”，引发了这一切的是 X 的表情符号[。](https://emoji-of.com/)

它运行一个调度任务，该任务写入到 [Postgres](https://www.postgresql.org/) 数据库，以及 [Heroku](https://www.heroku.com/) 上的 [GraphQL](https://www.graphql.com/) 服务器，并在 [Netlify](https://www.netlify.com/) 上有一个[reaction](https://reactjs.org/)前端。[样式组件](https://styled-components.com/)和[对 css 和动画进行弹簧反作用](https://github.com/react-spring/react-spring)。它现在运行谷歌分析，看看是否有人真的在访问！

让我知道你是否喜欢一篇关于上述任何技术的文章！