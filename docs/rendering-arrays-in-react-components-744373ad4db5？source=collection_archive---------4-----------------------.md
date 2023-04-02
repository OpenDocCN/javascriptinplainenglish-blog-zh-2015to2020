# 在 React 组件中呈现数组

> 原文：<https://javascript.plainenglish.io/rendering-arrays-in-react-components-744373ad4db5?source=collection_archive---------4----------------------->

![](img/717de80c413b4309f0d635e9af535d14.png)

Photo by [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将研究如何在 React 组件中呈现列表。

# 列表和键

我们可以通过调用数组的`map`方法将列表转换成 HTML。

例如，如果我们想将一组数字显示为一个列表，我们可以这样写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
  } render() {
    return (
      <div>
        {[1, 2, 3, 4, 5].map(num => (
          <span>{num}</span>
        ))}
      </div>
    );
  }
}
```

在上面的代码中，我们在`[1, 2, 3, 4, 5]`数组上调用了`map`。在`map`方法的回调中，我们返回了一个包含数字的`span`，我们对每个元素都做了同样的处理。

然后我们得到:

```
12345
```

显示在屏幕上。

我们可以将它赋给一个变量，然后将其传递给`ReactDOM.render`方法，如下所示:

```
import React from "react";
import ReactDOM from "react-dom";const nums = [1, 2, 3, 4, 5].map(num => <span>{num}</span>);const rootElement = document.getElementById("root");
ReactDOM.render(nums, rootElement);
```

我们会展示同样的商品。

同样，我们可以在`render`方法中使用相同的代码:

```
import React from "react";
import ReactDOM from "react-dom";class App extends React.Component {
  constructor(props) {
    super(props);
  } render() {
    const nums = [1, 2, 3, 4, 5].map(num => <span>{num}</span>);
    return <div>{nums}</div>;
  }
}const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

# 键

当我们呈现列表时，我们应该为每个呈现的元素提供一个`key` prop 值，以便 React 可以识别哪些项目已经更改、添加或删除。

它给元素一个稳定的身份。我们应该通过使用一个字符串来选择一个键，这个字符串在它的兄弟列表中唯一地标识一个列表项。

键应该是字符串值。

例如，如果我们想要呈现待办事项列表，我们应该选择`id`字段作为`key`的值，如下所示:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      todos: [
        { id: 1, text: "eat" },
        { id: 2, text: "drink" },
        { id: 3, text: "sleep" }
      ]
    };
  } render() {
    return (
      <div>
        {this.state.todos.map(todo => (
          <p key={todo.id.toString()}>{todo.text}</p>
        ))}
      </div>
    );
  }
}
```

在上面的代码中，我们使用了`key={todo.id.toString()}`属性将`key`设置为被转换为字符串的`todo`的`id`。

如果我们的项目没有稳定的身份，我们可以使用索引作为最后的手段:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      todos: [{ text: "eat" }, { text: "drink" }, { text: "sleep" }]
    };
  } render() {
    return (
      <div>
        {this.state.todos.map((todo, index) => (
          <p key={index.toString()}>{todo.text}</p>
        ))}
      </div>
    );
  }
}
```

`index`始终可用，并且对于每个数组元素都是唯一的，因此它可以用作`key`属性的值。

# 用键提取组件

如果我们渲染组件，我们应该将`key`道具放在组件中，而不是被渲染的元素中。

例如，以下是对`key`道具的不正确使用:

```
function TodoItem({ todo }) {
  return <p key={todo.id.toString()}>{todo.text}</p>;
}class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      todos: [
        { id: 1, text: "eat" },
        { id: 2, text: "drink" },
        { id: 3, text: "sleep" }
      ]
    };
  } render() {
    return (
      <div>
        {this.state.todos.map(todo => (
          <TodoItem todo={todo} />
        ))}
      </div>
    );
  }
}
```

在`TodoItem`组件中，我们有:

```
<p key={todo.id.toString()}>{todo.text}</p>;
```

用`key`道具。我们不希望它在那里，因为我们不需要识别一个独特的`li`，因为它已经与外界隔离了。相反，我们想确定一个独特的`TodoItem`。

相反，我们应该写:

```
function TodoItem({ todo }) {
  return <p>{todo.text}</p>;
}class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      todos: [
        { id: 1, text: "eat" },
        { id: 2, text: "drink" },
        { id: 3, text: "sleep" }
      ]
    };
  } render() {
    return (
      <div>
        {this.state.todos.map(todo => (
          <TodoItem todo={todo} key={todo.id.toString()} />
        ))}
      </div>
    );
  }
}
```

我们应该在通过`map`的回调返回的项目中添加键。

![](img/fb379c14484ba3f757274201ee2d0096.png)

Photo by [Zbynek Burival](https://unsplash.com/@zburival?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 键只需要在同级中是唯一的

键只需要在同级元素中是唯一的。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      posts: [
        { id: 1, text: "eat" },
        { id: 2, text: "drink" },
        { id: 3, text: "sleep" }
      ],
      comments: [
        { id: 1, text: "eat" },
        { id: 2, text: "drink" },
        { id: 3, text: "sleep" }
      ]
    };
  } render() {
    return (
      <div>
        <div>
          {this.state.posts.map(post => (
            <p key={post.id.toString()}>{post.text}</p>
          ))}
        </div>
        <div>
          {this.state.comments.map(comment => (
            <p key={comment.id.toString()}>{comment.text}</p>
          ))}
        </div>
      </div>
    );
  }
```

由于`posts`和`comments`不在同一个`div`中渲染，因此`key`值可能会重叠。

`key`道具不会传递给组件。如果我们在一个组件中需要相同的值，我们必须使用不同的名称。

# 在 JSX 嵌入地图()

我们可以在花括号之间嵌入直接调用`map`的表达式。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
  } render() {
    return (
      <div>
        {[1, 2, 3, 4, 5].map(num => (
          <span key={num.toString()}>{num}</span>
        ))}
      </div>
    );
  }
}
```

# 结论

我们可以使用 JavaScript array 的`map`方法将值映射到 React 元素或组件。

为了帮助 React 识别唯一的元素，我们应该为呈现的每个条目向`key` prop 传递一个唯一的字符串。

它应该被传递给由`map`的回调返回的任何东西，因为它们是必须由 React 唯一标识的项目。

此外，键在直接兄弟中必须是唯一的。其他地方可以有相同的键值。