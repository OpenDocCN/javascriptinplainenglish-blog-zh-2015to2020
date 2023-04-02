# 基本的内置反应挂钩——使用状态和使用效果

> 原文：<https://javascript.plainenglish.io/basic-built-in-react-hooks-usestate-and-useeffect-6011271ed0af?source=collection_archive---------10----------------------->

![](img/8b53c621a0c284ff321de32972203bb5.png)

Photo by [Chandra Maharzan](https://unsplash.com/@chandramaharzan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将看看一些基本的内置 React 挂钩，包括`useState`和`useEffect`。

# `useState`

`useState`钩子让我们管理功能组件的内部状态。它接受一个初始值作为参数，并返回一个具有当前状态的数组和一个更新它的函数。

当组件最初呈现时，它返回初始状态。

如果新值是使用以前的状态计算出来的，我们可以传入一个函数来更新这个值。

例如，我们可以编写以下代码来更新基于先前值的值:

```
function App() {
  const [count, setCount] = React.useState(0);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(0)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>
        Decrement
      </button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>
        Increment
      </button>
    </>
  );
}
```

在上面的代码中，我们有:

```
setCount(prevCount => prevCount - 1)}
```

并且:

```
setCount(prevCount => prevCount + 1)}
```

该函数通过传入将先前的计数作为参数并返回新计数的函数来分别递减和递增`count`。

否则，我们可以向状态更新函数传递新值，就像我们在:

```
setCount(0)
```

`useState`不会自动合并更新对象。我们可以用 spread 语法复制这种行为:

```
function App() {
  const [nums, setNums] = React.useState({});
  return (
    <>
      <p>{Object.keys(nums).join(",")}</p>
      <button
        onClick={() =>
          setNums(oldNums => {
            const randomObj = { [Math.random()]: Math.random() };
            return { ...oldNums, ...randomObj };
          })
        }
      >
        Click Me
      </button>
    </>
  );
}
```

在上面的代码中，我们有:

```
setNums(oldNums => {
            const randomObj = { [Math.random()]: Math.random() };
            return { ...oldNums, ...randomObj };
          })
```

用一个随机数作为键和值来创建一个`randomObj`对象，我们将它合并到另一个具有旧值的对象中，然后返回它。

然后我们显示它:

```
Object.keys(nums).join(",")
```

通过得到钥匙并把它们连接在一起。

## 惰性初始状态

如果我们想延迟初始状态的设置，我们可以向`useState`传递一个函数。

如果我们传入一个函数，它会在初始渲染后被忽略。

如果初始状态是通过一些昂贵的操作计算出来的，这是很有用的。

例如，我们可以写:

```
function App() {
  const [count, setCount] = React.useState(() => 0);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(() => 0)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>
        Decrement
      </button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>
        Increment
      </button>
    </>
  );
}
```

在上面的代码中，我们有:

```
React.useState(() => 0)
```

这个函数只返回 0。

我们会看到和以前一样的结果。

## 退出状态更新

如果我们用与当前值相同的值更新一个状态钩子。React 将跳过更新状态，而不渲染子对象或激发效果。

React 使用`Object.is()`来比较当前状态和新状态，除了`+0`和`-0`被视为不相等而`NaN`等于自身之外，它与`===`操作符很接近。

在退出之前，React 可能仍然会渲染，但是如果它发现新旧值相同，它不会更深入到树中。

![](img/ef2b1c471ebe5cfd8373aafbbf2a6142.png)

Photo by [Louis Reed](https://unsplash.com/@_louisreed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `useEffect`

我们可以使用`useEffect`钩子来做各种不允许在函数组件主体内部进行的操作，这是渲染阶段之外的事情。

因此，我们可以用它来做任何突变、订阅、设置定时器和其他副作用。

运行代码需要回调。

我们可以在内部返回一个函数，以便在每次渲染后以及组件卸载时运行任何清理代码。

在延迟事件期间，传入`useEffect`的回调在布局和绘制之后触发。

这使得它适合于运行不应该阻止浏览器更新屏幕的操作。

必须同步运行的代码可以放到`useLayoutEffect`钩子的回调中，这是`useEffect`的同步版本。

它肯定会在任何新的渲染之前启动。React 将总是在开始新的更新之前刷新以前的渲染效果。

## 有条件地触发一个效果

我们可以向`useEffect`传递第二个参数，该参数带有一个值数组，要求在值发生变化时运行一个效果。

例如，我们可以使用它在初始渲染时从 API 获取数据，如下所示:

```
function App() {
  const [joke, setJoke] = React.useState({});
  useEffect(() => {
    (async () => {
      const response = await fetch("[https://api.icndb.com/jokes/random](https://api.icndb.com/jokes/random)");
      const res = await response.json();
      console.log(res);
      setJoke(res);
    })();
  }, []);
  return (
    <>
      <p>{joke.value && joke.value.joke}</p>
    </>
  );
}
```

将空数组作为第二个参数传递会阻止它在后续渲染中加载。

我们可以向数组传递一个值，观察数组中的值的变化，然后运行回调函数:

```
function App() {
  const [joke, setJoke] = React.useState({});
  const [id, setId] = React.useState(1);
  useEffect(() => {
    (async () => {
      const response = await fetch(`[https://api.icndb.com/jokes/${id}`](https://api.icndb.com/jokes/${id}`));
      const res = await response.json();
      console.log(res);
      setJoke(res);
    })();
  }, [id]);
  return (
    <>
      <button onClick={() => setId(Math.ceil(Math.random() * 100))}>
        Random Joke
      </button>
      <p>{joke.value && joke.value.joke}</p>
    </>
  );
}
```

在上面的代码中，当我们单击随机笑话按钮时，`setId`被调用，调用的新数字在 1 到 100 之间。然后`id`改变，触发`useEffect`回调运行。

然后用新值设置`joke`，然后新的`joke`显示在屏幕上。

# 结论

`useState`和`useEffect`是最基本的内置 React 钩子。

`useState`让我们通过传入一个返回初始状态的函数或者直接传入初始状态来更新状态。

它返回一个具有当前状态的数组和一个更新状态的函数。返回的状态更新函数接受一个值或一个以以前的值作为参数的函数，如果新值依赖于以前的值，则返回新值。

`useEffect`让我们运行任何副作用、突变或渲染阶段之外的任何东西。

它接受一个回调，其中要运行的代码作为第一个参数，并接受一个数组，其中包含在回调运行之前要监视的更改的值。

如果传入一个空数组，那么回调只在初始渲染时运行。

【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)