# React 中什么时候应该用花括号{ }和圆括号()。

> 原文：<https://javascript.plainenglish.io/curly-braces-versus-parenthesis-in-reactjs-4d3ffd33128f?source=collection_archive---------0----------------------->

## 在职业生涯的开始阶段，当学习 React 和 ES6 JavaScript 语法时，可能会混淆何时使用花括号{ }以及何时使用括号()。

![](img/b74cde098ef5595bba66ccfbaac0aff1.png)

# 大括号{ }的用法

花括号{ }是 JSX 的特殊语法。它用于在编译期间评估 JavaScript 表达式。JavaScript 表达式可以是变量、函数、对象或任何解析为值的代码。

我们举个例子。

*   评估 JavaScript 变量

```
const yellowStyle={color: 'yellow'} <Star style={yellowStyle} />
```

这与

```
<Star style={{color: 'yellow'}} />
```

*   评估函数或事件处理程序

```
class PopUp extends React.Component { // es7 way of setting default state
  state = {
    visible: true;
  } render() {
    return <Modal onClose={this._handleClose}/>;
  } _handleClose = () => {
    this.setState({ visible: false });
  }}
```

这不要与 ES6 中的类方法混淆，后者也使用花括号{ }

```
class StarsComponent {
  constructor(size) {
    this.size = size;
  } // Curly braces are used to define a method body in a class
  get size() {
    return this.size;
  }

  // ReactJS library comes with a predefined render() method
  render() {
    return <div>*</div>;
  }}

const stars = new StarsComponent(5);

console.log(stars.size); // 5
```

# 括号( )是如何使用的？

括号在 arrow 函数中用于返回对象。

```
() => ({ name: 'Amanda' })  // Shorthand to return an object
```

这相当于:

```
() => {
   return { name : 'Amanda' }
}
```

括号用于对 JavaScript `return`语句中的多行代码进行分组，以防止分号自动插入错误的位置。

先说一些基本面。

JavaScript 中没有必要添加分号。JavaScript 引擎会在一个`return`语句后的一行中第一个可能的位置自动插入一个分号。如果 JavaScript 引擎把分号放在了不该放的地方，你的代码就不会编译。

让我们看看下面的代码。

内伊，这不编译。

```
class StarsComponent {
  constructor(size) {
    this.size = size;
  }

  render() {
    return <div>  // <--JavaScript engine inserts semicolon here
           *
           </div>
  }}
```

耶，这个管用。

```
class StarsComponent {
  constructor(size) {
    this.size = size;
  }

  render() {
    return (<div> 
            *
            </div>) // <--JavaScript engine inserts semicolon here
  }}
```

为什么？当您将左括号与`return`放在同一行时:

```
return (
```

您是在告诉 JavaScript 引擎，在括号结束之前，它不能自动插入分号。

```
return (
    ... 
    ...
); <-- inserts semicolon at the end of parenthesis
```

对于单行语句，我们不需要将它放在括号内。

```
class StarsComponent {
  constructor(size) {
    this.size = size;
  }

  // Not required to wrap brackets around a single line of code
  render() {
    return <div>*</div>;
  }}
```