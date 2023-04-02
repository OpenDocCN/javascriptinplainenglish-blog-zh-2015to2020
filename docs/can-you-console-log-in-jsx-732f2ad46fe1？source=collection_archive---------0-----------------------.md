# 你能在 JSX 登陆吗？

> 原文：<https://javascript.plainenglish.io/can-you-console-log-in-jsx-732f2ad46fe1?source=collection_archive---------0----------------------->

## TLDR:嵌入表情

![](img/e0f9a5f5985cf39fe3b3f04cfb85aa81.png)

作为一名编码教师，我见过许多学生这样做:

```
render() {
  return (
    <div>
      <h1>List of todos</h1>
      console.log(this.props.todos)
    </div>
  );
}
```

这将不会在控制台中打印预期的列表。它只会在浏览器中呈现字符串*console . log(this . props . todos)*。

让我们先来看看一些非常简单的解决方案，然后我们会解释原因。

# 最常用的解决方案:

将表情嵌入你的 JSX:

```
render() {
  return (
    <div>
      <h1>List of todos</h1>
      { console.log(this.props.todos) }
    </div>
  );
}
```

如果您喜欢这篇文章，请考虑在以下网址订阅更多内容:

[](https://www.gimtec.io/) [## GIMTEC

### 作为一名软件工程师，每周为您撰写一篇新文章，或者查看我即将推出的课程。

www.gimtec.io](https://www.gimtec.io/) 

# 另一个流行的解决方案:

将您的`console.log`放在`return()`之前:

```
render() {
  console.log(this.props.todos);
  return (
    <div>
      <h1>List of todos</h1>
    </div>
  );
}
```

# 一个奇特的解决方案:

通过编写和使用您自己的`<ConsoleLog>`组件来发挥您的想象力:

```
const ConsoleLog = ({ children }) => {
  console.log(children);
  return false;
};
```

然后使用它:

```
render() {
  return (
    <div>
      <h1>List of todos</h1>
      <ConsoleLog>{ this.props.todos }</ConsoleLog>
    </div>
  );
}
```

# 为什么会这样呢？

我们必须记住，JSX 不是普通的 JavaScript，也不是 HTML。它是一种语法扩展。

最终，JSX 被编译成普通的 JavaScript。

例如，如果我们写出下面的 JSX:

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

它将被编译成:

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

我们来回顾一下`React.createElement`的参数:

*   `'h1'`:这是标签的名称，作为一个字符串
*   `{ className: 'greeting' }`:这些是`<h1>`中使用的道具。它被转换成一个对象。对象的关键字是道具的名称和值，也就是它的值。
*   `'Hello, world!'`:这个叫做`children`。它是在开始标签`<h1>`和结束标签`</h1>`之间传递的任何东西。

现在让我们回顾一下我们在本文开始时尝试编写的失败的 console.log:

```
<div>
  <h1>List of todos</h1>
  console.log(this.props.todos)
</div>
```

这将被编译成:

```
// when more than 1 thing is passed in, it is converted to an arrayReact.createElement(
  'div',
  {}, // no props are passed/
  [ 
    React.createElement(
      'h1',
      {}, // no props here either
      'List of todos',
    ),
    'console.log(this.props.todos)'
  ]
);
```

看看`console.log`是如何作为字符串传递给`createElement`的。它没有被执行。

有道理，上面我们有标题`List of todos`。计算机如何知道哪些文本需要执行，哪些是您想要呈现的？

**答**:它把两者都看成一个字符串。它总是将文本视为字符串。

因此，如果您想要执行它，您需要指定 JSX 这样做。通过用`{}`将其作为表达式嵌入。

这就对了。现在你知道在 JSX 的什么地方、什么时候以及如何使用`console.log`了！

如果你喜欢这篇文章，可以考虑加入我在 [GIMTEC](https://www.gimtec.io/) 的时事通讯。

[](https://www.gimtec.io/) [## GIMTEC

### 训练营后教育。我希望在我完成训练营时就拥有的每周时事通讯和课程。

www.gimtec.io](https://www.gimtec.io/)