# 3 对可能被遗忘的概念做出反应🤯

> 原文：<https://javascript.plainenglish.io/3-react-concepts-which-can-be-forgotten-6e2c974cd7bb?source=collection_archive---------3----------------------->

> React 经过多年的发展，一些概念被更好的概念所取代

![](img/5b824d003e01dfffbe0a1f9ee8409c57.png)

# 1.使用类📜

每当我们想使用状态或利用生命周期方法时，我们都必须编写一个 *Javascript* 类来创建一个 *React* 组件。即使我们不需要使用这些特性，我们也提前选择了一个类，因为你永远不知道你的组件什么时候会需要一个状态或者副作用。

幸运的是，2019 年初 *React* 16.8 发布了钩子的功能。 *React hooks* 为我们提供了用 *useState* hook 替换状态的能力，这比之前的状态概念要方便得多。

生命周期方法也被 *React* 钩子所取代。*[*use effect*](https://reactjs.org/docs/hooks-effect.html)*运行在每一个渲染上，并给予完全摆脱生命周期方法的机会。**

# **2.使用生命周期方法🔄**

**老实说，当我第一次读到关于[*use effect*](https://reactjs.org/docs/hooks-effect.html)*的时候，我并不知道一个单一的方法如何能够取代几个:`componentDidMount`、`componentDidUpdate`和`componentWillUnmount`🙄***

***事情是这样的, *React* 团队认为生命周期方法是有害的，并决定放弃它们，原来副作用概念也可以达到同样的功能。***

**您可以定义 [*useEffect*](https://reactjs.org/docs/hooks-effect.html) 函数来描述每次渲染后需要做什么，在所描述的函数中，您可以返回另一个函数，该函数将在每次新的渲染之前或组件卸载阶段被调用。**

**你可能会说，从性能的角度来看这是不明智的，或者说“你不应该在每个 rerender 上都使用 API”，你可能是对的。显然，React 团队已经涵盖了这一点，有一些方法可以改变您的[*use effect*](https://reactjs.org/docs/hooks-effect.html)*函数，使其按照您想要的和需要的方式工作。如果你想知道如何做到这一点， *React* 文档已经为你准备好了[📖](https://reactjs.org/docs/hooks-effect.html)***

# **3.将返回内容包装到 div/array🌯**

**如果你曾经使用 *React* 超过一周，你肯定会面临`Adjacent JSX elements must be wrapped in an enclosing tag.`错误。这仅仅意味着您不能从`render`方法中返回多个批处理。**

**以前的解决方案是将元素包装到“div”中，但是这样一来，我们最终会在 *HTML* 中有很多多余的“div”。后来的 *React* *16.0* 为我们提供了通过将元素放入数组然后返回来消除冗余`divs`的能力。**

**随着*的释放*产生反应 *16.2* 片段被引入。现在，如果你想从`render`方法中返回多个元素，你只需简单地将它们包装在`<></>`中，问题就解决了。现在，您可以忘记使用 div/array 包装器来解决这个问题了🤯**

# **还有一个例外💥**

**当您创建错误边界时，您将无法避免使用类和生命周期方法。长话短说，错误边界是为了捕捉子组件中发生的异常而创建的组件。边界需要使用生命周期方法— `getDerivedStateFromError`或`componentDidCatch`。**

**然而，这不是我们在日常开发中使用的，错误边界为项目设置了一次，并留下来做他们的工作。此外， *React* 团队已经在文档中标记了替换这些生命周期方法[的钩子将很快被创建](https://reactjs.org/docs/hooks-faq.html#how-do-lifecycle-methods-correspond-to-hooks)🔨**

# **我不是鼓励你忘记这些❌**

**不要误会我的意思，我并不是想把这些想法从你的脑海中抹去。记住这些真的很有用，尤其是当你开始一个旧的项目的时候。**

**通过这篇文章，我想展示 React 是如何发展的，以及在从头开始开发时可以抛弃哪些概念。**

**你对转向框架中的新概念有什么看法？你是一个创新的爱好者，还是更喜欢坚持时间考验的经典？让我在评论中知道💬**