# 反应提示—条件句

> 原文：<https://javascript.plainenglish.io/react-tips-conditionals-db8cf3c2cdda?source=collection_archive---------14----------------------->

![](img/46ef28efbb626c98cbb1f60ef191d6fc.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是创建前端应用程序最流行的库之一。它还可以用来创建带有 React Native 的移动应用程序。

在本文中，我们将看看如何以各种方式有条件地显示项目。

# 有条件地显示项目

有几种方法可以使用 React 有条件地显示项目。我们可以使用像`if`和`switch`这样的条件语句。如果我们只是在两个项目之间切换，那么我们也可以使用三元表达式。

要使用三元表达式，我们可以编写以下代码来实现:

```
import React from "react";export default function App() {
  const [on, setOn] = React.useState(true);
  return (
    <div className="App">
      <button onClick={() => setOn(on => !on)}>toggle</button>
      {on ? <p>on</p> : <p>off</p>}
    </div>
  );
}
```

在上面的代码中，我们有一个切换按钮，当点击它时，在`true`和`false`之间切换`on`状态。

如果`on`是`true`，我们渲染‘开’。否则，我们渲染‘关’。下面是我们用来做这件事的三元表达式:

```
{on ? <p>on</p> : <p>off</p>}
```

这很棒，因为我们不必写`if`语句来做同样的事情，这要长得多。

它也是一个表达式，这意味着我们可以将它嵌入到我们的 React 代码中，而不像`if`或`switch`语句那样创建函数。

因此，如果我们只根据某种条件显示一件事或另一件事，这比使用条件语句要好得多。

如果`on`是`false`，我们可以用更短的方式来写，如果我们不显示任何东西。

在这种情况下，我们可以只使用`&&`操作符，因为它评估两个操作数，看它们是否都是真的。如果第一个操作数是 falsy，那么它将只返回`false`而不是计算第二个操作数。

例如，我们可以如下使用它:

```
import React from "react";export default function App() {
  const [on, setOn] = React.useState(true);
  return (
    <div className="App">
      <button onClick={() => setOn(on => !on)}>toggle</button>
      {on && <p>on</p>}
    </div>
  );
}
```

在上面的代码中，如果`on`是`true`，那么它将计算第二个操作数，即`<p>on</p>`。否则，它只会返回`false`。

所以当`on`为`true`时，那么`<p>on</p>`就会被渲染。

如果我们还有更长的，那么我们需要写出完整的条件语句。我们应该在它自己的功能或组件中这样做。

例如，如果我们基于多于 2 个案例显示更多的东西，那么我们需要像下面的代码那样写出它们:

```
import React from "react";const Fruit = ({ val }) => {
  if (val === 0) {
    return <p>apple</p>;
  } else if (val === 1) {
    return <p>orange</p>;
  } else if (val === 2) {
    return <p>grape</p>;
  }
};export default function App() {
  const [num, setNum] = React.useState(0);
  return (
    <div className="App">
      <button onClick={() => setNum(num => (num + 1) % 3)}>rotate</button>
      <Fruit val={num} />
    </div>
  );
}
```

在上面的代码中，我们有一个`Fruit`组件，它是一个无状态的组件，根据传入的`val`属性的值，返回水果的名称并在 p 元素中显示。

然后在我们的`App`组件中，我们有一个按钮，它通过调用`setNum`在数字之间旋转，通过在 0、1 和 2 之间旋转来更新数字。

因此，当我们单击旋转按钮时，我们会看到水果的名称在“苹果”、“桔子”和“葡萄”之间旋转。

同样，我们可以对`switch`语句做同样的事情，如下所示:

```
import React from "react";const Fruit = ({ val }) => {
  switch (val) {
    case 0: {
      return <p>apple</p>;
    }
    case 1: {
      return <p>orange</p>;
    }
    case 2: {
      return <p>grape</p>;
    }
    default: {
      return undefined;
    }
  }
};export default function App() {
  const [num, setNum] = React.useState(0);
  return (
    <div className="App">
      <button onClick={() => setNum(num => (num + 1) % 3)}>rotate</button>
      <Fruit val={num} />
    </div>
  );
}
```

上面的代码更长，因为我们应该放置一个`default` case，不管它是否被使用，以防我们遇到一些意外的值。

此外，我们使用了`case`块而不是`case`语句，这样如果需要，我们可以在每个块中用相同的名称定义块范围的变量，所以随时使用它是一个好习惯。

最后，我们得到和以前一样的结果。

如果我们的组件中有`if`和`switch`语句，那么最好把它们放在自己的组件中，因为它们很长。

![](img/8319f1440e4f3fc05ba399789fe3d8c8.png)

Photo by [Kyaw Tun](https://unsplash.com/@kyawthutun?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 React 组件中有很多方法可以有条件地显示事物。如果有两种情况，我们可以使用三元表达式。如果我们想有条件地显示一件事，我们可以使用`&&`操作符。

否则，如果需要的话，我们应该将`if`和`switch`语句放在它们自己的组件中。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****