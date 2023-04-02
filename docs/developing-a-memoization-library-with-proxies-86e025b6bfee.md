# 用代理开发记忆库

> 原文：<https://javascript.plainenglish.io/developing-a-memoization-library-with-proxies-86e025b6bfee?source=collection_archive---------15----------------------->

## 代理比较和代理记忆

![](img/868360b91f371a3114c11a01678bad7f.png)

# 介绍

我开始开发[反应-反应-还原](https://github.com/dai-shi/reactive-react-redux)和[反应-跟踪](https://github.com/dai-shi/react-tracked)已经有一段时间了。这些库提供所谓的[状态使用跟踪](https://blog.axlight.com/posts/what-is-state-usage-tracking-a-novel-approach-to-intuitive-and-performant-api-with-react-hooks-and-proxy/)来优化 React 中的渲染。我认为这种方法非常新颖，我已经花了很多精力来提高它的性能。

最近，我想如果它能被更广泛地使用就更好了。我想知道它是否可以用在普通的 JS 中。vanilla JS 中的 API 是什么？如果通俗易懂就好了。我的想法以记忆化告终，主要是因为主要目标是替代[重选](https://github.com/reduxjs/reselect)。

新库被命名为`proxy-memoize`。

# 代理记忆

GitHub:[https://github.com/dai-shi/proxy-memoize](https://github.com/dai-shi/proxy-memoize)

`proxy-memoize`库提供了一个记忆功能。它将接受一个函数并返回一个记忆函数。

```
import memoize from 'proxy-memoize';const fn = (x) => ({ foo: x.foo });
const memoizedFn = memoize(fn);
```

这个图书馆有很多设计选择。要记忆的函数必须是只接受一个对象作为参数的函数。因此，不支持如下功能。

```
const unsupportedFn1 = (number) => number * 2;const unsupportedFn2 = (obj1, obj2) => [obj1.foo, obj2.foo];
```

这将允许用`WeakMap`缓存结果。我们可以缓存尽可能多的结果，并在它们不再有效时让 JS 垃圾收集。

如果我们在`WeakMap`缓存中找不到结果，就使用代理。memoized 函数调用原始函数，参数对象由代理包装。代理在调用函数时跟踪对象属性的使用。跟踪的信息称为“受影响的”，这是原始对象的部分树结构。为了简单起见，我们在这篇文章中使用点符号。

让我们看看下面的例子。

```
const obj = { a: 1, b: { c: 2, d: 3 } };// initially affected is emptyconsole.log(obj.a) // touch "a" property// affected becomes "a"console.log(obj.b.c) // touch "b.c" property// affected becomes "a", "b.c"
```

一旦“受影响的”被创建，如果受影响的属性被改变，它可以检查新的对象。只有当任何受影响的属性被更改时，它才会重新调用该函数。这将允许非常精细调谐的记忆。

让我们看一个例子。

```
const fn = (obj) => obj.arr.map((x) => x.num);
const memoizedFn = memoize(fn);const result1 = memoizedFn({
  arr: [
    { num: 1, text: 'hello' },
    { num: 2, text: 'world' },
  ],
})// affected is "arr[0].num", "arr[1].num" and "arr.length"const result2 = memoizedFn({
  arr: [
    { num: 1, text: 'hello' },
    { num: 2, text: 'proxy' },
  ],
  extraProp: [1, 2, 3],
})// affected properties are not change, hence:
result1 === result2 // is true
```

使用情况跟踪和受影响的比较是由内部库“代理比较”完成的

# 代理比较

GitHub:[https://github.com/dai-shi/proxy-compare](https://github.com/dai-shi/proxy-compare)

这是一个从 react-tracked 中提取的库，只提供与代理的比较特性。(实际上，react-tracked v2 将使用这个库作为依赖项。)

该库导出两个主要函数:`createDeepProxy`和`isDeepChanged`

它的工作原理如下:

```
const state = { a: 1, b: 2 };
const affected = new WeakMap();
const proxy = createDeepProxy(state, affected);
proxy.a // touch a property
isDeepChanged(state, { a: 1, b: 22 }, affected) // is false
isDeepChanged(state, { a: 11, b: 2 }, affected) // is true
```

`state`可以是一个嵌套的对象，只有当一个属性被触摸时，才会创建一个新的代理。值得注意的是`affected`是从外部提供的，这将有助于将它集成到 React 钩子中。

关于性能改进和处理边缘情况还有其他一些要点。在这篇文章中，我们不做过多的描述。

# 与 React 上下文一起使用

正如在[一篇过去的文章](https://blog.axlight.com/posts/4-options-to-prevent-extra-rerenders-with-react-context/)中所讨论的，一种选择是使用 useMemo。如果 proxy-memoize 与 useMemo 一起使用，我们将能够获得类似 react-tracked 的好处。

```
import memoize from 'proxy-memoize';const MyContext = createContext();const Component = () => {
  const [state, dispatch] = useContext(MyContext);
  const render = useMemo(() => memoize(({ firstName, lastName }) => (
    <div>
      First Name: {firstName}
      <input
        value={firstName}
        onChange={(event) => {
          dispatch({ type: 'setFirstName', firstName: event.target.value });
        }}
      (Last Name: {lastName})
      />
    </div>
  )), [dispatch]);
  return render(state);
};const App = ({ children }) => (
  <MyContext.Provider value={useReducer(reducer, initialState)}>
    {children}
  </MyContext.Provider>
);
```

当上下文改变时，`Component`将重新呈现。然而，它返回记忆化的 react 元素树，除非`firstName`没有改变。所以，重新渲染到此为止。这种行为不同于 react-tracked，但应该得到相当的优化。

# 与 React Redux 一起使用

它可以是重新选择的简单替换。

```
import { useDispatch, useSelector } from 'react-redux';
import memoize from 'proxy-memoize';const Component = ({ id }) => {
  const dispatch = useDispatch();
  const selector = useMemo(() => memoize((state) => ({
    firstName: state.users[id].firstName,
    lastName: state.users[id].lastName,
  })), [id]);
  const { firstName, lastName } = useSelector(selector);
  return (
    <div>
      First Name: {firstName}
      <input
        value={firstName}
        onChange={(event) => {
          dispatch({ type: 'setFirstName', firstName: event.target.value });
        }}
      />
      (Last Name: {lastName})
    </div>
  );
};
```

这可能太简单了，无法展示 proxy-memoize 的强大功能，下面是一个有趣的用例。

```
memoize((state) => state.users.map((user) => user.firstName))
```

只有当`users`的长度改变或`firstName`之一改变时，才会重新评估。即使`lastName`被改变，它也一直返回缓存的结果。

# 结束语

启发我开发这个的是 MobX 和 Immer 的关系。我对它们的实现一点都不熟悉，但我感觉 Immer 是 MobX 的一个子集，用于更广泛的用例。我想创造像 Immer 一样的东西。Immer 让您神奇地将可变(写)操作转换为不可变对象。proxy-memoize 让您能够神奇地为不可变对象创建选择器(读取)函数。

*原载于 2020 年 11 月 29 日 https://blog.axlight.com*[](https://blog.axlight.com/posts/developing-a-memoization-library-with-proxies/)**。**