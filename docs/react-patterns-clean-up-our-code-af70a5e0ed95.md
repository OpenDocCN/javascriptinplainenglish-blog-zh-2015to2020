# 反应模式—清理我们的代码

> 原文：<https://javascript.plainenglish.io/react-patterns-clean-up-our-code-af70a5e0ed95?source=collection_archive---------6----------------------->

![](img/3633cb722902fb72bbaf81d40f116577.png)

Photo by [Evelyn](https://unsplash.com/@evelynnnnn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将看看如何清理我们的 React 代码。

# JSX

JSX 是一种我们用来编写 React 应用程序的编程语言。

我们应该用它来写任何东西，除了最琐碎的应用程序。

它让我们的生活变得更容易。

它为我们提供了类似 XML 的语法，让我们告诉 React 用熟悉的语法呈现什么。

例如，我们可以写:

```
import React from "react";export default function App() {
  return <p>hello world</p>;
}
```

编写一个“hello world”组件。

将 HTML 与逻辑混合在一起乍一看似乎很奇怪，但是我们需要它来处理复杂性。

这些标签使得表示 DOM 树比用普通的 JavaScript 更容易。

# 巴比伦式的城市

必须安装 Babel 来支持 ES2015。

Create React App 自带 Babel，所以我们不用自己安装任何东西。

我们只是使用`npm start`来运行我们的项目，它会为我们处理所有的传输。

# 小道具

JSX 元素和组件可以有道具，类似于 HTML 中的属性。

不同之处在于，这些值不必是静态的。

例如，我们可以写:

```
import React from "react";const Hello = ({ greeting }) => {
  return <p>{greeting}</p>;
};export default function App() {
  return <Hello greeting="hello world" />;
}
```

我们有一个`Hello`组件，它需要一个`greeting`道具。

然后我们可以从`Hello`的参数中得到它的值。

然后我们在屏幕上看到`hello world`。

# 儿童

我们可以创建子组件来嵌套子组件。

例如，我们可以写:

```
import React from "react";export default function App() {
  return (
    <div>
      <p>hello</p>
    </div>
  );
}
```

我们在 div 元素中放了一个 p 元素。

# 与 HTML 的区别

尽管 JSX 和 HTML 很相似，但是和 HTML 还是有区别的。

JSX 的属性是骆驼案而不是烤肉串案。

还有，HTML 中的`class`属性在 JSX 是`className`，在 JSX`for`HTML 属性是`htmlFor`。

`class`和`for`是 JavaScript 中的保留字，所以我们不能使用它们。

# 风格

HTML 中的`style`属性接受带有样式字符串的文本。

样式属性接受一个对象。

# 根

组件中的 JSX 代码必须有一个根元素。

例如，不返回:

```
<div />
<div />
```

我们必须写下:

```
<div>
 <div />
 <div />
</div>
```

或者:

```
<>
 <div />
 <div />
</>
```

`<>`和`</>`是片段的开始和结束标签，可以用作包装器元素，但不呈现任何内容。

# 间隔

JSX 的空格有点棘手。

如果我们想要空间，我们必须写:

```
{' '}
```

然后我们可以分离内容。

# 布尔属性

JSX 可以有布尔值。

如果我们没有指定值，那么它默认为`true`。

例如，如果我们有:

```
<button disabled />
```

则该按钮将被禁用。

这和 HTML 是一致的。

# 传播属性

spread 操作符对于节省我们输入代码的时间很重要。

例如，我们可以写:

```
const attrs = { id: 'baz' }
return <div {...attrs} />
```

然后我们把`attrs`的属性作为道具传播。

因此，我们将`id`的值作为道具传递，并将`div` 的 ID 设置为`baz`。

# 模板

道具可以取动态值。这就是为什么我们可以用它来做模板。

例如，我们可以通过编写以下内容有条件地禁用按钮:

```
<button disabled={errors.length} />
```

那么我们唯一禁用的一个按钮是`errors`有一个非零长度。

# 模式

React 组件中有一些常见的模式。我们应该遵循它们以正确的方式创建组件。

![](img/d06426c206c44aececb4dfd6237a8748.png)

Photo by [Paweł Czerwiński](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 多线

JSX 优先于`createElement`。这是因为我们可以用它更容易地创建复杂的组件。

如果我们有嵌套元素，我们写:

```
<div>
 <Header />
   <div>
     <Content content={...} />
   </div>
</div>
```

而不是:

```
<div><Header /><div><Content content={...} /></div></div>
```

保持行短可以让我们不必水平滚动来阅读整行。

当我们有多行表达式时，我们应该把括号括起来。

否则，JavaScript 可能会在意想不到的地方插入分号。

# 结论

我们保持多行 JSX 代码简短，并用圆括号括起来。

除了最琐碎的应用程序，应该使用 JSX，这样我们就可以使用它提供的许多快捷方式。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**