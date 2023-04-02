# Top React 挂钩—数组和 Cookies

> 原文：<https://javascript.plainenglish.io/top-react-hooks-arrays-and-cookies-9a4b7df57802?source=collection_archive---------14----------------------->

![](img/e624ee3be48544be3c9853f616017558.png)

Photo by [SJ Baren](https://unsplash.com/@sjcbrn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

它附带的一个挂钩是`useArray`挂钩。

要使用它，我们可以写:

```
import React from "react";
import { useArray } from "react-recipes";export default function App() {
  const { add, clear, removeIndex, removeById, value: currentArray } = useArray(
    ["cat", "dog", "bird"]
  ); return (
    <>
      <button onClick={() => add("chicken")}>Add animal</button>
      <button onClick={() => removeIndex(currentArray.length - 1)}>
        Remove animal
      </button>
      <button onClick={clear}>Clear</button>
      <p>{currentArray.join(", ")}</p>
    </>
  );
}
```

`useArray`钩子取一个数组作为数组的初始值。

它返回一个具有各种属性的对象。

`add`是一个让我们添加项目的功能。

`clear`是清除数组的功能。

`removeIndex`是一个通过索引删除条目的函数。

`removeById`让我们通过项目的`id`属性移除项目。

挂钩让我们以更干净的方式运行异步代码。

我们可以将异步代码分离成它自己的函数。

例如，我们可以写:

```
import React from "react";
import { useAsync } from "react-recipes";const draw = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const rnd = Math.random() * 10;
      rnd <= 5 ? resolve(rnd) : reject("error");
    }, 2000);
  });
};export default function App() {
  const { error, execute, pending, value } = useAsync(draw, false); return (
    <div>
      <button onClick={execute} disabled={pending}>
        {pending ? "Loading..." : "Click Me"}
      </button>
      {value && <div>{value}</div>}
      {error && <div>{error}</div>}
    </div>
  );
}
```

使用钩子。

我们有带`draw`功能的`useAsync`钩子。

`draw`根据`rnd`的值，返回解析为随机值的承诺或拒绝承诺。

钩子的第二个参数是我们是否让钩子立即运行。

钩子返回一个具有各种属性的对象。

`error`是来自被拒绝承诺的字符串。

`execute`是一个函数，用来延迟我们函数的执行。

`pending`是一个布尔值，表示它何时执行。

`value`是执行我们的函数返回的值。

`useCookie`钩子让我们操纵客户端 cookies。

要使用它，我们可以写:

```
import React from "react";
import { useCookie } from "react-recipes";export default function App() {
  const [userToken, setUserToken, deleteUserToken] = useCookie("token", "0"); return (
    <div>
      <p>{userToken}</p>
      <button onClick={() => setUserToken("100")}>Change token</button>
      <button onClick={() => deleteUserToken()}>Delete token</button>
    </div>
  );
}
```

使用`useCookie`挂钩。

钩子的参数是 cookie 的键和初始值。

它返回一个数组，里面有一些我们可以使用的变量。

`userToken`是令牌值。

`setUserToken`是用`token`键设置 cookie 的功能。

`deleteUserToken`用`token`键删除 cookie。

![](img/0faa256590b91680a4852115b1d371b6.png)

Photo by [Mollie Sivaram](https://unsplash.com/@molliesivaram?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

React Recipes 附带了操作数组和 cookies 的钩子。

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**