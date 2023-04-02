# 在 React 组件中添加多个类名

> 原文：<https://javascript.plainenglish.io/adding-multiple-class-names-in-react-components-ec43bc5f94fa?source=collection_archive---------3----------------------->

![](img/59a16f83f739ca2d65f861f06e458df7.png)

Photo by [Sam Balye](https://unsplash.com/@sbk202?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是构建现代交互式前端 web 应用最常用的前端库。它还可以用来构建移动应用程序。

在本文中，我们将了解如何向 React 组件添加多个类名。

# 类名字符串

默认情况下，React 只允许在`className`属性中使用字符串。然而，我们可以很容易地用模板字符串创建动态字符串。

例如，我们可以创建一个具有动态类名的组件，如下所示:

`styles.css`:

```
.red {
  color: red;
}.lightblue {
  background-color: lightblue;
}
```

`App.js`:

```
import React from "react";
import "./styles.css";export default function App() {
  const [classNames, setClassNames] = React.useState(``);
  const [showRed, setShowRed] = React.useState(false);
  const [showBlue, setShowBlue] = React.useState(false);
  React.useEffect(() => {
    setClassNames(`${showRed ? "red" : ""} ${showBlue ? "lightblue" : ""}`);
  }, [showRed, showBlue]);
  return (
    <div>
      <button onClick={() => setShowRed(showRed => !showRed)}>
        Toggle Red
      </button>
      <button onClick={() => setShowBlue(showBlue => !showBlue)}>
        Toggle Light Blue
      </button>
      <div className={classNames}>foo</div>
    </div>
  );
}
```

在上面的代码中，我们用`showRed`和`showBlue`布尔状态分别打开和关闭`red`和`blue`类名。

我们使用`useEffect`钩子来观察`showRed`和`showBlue`值的变化，然后在`useEffect`回调中调用`setClassNames`来分别设置类名。

然后，当我们单击切换红色和切换浅蓝色按钮时，我们将看到文本和背景颜色的变化。

# 类名包

使用字符串对一些类来说是好的，但是如果我们有更多的类，它就变得复杂了。

我们可以使用`classnames`包使添加多个类变得更容易。它对静态和动态类都有效。

要使用它，我们首先通过运行以下命令来安装该软件包:

```
npm install classnames
```

那么我们可以如下使用它:

```
import React from "react";
import "./styles.css";
const classNames = require("classnames");export default function App() {
  const [showRed, setShowRed] = React.useState(false);
  const [showBlue, setShowBlue] = React.useState(false); return (
    <div>
      <button onClick={() => setShowRed(showRed => !showRed)}>
        Toggle Red
      </button>
      <button onClick={() => setShowBlue(showBlue => !showBlue)}>
        Toggle Light Blue
      </button>
      <div className={classNames({ red: showRed, lightblue: showBlue })}>
        foo
      </div>
    </div>
  );
}
```

在上面的代码中，我们重写了上面的例子来使用`classnames`包。从中，我们导入了`classNames`函数，在这里我们传入了以下对象:

```
{ red: showRed, lightblue: showBlue }
```

上面的代码具有相同的代码或者切换状态，但是`classNames`调用返回了我们想要包含的所有类的类名字符串，所以我们将它直接传递给`className` prop。

因此，我们应该得到和以前一样的结果，但是代码更少。

如果我们想添加静态类，我们可以这样写:

```
classNames('red', 'lightblue');
```

那么总是会返回两个类名。

我们还可以混合静态和动态类名:

```
classNames('red', { 'lightblue': showBlue });
```

然后`red`返回所有路，只有`showBlue`为`true`时才会包含`lightblue`。

我们还可以添加阵列:

```
classNames(['red', 'lightblue']);
```

Falsy 值如 0、`false`、`null`、`undefined`或空字符串被忽略。

## ES2015 的动态类名

我们可以使用 ES2015 添加动态类名，因为它支持计算属性名。

例如，我们可以如下使用它:

`styles.css`:

```
.btn-foo,
.btn-bar,
.btn-baz {
  color: red;
}
```

`App.js`:

```
import React from "react";
import "./styles.css";
const classNames = require("classnames");export default function App() {
  const [showRed, setShowRed] = React.useState(false);
  return (
    <div>
      {["foo", "bar", "baz"].map(buttonType => (
        <button
          className={classNames({ [`btn-${buttonType}`]: showRed })}
          onClick={() => setShowRed(showRed => !showRed)}
        >
          Button {buttonType}
        </button>
      ))}
    </div>
  );
}
```

在上面的代码中，我们有 3 个带有动态类名的按钮，都带有按钮前缀。

在`styles.css`中，我们为按钮添加类名，并将颜色设置为红色。

然后在`onClick`处理程序中，我们点击按钮时切换`showRed`。

因此，当我们单击任何按钮时，我们会看到红色被打开和关闭。

![](img/1d6e828e6647ea4946cac3a84cefb4f7.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`classNames`包来创建动态的对象和数组的类名，这样我们就不用自己动手了。

对于简单的场景，我们也可以使用动态模板字符串来打开和关闭类。

## **来自简明英语团队的通知**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****