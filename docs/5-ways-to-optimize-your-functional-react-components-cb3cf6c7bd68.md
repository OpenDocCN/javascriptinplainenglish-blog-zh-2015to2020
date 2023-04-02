# 优化功能反应元件的 5 种方法

> 原文：<https://javascript.plainenglish.io/5-ways-to-optimize-your-functional-react-components-cb3cf6c7bd68?source=collection_archive---------0----------------------->

![](img/6af794b293f5e13100661eaa8cc159b8.png)

Original Photo by [Leo Rivas](https://unsplash.com/@leorivas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

1.  [尽可能避免箭头功能](#a5b8)
2.  [使用 useMemo 缓存昂贵的计算结果](#6a2b)
3.  [用碎片代替空地](#9433)
4.  [使用油门防止过度执行](#ab1e)
5.  [使用 useRef 避免组件重新渲染](#fb7b)

# 1.尽可能避免使用箭头函数

你很可能听说过使用 arrow 函数比使用要好。绑定，这绝对是正确的。但在使用 functional reactor 时，我们在许多情况下也可以避免使用 arrow 函数，因为每次重新呈现组件时，它们都会创建一个新函数。我们可以通过使用 function 关键字而不是使用箭头来简单地定义一个函数。

例如，此组件使用 3 个箭头函数:

但我们可以对其进行优化，改为使用 0 个 arrow 函数:

这将防止在每次重新呈现时创建新函数，并且您仍然可以完全访问所需的任何变量。

注意:如果传递给子组件的回调函数有多个参数，则可以使用 arrow 函数作为支柱，但内部函数可以保留为函数。例如:

# 2.使用 useMemo 缓存昂贵的计算

[https://reactjs.org/docs/hooks-reference.html#usememo](https://reactjs.org/docs/hooks-reference.html#usememo)

useMemo 是一个很好的方法，可以防止在组件的每次重新呈现时调用代价高昂的函数，就像在更新状态时一样。相反，您可以告诉 useMemo 监视某些变量(即函数实际使用的变量)，并且仅在这些变量发生更改时重新计算。

这里有一个简单的例子:无论何时更改 a、b 或 c，都会调用 calc 函数，即使 c 与计算无关:

如果使用 useMemo，则仅在 a 或 b 发生变化时进行计算，而不会在 c:

这可以扩展到许多不同的使用情形；例如，您可以返回一个 JSX 组件而不是一个值，这意味着您可以确定何时希望页面的某些部分重新呈现或保持不变。所以，下次当你需要根据一个变量的变化来更新你的组件，并开始按习惯键入“useEffect”时，花点时间考虑一下在这种情况下 useMemo 是否会更好地工作。

# 3.使用碎片而不是空的 div

[https://reactjs.org/docs/fragments.html](https://reactjs.org/docs/fragments.html)

从函数返回的任何 JSX 都必须在一个容器中，所以经常使用一个空的 div(没有属性)来分组多个元素。但是 React 建议使用片段，这不会给 DOM 结构增加额外的节点。这使得阅读和调试代码变得更加容易，尤其是当您从浏览器中检查元素时。

例如，代替这个:

你可以用这个:

# 4.使用节流来防止过度执行

[https://reactjs.org/docs/faq-functions.html#throttle](https://reactjs.org/docs/faq-functions.html#throttle)

您可以使用 lodash 中的 throttle 函数来防止一个函数在给定的时间范围内被执行多次。这在处理搜索、加载或基本上任何类型的交互时非常方便，在这些交互中，用户( ***或甚至另一段代码*** )可以在第一次执行结束之前多次调用一个函数。

ReactJS 在上面的链接中有一个非常好的例子，你每秒只能点击一次按钮；在这里，它被转换为功能组件:

注意:lodash 的去抖功能可以以类似的方式在指定的时间后执行一个功能(对于处理快速/多个键盘事件特别有用)

# 5.使用 useRef 来避免组件重新呈现

我在多个应用程序中多次遇到的一个问题是，输入的 onChange 事件导致整个组件在每次键盘点击后重新呈现。大多数时候，React 和现代浏览器(如 Chrome 和 Firefox)可以很好地处理这种更新，你不会注意到任何延迟。但是当一个组件变得有数百(如果不是数千)行长，有几十个状态变量和函数时，渲染延迟开始变得非常明显，特别是如果你必须支持 IE11 这样的旧浏览器。这可能从几百毫秒到几秒钟不等。

一种解决方案(尽管是非标准的方式)是使用 useRef 来更新目标值，而不使用 useState。例如，考虑这个组件:

在每次按键输入时，您将看到控制台日志被执行，整个组件将重新呈现。但是我们可以通过使用 useRef 来防止这种情况:

这里，屏幕上显示的文本会更新，而无需重新呈现整个组件。这可以显著提高大型组件的速度。

希望这五点能让你在处理复杂问题时有更多的工具和选择。快乐函数式 React 编码！