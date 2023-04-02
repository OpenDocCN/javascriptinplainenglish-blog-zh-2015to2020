# 使用片段将 React 组件分组在一起

> 原文：<https://javascript.plainenglish.io/using-fragments-to-group-react-components-f30bfd515665?source=collection_archive---------3----------------------->

![](img/1664162ad18a47e8a4b6e5aa38612059.png)

Photo by [Victoria Strukovskaya](https://unsplash.com/@struvictoryart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将了解如何使用片段将组件分组在一起，而不使用 HTML 元素。

# 碎片

我们需要片段将组件组合在一起，而不需要在 DOM 中添加额外的包装元素。

例如，不要写:

```
class App extends React.Component {
  render() {
    return (
      <div>
        <p>foo</p>
        <p>bar</p>
      </div>
    );
  }
}
```

我们可以改为写:

```
class App extends React.Component {
  render() {
    return (
      <React.Fragment>
        <p>foo</p>
        <p>bar</p>
      </React.Fragment>
    );
  }
}
```

第一个和第二个示例的区别在于，第一个示例将呈现:

```
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

而第二个示例将呈现:

```
<p>foo</p>
<p>bar</p>
```

这对于呈现包含多组元素(如表格)的元素也很有帮助。表格具有必须成组添加的`td`和`th`元素。

例如，我们可以将`th`元素移动到一个组件中，如下所示:

```
class Heading extends React.Component {
  render() {
    return (
      <React.Fragment>
        {["foo", "bar"].map((th, index) => (
          <th key={index}>{th}</th>
        ))}
      </React.Fragment>
    );
  }
}class App extends React.Component {
  render() {
    return (
      <table>
        <thead>
          <tr>
            <Heading />
          </tr>
        </thead>
      </table>
    );
  }
}
```

这将只呈现`tr`中的`th`元素，所以它将是有效的 HTML。如果我们将`th`元素与其他元素组合在一起，它将成为无效的 HTML，因为它们不应该被任何额外的东西包裹。

# 短语法

还有一个片段的简写。我们可以写`<>`和`</>`而不是写`<React.Fragment>`和`</React.Fragment>`。

所以我们也可以写:

```
class App extends React.Component {
  render() {
    return (
      <>
        <p>foo</p>
        <p>bar</p>
      </>
    );
  }
}
```

我们可以在任何其他元素上使用短语法，除了它不支持键或属性。

该语法不支持键或任何其他属性。

![](img/23c7e474aaae5f024b91895b8729b8bc.png)

Photo by [Florian Berger](https://unsplash.com/@bergerteam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 键控片段

用`<React.Fragment>`语法声明的片段可能有键。这对于将集合映射到片段数组非常有用。

例如，我们可以如下使用它:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      glossary: [
        { id: 1, word: "coffee", meaning: "black drink" },
        { id: 2, word: "milk", meaning: "white drink" }
      ]
    };
  } render() {
    return (
      <div>
        {this.state.glossary.map(g => (
          <React.Fragment key={g.id.toString()}>
            <dt>{g.word}</dt>
            <dd>{g.meaning}</dd>
          </React.Fragment>
        ))}
      </div>
    );
  }
}
```

在上面的代码中，我们在`React.Fragment`组件中使用了`key`属性，这样 React 就可以从一组片段中识别出唯一的元素。

然后我们得到一个内部只有 dt 和 dd 元素的 div。

`key`属性是唯一可以传递给`Fragment`的属性。将来可能会支持其他道具。

# 结论

我们可以使用片段将元素分组在一起，而无需在 DOM 中呈现额外的包装元素。

为此，我们可以使用`React.Fragment`组件或简写的`<></>`语法。

如果我们将数组映射到片段，那么我们必须将一个`key`属性放到`React.Fragment`组件中，这样 React 就可以唯一地标识映射的元素。