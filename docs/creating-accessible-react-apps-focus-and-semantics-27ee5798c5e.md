# 创建可访问的 React 应用程序——焦点和语义

> 原文：<https://javascript.plainenglish.io/creating-accessible-react-apps-focus-and-semantics-27ee5798c5e?source=collection_archive---------6----------------------->

![](img/21d456bf5a793e318459e0d1396ca5de.png)

Photo by [Brigitte Tohm](https://unsplash.com/@brigittetohm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将看看如何创建可访问的 React 应用程序。

# 为什么是可访问性？

我们应该创建可访问的应用程序，以便每个人都可以使用它们。

# 标准和指南

[网站内容可访问性指南](https://www.w3.org/WAI/intro/wcag)提供了创建可访问网站的指南。

此外，我们还有[Web Accessibility Initiative——Accessible Rich Internet Applications](https://www.w3.org/WAI/intro/aria)文档包含构建完全可访问的 JavaScript 小部件的指南。

JSX 完全支持 HTML 属性。这些属性以烤肉串的形式保存在 HTML 中。

# 语义 HTML

HTML 标签因其含义而得名。这意味着它们对屏幕阅读器和其他解析 web 内容的辅助程序有意义。

这意味着我们不应该使用`div`来分组元素，而是应该使用片段，这样我们的应用程序就不会有额外的`div`元素妨碍屏幕阅读器。

例如，我们可以向组件添加片段，如下所示:

```
function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.meaning}</dd>
    </Fragment>
  );
}class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      items: [
        { term: "coffee", meaning: "black drink" },
        { term: "milk", meaning: "white drink" }
      ]
    };
  } render() {
    return (
      <dl>
        {this.state.items.map(item => (
          <ListItem item={item} key={item.id} />
        ))}
      </dl>
    );
  }
}
```

然后，我们得到以下呈现的 HTML:

```
<dl>
  <dt>coffee</dt>
  <dd>black drink</dd>
  <dt>milk</dt>
  <dd>white drink</dd>
</dl>
```

正如我们所看到的，片段不会产生任何额外的 HTML，而是让我们将元素组合在一起。

一个简写或片段是`<>`和`</>`。因此，我们可以将上面的代码重写如下:

```
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.meaning}</dd>
    </>
  );
}
```

我们保持代码的其余部分不变。

然后我们得到相同的 HTML 渲染。

# 可访问的表单

我们应该给表单贴上标签，为所有用户提供可访问性。

为此，我们向`label`元素添加一个`for`属性。在 React 中，由`htmlFor`属性表示的`for`元素。

这适用于所有表单控件。

例如，我们可以做以下操作来添加一个`htmlFor`道具:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { name: "" };
  } render() {
    return (
      <>
        <label htmlFor="name">Name:</label>
        <input type="text" name="name" value={this.state.name} />
      </>
    );
  }
}
```

在上面的代码中，我们有:

```
htmlFor="name"
```

将标签与表单域相关联。`htmlFor`值应该与表单控件元素的`name`值相同。

完整的可访问性指南位于:

*   [W3C 向我们展示了如何标记元素](https://www.w3.org/WAI/tutorials/forms/labels/)
*   [WebAIM 向我们展示了如何标记元素](https://webaim.org/techniques/forms/controls)
*   [paci ello 集团解释了可访问名称](https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)

# 向用户通知错误

用户需要理解错误。以下指南告诉我们如何向屏幕阅读器显示错误文本:

*   [W3C 演示用户通知](https://www.w3.org/WAI/tutorials/forms/notifications/)
*   [WebAIM 查看表单验证](https://webaim.org/techniques/formvalidation/)

![](img/8dd3da13170308e5eef6812cd526e3f1.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 聚焦控制

我们应该只通过使用 CSS 来改变或删除轮廓，这样用户即使看不到轮廓也可以获得聚焦的元素。

# 跳到所需内容的方法

此外，我们应该为用户提供跳转到他们想看或想用的内容的方法。

我们可以用内部页面锚和一些样式来实现。

我们还可以将重要的内容放在像`main`和`aside`这样的元素中，这样它们就可以与其他内容区分开来。

# 以编程方式管理焦点

在 React 中，我们可以通过使用 refs 来访问 DOM 元素。这意味着我们可以通过调用 DOM 方法来控制元素何时获得焦点。

例如，如果我们想在组件安装在 DOM 树中时聚焦一个元素，我们可以如下调用`focus`:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  } componentDidMount() {
    this.textInput.current.focus();
  } render() {
    return <input type="text" ref={this.textInput} />;
  }
}
```

在上面的代码中，我们通过编写以下代码创建了一个`ref`来访问 DOM 元素:

```
this.textInput = React.createRef();
```

在`input`元素中，我们有:

```
ref={this.textInput}
```

将`input`元素与我们创建的`ref`元素关联起来。

然后在`componentDidMount`钩子中，我们有:

```
this.textInput.current.focus();
```

在组件安装时聚焦输入。`this.textInput.current`返回 input 元素，这样我们就可以在上面调用原生的`focus`方法。

# 结论

在创建 React 应用时，我们应该考虑可访问性。

这意味着我们应该使用语义 HTML 标签，以便所有用户都能理解一个页面的内容。

此外，我们应该为用户提供跳转到他们想看的内容的方法。

最后，我们可以通过编程来聚焦元素，这样当焦点丢失时，用户可以通过重新获得焦点来立即使用输入。