# 我们应该学会掌握 React 的 JavaScript 概念——模块和承诺

> 原文：<https://javascript.plainenglish.io/javascript-concepts-we-should-learn-to-master-react-modules-and-promises-99a38842771?source=collection_archive---------6----------------------->

![](img/2d2bde813c3430c709524c220efb095d.png)

Photo by [Caleb Martin](https://unsplash.com/@cmart10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

了解最新的 JavaScript 语法对于理解和掌握 React 至关重要。

阅读手册要容易得多，编写 React 代码也要短得多、干净得多。

一旦我们掌握了一些新的 JavaScript 语法，编写 React 代码将变得轻而易举。

在本文中，我们将了解 JavaScript 模块在 React 应用程序中的使用，以及写承诺的更简短方式。

# 模块

ES6 模块是任何现代 JavaScript 应用程序的关键部分。他们让我们封装代码，只暴露我们想暴露的代码。我们不需要对功能做任何改动，也不需要使用第三方模块系统来做同样的事情。

在 JavaScript 模块中，有默认的导出，它导出没有名字的条目，我们可以自己设置名字。

此外，还有命名的导出。但是，我们可以在用`as`关键字导入时更改名称。

例如，模块的一个基本用途如下:

`App.js`

```
import React from "react";
const App = () => {
  return <div>hello</div>;
};
export default App;
```

`main.js`

```
import React from "react";
import ReactDOM from "react-dom";import App from "./App";const rootElement = document.getElementById("root");
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  rootElement
);
```

在上面的代码中，我们有:

```
export default App;
```

导出没有名称的`App`组件。然后我们通过编写来导入它:

```
import App from "./App";
```

在`main.js`。

我们可以用我们喜欢的任何名称导入它，因为它是默认的导出。我们不能有一个以上的默认导出。

大多数库，包括 React 本身，也封装在 JavaScript 模块中。

以上示例包括以下导入:

```
import React from "react";
import ReactDOM from "react-dom";
```

这些导入也是 JavaScript 模块的默认导出，因为在关键字`import`后面没有括号。

要创建具有命名导出的模块，我们可以编写以下代码:

`Foo.js`

```
import React from "react";
const Foo = () => {
  return <div>foo</div>;
};
export { Foo };
```

`App.js`:

```
import React from "react";
import { Foo } from "./Foo";
const App = () => {
  return (
    <div>
      <Foo />
    </div>
  );
};
export default App;
```

在上面的代码中，我们有将`Foo`组件导出为命名导出的`Foo.js`模块。

然后在`App.js`中，我们通过编写以下内容将`Foo`组件作为命名导入导入:

```
import { Foo } from "./Foo";
```

我们也可以用关键字`as`重命名命名导入，如下所示:

```
import React from "react";
import { Foo as FooComponent } from "./Foo";
const App = () => {
  return (
    <div>
      <FooComponent />
    </div>
  );
};
export default App;
```

在上面的代码中，我们用`as`关键字将`Foo`组件重命名为`FooComponent`，并在`App`中引用它来呈现它。

导入和导出是静态分析的，因此文本编辑器和 ide 可以检查我们是否正确地导入和导出了它。

这是使用模块的另一个优点。这些好处对几乎每个人都有用，因为几乎每个人都在使用 JavaScript 模块。

![](img/d478736b6454d782625f1c197edd3d02.png)

Photo by [Javier Mazzeo](https://unsplash.com/@javier365?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 异步和等待

`async`和`await`是承诺和给`then`回电的简写。

它让我们更容易地链接回调，看起来也更干净。

例如，我们可以如下使用它:

```
import React, { useEffect, useState } from "react";const App = () => {
  const [{ name, age, count }, setNameData] = useState({}); const getData = async () => {
    const res = await fetch("https://api.agify.io/?name=michael");
    const data = await res.json();
    setNameData(data);
  }; useEffect(() => {
    getData();
  }, []); return (
    <div>
      {name} {age} {count}
    </div>
  );
};
export default App;
```

在上面的代码中，我们有`getData`函数，它是一个`async`函数。它使用`async`和`await`语法来链接承诺。

`fetch`返回一个承诺，所以我们对它使用`await`来获得解析的值。同样，我们对`res.json()`做了同样的事情，它也返回一个承诺。

这与以下内容相同:

```
import React, { useEffect, useState } from "react";const App = () => {
  const [{ name, age, count }, setNameData] = useState({}); const getData = () => {
    fetch("https://api.agify.io/?name=michael")
      .then(res => res.json())
      .then(data => setNameData(data));
  }; useEffect(() => {
    getData();
  }, []); return (
    <div>
      {name} {age} {count}
    </div>
  );
};
export default App;
```

在上面的代码中，我们有:

```
fetch("https://api.agify.io/?name=michael")
      .then(res => res.json())
      .then(data => setNameData(data));
```

这与我们在`async`和`await`语法中的用法相同，除了我们必须在每个`then`回调中返回一个承诺以继续执行下一个承诺。

使用`async`和`await`语法，我们不需要处理回调，这很好。

# 结论

如果我们构建 React 应用程序，将代码划分成模块是使代码干净的必要条件。此外，几乎所有的库都可以作为 JavaScript 模块获得，所以如果我们想添加任何库，就必须使用它们。

`async`和`await`语法是写承诺的一种更短的方式，它让我们消除了频繁使用回调，如果我们使用旧的`then`方法，我们必须添加回调。

# **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**