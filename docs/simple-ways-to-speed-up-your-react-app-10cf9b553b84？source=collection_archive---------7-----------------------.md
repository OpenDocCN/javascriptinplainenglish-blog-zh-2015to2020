# 加速 React 应用程序的简单方法

> 原文：<https://javascript.plainenglish.io/simple-ways-to-speed-up-your-react-app-10cf9b553b84?source=collection_archive---------7----------------------->

![](img/473e16abdb4586cb784a65afa32645e6.png)

Image by [ProDexorite](https://pixabay.com/users/ProDexorite-823142/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=967011) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=967011)

React 可以说是最简单的 UI 库。但是这种轻松是有代价的，那就是性能。在本文中，我将列出一些方法来大幅提高 React 应用程序的性能。

# 用钥匙！

这可能是显而易见的，但却是势在必行的。如果你以列表的形式渲染任何东西(使用`.map`，或者类似`FlatList`的东西)，React 会使用键来优化重新渲染。如果你错过了键，或者，更糟糕的是，使它们不唯一，不仅你的应用程序将运行缓慢，而且可能导致一些令人沮丧的错误。所以，这段代码:

```
data.map(item => <SomeComponent item={item} />)
```

变成了:

```
data.map(item => <SomeComponent key={item.id} item={item} />)
```

您可能想使用索引作为键:

```
data.map((item, index) => <SomeComponent key={index} item={item} />)
```

让我打断你一下。只有当`data`中的项目从未被重新排列、删除或插入时，这才会起作用。即使现在是这种情况，想想未来的几个步骤:你需要在几个冲刺阶段变异`data`吗？我想是的。

# 使用虚拟列表！

与前一点密切相关，这适用于需要呈现大量列出的数据的情况。React 和 React 本地项目都有一些提供类似功能的库。虚拟化列表的工作方式是，只呈现当前在视口中可见的几个项目，其他所有项目都在滚动时缓慢(按需)呈现。

在 React 中，对于稍微复杂一点的用例，您的选择是`[react-window](https://github.com/bvaughn/react-window)`和`[react-virtualized](https://github.com/bvaughn/react-virtualized)`。例如，使用`react-window`会将这段代码:

```
data.map(item => <SomeComponent key={item.id} item={item} />);
```

变成这样:

```
const Row = ({index}) => <SomeComponent item={data[index]} /> // later, in render 
return ( 
  <List height={150} itemCount={data.length}> {Row} </List> 
);
```

现在你可以在`data`中拥有尽可能多的物品，而不用担心它们会永远呈现出来！

在 React Native 中，使用了内置的`FlatList`:

```
<FlatList 
  data={data}
  renderItem={({item}) => <SomeComponent item={item} />} 
/>
```

# 使用 React profiler！

如果在实现了上述两种技术之后，您仍然对 React 应用程序的性能不满意，请考虑使用 React profiler。它会准确地告诉你什么重新渲染，什么时候，需要多长时间，最重要的是，是什么导致了重新渲染。

React profiler 内置于 React devtools 扩展中，因此，很可能您已经安装了它。如果没有，[头这里](https://addons.mozilla.org/en-CA/firefox/addon/react-devtools/)。不幸的是，对于 React 本地开发人员来说，没有简单的方法可以做到这一点。有两种方法可以分析 React 本机应用程序:

1.  打开手机/模拟器上的开发菜单(shake/Ctrl+M)并按下*启用性能叠加*。它将提供一个小窗口，显示 UI 和 JS 线程的当前 FPS，以及跳过的帧数。使用它，你可以大致知道问题出在哪里。
2.  Android 用`systrace`，IOS 用 XCode。这些都是低级工具，需要一些高级设置。这超出了本文的范围，所以[阅读此](https://reactnative.dev/docs/profiling)以了解更多。

# 使用 useMemo！

很有可能，你正在你的应用中做一些数据处理。有可能，您不必在每次重新呈现时都重新处理它。`useMemo`就是专门为此设计的。

`useMemo`接受一个函数和一个依赖列表。然后，它会记住函数返回的值，除非其中一个依赖关系发生变化，否则不会再次调用该函数。例如:

```
const importantData = useMemo(() => calculateImportantData(rawData, [rawData]);
```

想了解更多关于`useMemo`的信息，可以[阅读我的文章](https://everyday.codes/javascript/react-usememo-and-when-you-should-use-it/)详细解释。

# 使用 React 并发模式？

小心行事。React 并发模式是对 React 内部机制的重大改写，它允许异步呈现和多个虚拟 DOM 树。它提供了巨大的性能提升，但是这个特性仍然是实验性的，不适合生产应用。点击阅读更多关于它如何工作以及如何启用它的信息[。](https://everyday.codes/javascript/everything-you-need-to-know-about-react-concurrent-mode-in-2020/)

# 结束语

感谢您的阅读，我希望您学到了一些关于优化 React 代码的新知识。敬请关注更多内容！

# 资源

*   [React 本机性能概述](https://reactnative.dev/docs/performance)
*   [React 协调算法如何工作](https://medium.com/better-programming/how-the-react-reconciliation-algorithm-works-e29bf77a4d78)
*   [反应并发模式](https://medium.com/javascript-in-plain-english/everything-you-need-to-know-about-react-concurrent-mode-in-2020-826af48c1f37)

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**