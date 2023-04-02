# 如何通过重新创建 useEffect()的自动更新特性来实现它

> 原文：<https://javascript.plainenglish.io/understanding-how-to-implement-the-useeffect-hook-by-re-creating-the-automatic-update-feature-709999463656?source=collection_archive---------5----------------------->

## 在我之前的博客中，我接触了一下`useEffect()`钩子的表面，在本地存储中设置了一个编码令牌。我决定更深入地实现这个钩子，作为生命周期方法的替代。

![](img/6d262cbd124bcf456ed63bad280a4951.png)

为了简洁和熟悉起见，演示使用`useEffect()`挂钩的代码将是我的[通过创建自动更新功能完全理解 React](https://medium.com/javascript-in-plain-english/update-feature-while-double-checking-assumptions-8c42e30564d6)和我的[用 Mirage JS](https://medium.com/swlh/custom-routes-with-mirage-js-3d0cc4124897) 博客定制路线中使用的代码的组合。更新功能博客包含要替换的`componentDidMount`和`componentDidUpdate`生命周期方法，而 Mirage JS 博客将利用触发这些生命周期方法的自定义路线。如果一开始这一切听起来很混乱，不要担心，我会一步一步地分解每件事。

# App.js 的起点

让我们从一些代码开始。这段代码对我的更新功能博客来说应该非常熟悉。到目前为止，唯一的主要区别是我通过 Mirage JS 使用模拟 API 来分离数据。

Initial App.js

Initial MovieCard.js

Initial Server.js

在 App.js 中，我使用`useEffect()`钩子作为`componentDidMount`来获取我的模拟服务器中的后端数据。请密切注意，我在钩子中还有第二个参数`[]`。在[反应使用效果文档](https://reactjs.org/docs/hooks-effect.html)中，

> **注**
> 
> 如果使用这种优化，确保数组包含来自组件范围(如 props 和 state)的所有值，这些值会随着时间的推移而改变，并由效果使用。否则，您的代码将引用以前呈现的过时值。了解更多关于[如何处理函数](https://reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies)和[当数组变化太频繁时该做什么](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often)。
> 
> 如果您想运行一个效果并只清理一次(在挂载和卸载时)，您可以传递一个空数组(`[]`)作为第二个参数。这告诉 React 你的效果不依赖于道具或状态的任何值，所以它不需要重新运行。这不是作为特例来处理的——它直接源于依赖关系数组的工作方式。
> 
> 如果你传递一个空数组(`[]`)，效果里面的道具和状态都会有初始值。而通过`[]`作为第二个论证更接近大家熟悉的`componentDidMount`和`componentWillUnmount`心智模型，通常会有[更好的](https://reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies) [解决方案](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often)来避免过于频繁的重新运行效果。此外，不要忘记 React 将运行`useEffect`推迟到浏览器完成绘制之后，所以做额外的工作不是问题。

如果没有第二个参数`[]`，那么`useEffect()`钩子将总是被调用(类似于一个无限循环)。

MovieCard.js gist 中没有发生什么，因为它只是根据来自其父组件(即 App.js)的数据来格式化和显示数据。gist 包含`handleChange`函数，该函数使 MovieCard 组件成为一个受控组件。对于 Server.js 要点，我遵循了 Mirage JS 网站上的[快速入门指南](https://miragejs.com/quickstarts/react/development)。

重复一下，我们仍然在重新创建我的更新功能博客中显示的相同功能，但这次我们将使用`useEffect()`钩子。现在我们已经做好了基础工作，让我们开始实施吧！

# 创建 onSubmit 函数

作为该功能的快速复习，每当我们改变一部电影的描述时，所有同名电影也应该自动改变。让我们创建提交函数，它将向后端提交新的描述。提交函数将在 App 组件中，因为该组件知道所有处于`theater`状态的电影。

首先，让我们将`onSubmit`事件监听器添加到 MovieCard 组件的表单中。

Adding the onSubmit event listener

接下来，我们给 App.js 添加提交功能。

Adding the submitting function

`handleSubmit`函数是对后端的标准更新请求。注意第 20 行，这是到我的模拟服务器的自定义路由。因为定制路由还没有在模拟服务器中创建，所以让我们来创建它。

# 创建自定义路线和逻辑

Adding the custom route to the mock server

在第 47 行，我添加了与 App.js 的第 20 行中显示的 URL 相匹配的自定义路由。在更新特性博客中，我通过迭代电影数组更新了描述，并在电影标题相同的地方更改了电影描述。我们可以在我自定义路由函数处理程序中利用相同的逻辑。

Adding the function handler in the custom route

第 54–64 行是更新描述的逻辑。这实际上是从更新功能博客中复制和粘贴的。添加第 50、51 和 67 行是为了支持更新逻辑。第 50 行获取我们想要更新的确切的`theater`模型，第 51 行获取带有 ES6 对象析构的 App.js `handleSubmit`函数中更新请求主体的信息，第 67 行使用第 54–64 行的新剧院数据更新所选的剧院。第 50、51 和 67 行都使用了 Mirage JS 自带的函数(即分别为`.attrs`、`request.requestBody`和`.update()`)。

既然 theater 已经更新，我想从前端再次获取它以检索新数据。因为我将使用与之前相同的路径和逻辑，所以我可以从`useEffect()`钩子体中提取逻辑，并创建一个单独的函数来获取。

Adding the fetch function

接下来，我需要以某种方式触发`useEffect`钩子，以便重新运行 fetch 函数。

# 将 componentDidMount 和 componentDidUpdate 与 useEffect 结合使用

目前在我的`useEffect()`钩子中，第二个参数是一个空数组。在通读了 useEffect 文档之后，我找到了再次触发我的`useEffect()`钩子的方法。摘自文件:

> 在某些情况下，每次渲染后清除或应用效果可能会导致性能问题。在类组件中，我们可以通过在`componentDidUpdate`中编写一个与`prevProps`或`prevState`的额外比较来解决这个问题:
> 
> `componentDidUpdate(prevProps, prevState) {
> if (prevState.count !== this.state.count) {
> document.title = `You clicked ${this.state.count} times`;
> }
> }`
> 
> 这个需求很常见，它被内置到了`useEffect` Hook API 中。如果某些值在重新渲染之间没有改变，你可以告诉 React to *skip* 应用效果。为此，将一个数组作为可选的第二个参数传递给`useEffect`:
> 
> `useEffect(() => {
> document.title = `You clicked ${count} times`;
> }, [count]); // Only re-run the effect if count changes`

在我们的例子中，我们感兴趣的变化值是电影描述。但是，由于以下原因，不可能使用`theater`状态作为第二个可选参数。

假设我们想使用`[ theater.movies ]`作为第二个参数。这将导致空错误。`theater`状态是`null`，因为初始状态是用`const [theater, setTheater] = useState()`的空括号定义的。

那么用`[ theater ]`呢？这将导致无限的获取循环。当组件挂载时，`useEffect()`钩子将触发(第一次)，调用`fetchTheater`函数。解析获取请求后，`setTheater`将为解析获取的解析数据设置新的`theater`状态。由于`theater`状态改变，再次触发`useEffect()`效果(第二次)。这将调用`fetchTheater`功能，并且`setTheater`将再次设置新的`theater`状态(即使数据根本没有改变)。由于`theater`状态改变，`useEffect()`挂钩将再次触发(第三次)。这就产生了一个无限循环，因为`theater`状态是不断变化的，尽管数据每次都是相同的。

为了避开这些场景，一个新的变量被创造出来，它的唯一目的是触发`useEffect()`成为`componentDidUpdate`。让我们称这个新变量为`toggle.`

Creating a new variable to trigger useEffect()

由于`toggle`的目的是触发`useEffect()`钩子，`toggle`被用作我们的第二个参数。`toggle`的初始状态无关紧要，因为我们只关心改变`toggle`的状态，而不是值本身。更新后的`useEffect()`挂钩现在应该如下所示:

Updated the useEffect() hook

为了触发`useEffect()`，我们需要通过`setToggle`改变`toggle`的状态。下一个问题是何时改变`toggle`状态。由于`fetchTheater`函数获取特定的剧院，我们应该在更新该剧院后改变`toggle`的状态。这发生在`handleSubmit`函数中解决更新获取请求之后。所以看起来我们应该在解决更新获取请求后改变`toggle`的状态。

Updated the handleSubmit function in order to change the toggle’s state

将所有内容拼凑在一起后，完成的 App.js 如下所示:

Completed App.js

# MovieCard.js 中的 useEffect()

与更新功能博客中发生的类似，MovieCard.js 也需要一个`useEffect()`钩子来捕获任何电影描述更新。这类似于它也需要`componentDidUpdate`来反映这些变化。通过遵循 App 组件中的`useEffect()`钩子的相同逻辑，在 MovieCard 组件中实现了一个`useEffect()`钩子。

Added the useEffect() hook to reflect movie description changes

第 8–12 行实现了`useEffect()`钩子。在钩子的主体中，如果需要的话，我使用了`setDescription`来更新电影描述。我只想在父组件(即 App.js)中来自`theater`状态的电影描述发生变化时触发这个挂钩，因此第二个参数是`[ props.movie.description ]`。

在这个最终实现中，我们使用钩子重新创建了自动更新特性！

# 关键外卖

最初，当我切换到使用`useEffect()`钩子时，我认为在需要更新的组件上使用另一个`useEffect()`钩子(例如，MovieCard.js)来代替更新嵌套对象中的键值对时的`componentDidUpdate`(例如，`theater`)会有所帮助。在此示例之后，情况并非如此。嵌套对象中对其他组件有下游影响的更新必须单独考虑。