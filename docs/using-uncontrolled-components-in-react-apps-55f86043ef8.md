# 在 React 应用中使用不受控制的组件

> 原文：<https://javascript.plainenglish.io/using-uncontrolled-components-in-react-apps-55f86043ef8?source=collection_archive---------6----------------------->

![](img/80a37b4b6f2861d3c110a71cebec813f.png)

Photo by [Magnus Engø](https://unsplash.com/@genuinemex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将了解如何在 React 代码中使用不受控制的组件。

# 不受控制的组件

不受控制的组件是我们使用 DOM 属性来操作表单输入的地方。

我们可以使用`ref`直接从 DOM 中获取表单值。

例如，我们可以这样写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.input = React.createRef();
  } handleSubmit(event) {
    alert(`Name submitted: ${this.input.current.value}`);
    event.preventDefault();
  } render() {
    return (
      <form onSubmit={this.handleSubmit.bind(this)}>
        <label>
          Name:
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

在上面的代码中，我们在构造函数中创建了`this.input` ref，并将其附加到 input 元素上。

然后，当我们输入一些内容并点击提交时，我们会得到我们在输入框中输入的内容，显示在警告框中，因为我们是用`this.input.current.value`访问它的。

这使得集成 React 和非 React 代码更加容易，因为我们使用 DOM 来获取值。

# 默认值

表单元素上的`value`属性将覆盖 DOM 中的值。

对于不受控制的组件，我们通常希望指定初始值，而让后续更新不受控制。

我们可以设置`defaultValue`属性来做到这一点:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.input = React.createRef();
  } handleSubmit(event) {
    alert(`Name submitted: ${this.input.current.value}`);
    event.preventDefault();
  } render() {
    return (
      <form onSubmit={this.handleSubmit.bind(this)}>
        <label>
          Name:
          <input type="text" defaultValue="Foo" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

在上面的代码中，我们将`defaultValue`属性设置为`Foo`，如果不改变输入值，这就是`this.input.current.value`的值。

![](img/9b212540da1277aca35b5bc93cb12586.png)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 文件输入标签

文件输入总是不受控制的组件，因为它的值只能由用户设置，而不能以编程方式设置。

因此，我们必须使用 ref 来访问它的值。

例如，我们可以这样写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.fileRef = React.createRef();
  } handleSubmit(event) {
    alert(`Filename: ${this.fileRef.current.files[0].name}`);
    event.preventDefault();
  } render() {
    return (
      <form onSubmit={this.handleSubmit.bind(this)}>
        <input type="file" ref={this.fileRef} />
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

在上面的代码中，我们创建了一个名为`fileRef`的 ref，并将其附加到文件输入中。然后，我们得到具有所选文件名称的`files[0].name`属性。

我们可以使用文件 API 来处理选定的文件。

# 结论

我们可以使用不受控制的组件直接从 DOM 中获取输入值。

这对于集成 React 和非 React 代码很有用。它对快速和肮脏的应用程序也很有用。

我们可以通过给`defaultValue`属性设置一个值来设置非受控输入的默认值。

此外，文件输入总是不受控制的。我们使用文件 API 来操作从文件输入中选择的文件。

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。