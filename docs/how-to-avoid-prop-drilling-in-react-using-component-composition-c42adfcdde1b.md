# 如何在使用组件组合反应中避免正确的钻取

> 原文：<https://javascript.plainenglish.io/how-to-avoid-prop-drilling-in-react-using-component-composition-c42adfcdde1b?source=collection_archive---------0----------------------->

## 没有 Redux 或上下文 API

![](img/da959d490f7f4ec445eaab7bf9c84fff.png)

Prop Drilling 是将数据从 React 组件树的一个部分传递到另一个部分的过程，方法是遍历不需要数据但只是帮助传递数据的其他部分。

想象一下，住在尼日利亚拉各斯的某人在亚马逊上订购了一个包裹。包裹必须经过许多人的手——它必须空运到尼日利亚，运输到拉各斯，从一个地方搬到另一个地方，直到到达买家手中。在每一个阶段，亚马逊都雇佣不“关心”产品的人提供服务，他们只是帮助将产品“传递”给关心产品的人——买家。

道具钻取类似:你把数据(道具)从某个`FirstComponent`传递到另一个`SecondComponent`——它不需要数据渲染，只把它传递给另一个`ThirdComponent`，后者也不需要它，可能传递给另一个`FourthComponent`。这可能会持续到数据到达`ComponentNeedingProps`。

考虑下面的代码片段:

`content`属性被传递给根组件`App`中的`FirstComponent`。但是`FirstComponent`不需要道具渲染，它只把它传给`SecondComponent`，T9 把它传给`ThirdComponent`，T10 把它传给`ComponentNeedingProps`。正是这个组件使用`content`属性来呈现`<h3>{content}</h3>`中的内容。

# 为什么道具钻探是一个问题？

道具钻井不一定是个问题。如果我们只在 2 或 3 层之间传递数据，那就没问题。追踪数据的流动将会很容易。但是想象一下，我们正在钻 5 层，或者 10 层，或者 15 层。

![](img/20e21fa04d1fd9f59bdfd3f4af94efb4.png)

# 如何解决问题

正确钻探在 React 中并不是一个新问题(相当明显)，已经有许多解决方案让我们将数据向下传递到深层嵌套的组件。

其中一个是 Redux:你创建一个数据`store`和`connect`到`store`的任何组件，瞧，不管组件在组件树中的位置在哪里，它都可以访问存储。

React 也有`Context`的概念，它允许你创建类似于全局`data store`的东西，并且“上下文”中的任何组件都可以访问数据存储。

然而，如果您想在不使用上下文的情况下解决这个问题**，您可以使用 [React 文档所建议的组件组合:](https://reactjs.org/docs/context.html#when-to-use-context)**

*如果你只想避免一些道具通过许多关卡，组件组合通常是比上下文更简单的解决方案*

在使用 Context 之前，你可以在这里[了解更多信息，并且查看](https://reactjs.org/docs/context.html#before-you-use-context) [Michael Jackson](https://medium.com/u/7f9567e0d71c?source=post_page-----c42adfcdde1b--------------------------------) 的帖子，了解为什么你应该避免使用 Context API。

# 如何避免使用组件组合进行正确钻探

组件组合是指将不同的组件(通常很简单)组合在一起，以创建更复杂的功能。如果你曾经编写过 React 应用程序，我敢打赌你一直在编写组件。举个例子:

```
function LoginForm(props){
  return (
    <Input name="fname" />
    <Button onClick={props.handleClick} />)
}
```

这里，通过使用组合，我们创建了一个“复杂”的功能，`LoginForm` 通过组合两个简单的功能，`Button` 和`Input` 组件。你可以在 React [文档页面](https://reactjs.org/docs/composition-vs-inheritance.html)上阅读更多关于合成的内容。

# 我们试图解决的实际问题是什么？

实际的问题是，我们希望在`ThirdComponent`中呈现`ComponentNeedingProps`，但是它需要来自根组件`App`的数据来实现。换句话说，`ComponentNeedingProps`需要来自组件树中更高位置的数据(`App`)，从那里它被渲染(`ThirdComponent`)。

解决办法？

# 儿童道具的野性力量

您可以通过使一个组件成为另一个组件的子组件来组合组件，例如:

```
<ReactComponent1>
  <ReactComponent2 />
</ReactCompoent1>
```

`ReactComponent2`在`ReactComponent1`内部被调用，因此它是它的子节点。每个组件都有一个名为`children`的“自动”属性，用于保存组件的子组件。所以在`ReactComponent1`中我们可以写:

```
function ReactComponent1({ children }) {
  return 
   (<div> I render my children
      {children} 
   </div>)
}
```

在这种情况下我们如何使用它？请记住，我们希望`ComponentNeedingProps`呈现在组件树中的另一个组件中，如果我们可以将`ComponentNeedingProps`作为子组件传递给它所需要的数据，然后在它的父组件中呈现它，那么我们就成功地避免了属性钻取。

因此，我们将拥有:

```
<ThirdComponent>
 <h3>I am the third component</h3>
     <ComponentNeedingProps content={content}  />
</ThirdCompoent>
```

而在`ThirdComponent` 的宣言中我们有:

```
function ThirdComponent({ children }) {
 return (
 <div>
    <h3>I am the third component</h3>
     {children}
   </div>
 );
}
```

这看起来和我们之前的没有太大的不同，但是等待奇迹吧。

通过使用这种呈现孩子的技术，我们可以将`App`重构为:

```
function App() {
const content = "Who needs me?";
return (
<div className="App">
  <FirstComponent>
    <SecondComponent>
      <ThirdComponent>
        <ComponentNeedingProps content={content}  />
      <ThirdComponent>
    </SecondComponent>
  </FirstComponent>
</div>
);
}
```

然后我们重构其他组件来呈现它们的`children`

第一组件:

```
function FirstComponent({ children }) {
  return (
    <div>
      <h3>I am the first component</h3>;
     { children }
    </div>
  );
}
```

第二组件:

```
function SecondComponent({ children }) {
  return (
    <div>
      <h3>I am the second component</h3>;
     {children}
    </div>
  );
}
```

第三组件:

```
function ThirdComponent({ children }) {
  return (
    <div>
      <h3>I am the third component</h3>
        {children}
    </div>
  );
}
```

`ComponentNeedingProps`保持原样:

```
function ComponentNeedingProps({ content }) {
  return <h3>{content}</h3>
}
```

你看到了吗？我们通过从数据源`App` 直接给`ComponentNeedingProps`它需要的数据，然后通过使用子道具，我们将它传递到它应该被渲染的地方`ThirdComponent`，从而避免了道具钻取。

太棒了。查看完整代码:

## 什么时候应该使用上下文 API？

您还可以使用上下文 API 来避免适当的钻探，我可能会在不久的将来就此写另一篇文章。

如果您需要让不同嵌套层次的许多组件可以访问某些数据，那么您应该使用上下文 API。React docs 建议我们“谨慎使用它，因为它使组件重用更加困难。”换句话说，您可能无法“脱离上下文”重用您的组件。

这是这篇文章的全部内容，谢谢你的阅读。你可以在 [CodeSandbox](https://codesandbox.io/s/propdrilling-vs-composition-updated-qe8nm?file=/src/App.js:0-878) 上玩代码

快乐编码。

***注*** *:根据评论中提供的反馈，这篇文章已经进行了重大修改。我感谢所有提供有用反馈的人。下面的一些评论不适用于这次修订。*