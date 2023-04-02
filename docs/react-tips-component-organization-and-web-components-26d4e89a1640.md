# React 提示—组件组织和 Web 组件

> 原文：<https://javascript.plainenglish.io/react-tips-component-organization-and-web-components-26d4e89a1640?source=collection_archive---------6----------------------->

![](img/f7a9101613ead55a0e612e9f5be60837.png)

Photo by [Marek Szturc](https://unsplash.com/@marxgall?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是构建现代交互式前端 web 应用最常用的前端库。它还可以用来构建移动应用程序。在本文中，我们将了解一些让 React 应用程序构建更容易的技巧和诀窍。

# 保持元件小

保持组件小，使其易于阅读、更改和测试。它们也更容易调试和维护。30 多行代码，可能太大了。

如果组件是父组件和子组件，通过在它们之间传递道具，我们可以很容易地将它们分成更小的组件。

如果它们不相关，我们也可以使用像 Redux 或上下文 API 这样的状态管理解决方案。

它们做得更少，因此更有可能被重用，因为它们适合更多的用例。

例如，下面是一个小组件:

```
import React from "react";export default function App() {
  const [count, setCount] = React.useState(0);
  return (
    <div className="App">
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <p>{count}</p>
    </div>
  );
}
```

它很短，自己做不了多少事。

# 避免过小的组件

组件太小也是一个问题。我们不想要很多只有一两行的元件。此外，我们不希望每个 div、span 或段落都是自己的组件。

我们应该让他们接受道具，让他们可以重复使用。例如，我们不应该在应用程序中到处都有这样的组件:

```
const Foo = <p>foo</p>;
```

# 在 React 中使用 Web 组件

我们可以将 web 组件直接放入 React 组件并使用它们。

例如，我们可以定义一个 web 组件，然后按如下方式使用它:

```
import React from "react";class FooParagraph extends HTMLElement {
  constructor() {
    super();
  } connectedCallback() {
    const shadow = this.attachShadow({ mode: "open" });
    const p = document.createElement("p");
    p.setAttribute("class", "wrapper");
    p.textContent = this.getAttribute("text");
    shadow.appendChild(p);
  }
}customElements.define("foo-paragraph", FooParagraph);export default function App() {
  return (
    <div className="App">
      <foo-paragraph text="abc" />
    </div>
  );
}
```

在上面的代码中，我们有了`FooParagraph` web 组件类。在类内部，我们有`connectedCallback`，它获取`text`的属性值，然后将带有`text`值的 p 标签添加到影子 DOM 中。

然后我们调用`customElements.define`来定义一个新的 web 组件。然后我们将它直接放入`App` React 组件。

![](img/60bc87b0bed2084c0d93bbd5bde64814.png)

Photo by [Lydia Torrey](https://unsplash.com/@soulsaperture?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在我们的 Web 组件中使用 React

我们还可以使用 React 创建 web 组件，方法是将 React 组件安装在 web 组件中，如下所示:

`src/index.js`:

```
import React from "react";
import ReactDOM from "react-dom";class XParagraph extends HTMLElement {
  connectedCallback() {
    const mountPoint = document.createElement("div");
    this.attachShadow({ mode: "open" }).appendChild(mountPoint); const text = this.getAttribute("text");
    ReactDOM.render(<p>{text}</p>, mountPoint);
  }
}
customElements.define("x-paragraph", XParagraph);
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
  </head> <body>
    <div id="app"></div>
    <x-paragraph text="foo"></x-paragraph>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码是用 Parcel 创建的项目的一部分。因此，我们可以在脚本中使用模块。

我们有一个`XParagraph` web 组件，它调用`ReactDOM.render`来呈现一个 p React 元素，其`text`属性值取自 web 组件的属性。

然后我们用`customElements.define`定义 web 组件，将元素的名称作为第一个参数，将`HTMLElement`类作为第二个参数。

在`index.html`中，我们添加了`x-paragraph` web 组件，其`text`属性被设置为`foo`，因此我们通过调用`this.getAttribute('text')`然后将返回值传递给`ReactDOM.render`来显示元素内部的文本内容。

因此，我们在屏幕上看到‘foo’这个词。

# 结论

为了使开发、测试和阅读代码变得容易，我们应该保持组件小。大约 30 行或更少是一个好的大小。

然而，它们不应该太小，比如 1 或 2 行，因为我们必须管理太多的行。如果他们不拿任何道具，那就更糟了。为了确保它们是可重用的，我们应该确保如果它们共享任何数据，它们都需要道具。

React 组件可以嵌入到 web 组件中，以便使用 React 创建易于重用的 web 组件。

此外，我们可以在 React 组件中嵌入 web 组件，这样我们就可以利用符合标准的定制 HTML 元素，我们可以在任何地方使用这些元素。

## **来自简明英语团队的通知**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****