# React 17 和 JSX 变换:从组件文件中删除一行代码

> 原文：<https://javascript.plainenglish.io/react-17-and-jsx-transform-removing-one-line-of-code-from-your-component-files-fadb2d13545c?source=collection_archive---------12----------------------->

![](img/e6e160302c0d14614458221ca371d0a5.png)

# 你有没有想过为什么写一个功能组件一定要导入 React？

根据您对 React 的经验水平，这个问题可能很明显，或者您可能已经习惯于在所有组件文件的顶部添加`import React from 'react'`。

让我们考虑下面的例子:

```
import React from 'react';export const ImABoringComponent = () => <p> I'm boring! </p>;
```

简单吧？我们创建一个箭头函数，返回我们认为看起来像普通 HTML 的内容。这里不可忽视的最重要的一点是，我们不仅编写了 HTML，实际上还编写了 JSX。

# 太棒了，什么是 JSX，为什么它看起来像 HTML？

在这篇文章中，我们不会详细讨论 JSX 与 HTML 的区别，但简而言之，JSX 是`React.createElement`的语法糖。它允许我们写一些看起来像 HTML 的东西，但是为了被你的浏览器理解和执行，它需要被转换成`React.createElement`。

# JSX 是如何转变为 React.createElement 的？

幸运的是，万事皆有工具，Babel 是最流行的将一种 JavaScript 语言语法编译成另一种语言的工具。你可以在这里阅读更多关于`@babel/react-preset` [的内容](https://babeljs.io/docs/en/#jsx-and-react)。

使用这个预设，Babel 将从我们的示例中输出以下内容:

```
import React from 'react';export const ImABoringComponent = () => React.createElement('p', null, "I'm boring!");
```

# 还不明显吗？

根据上面的片段，如果我们没有导入 React，然后 Babel 继续进行它的 JSX 转换，我们最终会试图调用`React.createElement`而没有引用`React`。

# 太好了，那么 React 17 和 JSX 变换是怎么回事呢？

React 的团队与 babel 合作更新了 Babel 插件`@babel/plugin-transform-react-jsx`。有了这个更新版本，您不再需要手动导入 React，它将自动导入和引用！我们上面的例子将被编译成类似于:

```
// This is what Babel adds
import {jsx as _jsx} from 'react/jsx-runtime';export const ImABoringComponent = () => _jsx('p', null, "I'm boring!");
```

我们省去了为 React 编写额外导入的努力！

非常感谢阅读，我希望这是有帮助的！

参考资料及延伸阅读:
-[https://reactjs.org/docs/react-api.html#createelement](https://reactjs.org/docs/react-api.html#createelement)
-[https://reactjs.org/docs/introducing-jsx.html](https://reactjs.org/docs/introducing-jsx.html)
-[https://babeljs.io/docs/en/#jsx-and-react](https://babeljs.io/docs/en/#jsx-and-react)
-[https://react js . org/blog/2020/09/22/introducing-the-new-jsx-transform . html](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
-[https://babel js . io/blog/2020/03/16/7 . 9 . 0 # a-new-transform](https://babeljs.io/blog/2020/03/16/7.9.0#a-new-jsx-transform-11154httpsgithubcombabelbabelpull11154)