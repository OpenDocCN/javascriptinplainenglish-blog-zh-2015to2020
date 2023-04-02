# 8 个反应技巧和窍门

> 原文：<https://javascript.plainenglish.io/8-react-tips-tricks-1f1a429d7da0?source=collection_archive---------1----------------------->

![](img/0a02f613e7fc386c232edf3eb33844ef.png)

Photo by [Kirk Morales](https://unsplash.com/@knation?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每个前端开发人员都以自己的方式实现 React。一些人将他们的处理函数直接写入他们的 JSX，另一些人编写单独的函数来处理所有额外的逻辑。无论你是一个经验丰富的 React 开发人员，还是刚刚入门，我都列出了 8 个技巧和窍门来帮助你编写代码。

## 1.React 可以给你的孩子分配钥匙，不需要想出创造性的方法

在 React 中创建元素列表时，需要一个[键](https://reactjs.org/docs/reconciliation.html#recursing-on-children)。键为这些元素提供了一个标识。当呈现具有所需键的元素数组时，您可能希望简单地执行如下操作:

```
{someData.map(item => <div key={item.id}>Hello!</div>)}
```

如果我告诉您有一个内置的方法可以自动为您分配键，会怎么样呢？输入`React.Children.toArray()`。

```
{React.Children.toArray(someData.map(item => <div>Hello!</div>)}
```

*侧提示:* [*不要使用*](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) `[*map()*](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)` [*指标。*](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)

## 2.片段速记

React 片段使我们避免了额外的 DOM 元素，但是这种简写方式可以为您节省一些时间(和空间)。

```
// Long
<React.Fragment></React.Fragment>// Short
<></>
```

当使用简写时，只要确保你的 linter 没有因为你使用它而尖叫，或者你的格式化程序错误地处理了简写并在你的代码中创建了一个随机的混乱。

## 3.保持组件小

保持你的组件小有助于你避免错误跟踪和代码滚动的噩梦。你的组件越小，就越容易测试，也就越容易让你的组件专注于它们真正应该做的事情。如果你的组件开始变大，重新评估逻辑，看看你是否能把其中的一部分移到实用程序、钩子中，或者甚至把那个组件分解成其他组件。

## 4.省略一个属性值默认为真

就像在 HTML 中为一个元素编写一个没有值的属性默认为 true 一样，省略一个组件的 prop 值也会默认为 true。

React 文档显示下面的两条线通过`autocomplete`道具作为`true`。

```
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

然而，React 团队[警告不要使用这个](https://reactjs.org/docs/jsx-in-depth.html#props-default-to-true)。

## 5.保持你的专有名称简单

有一些疯狂的道具名，但幸运的是，如果你的组件很小，你的道具名通常会跟在 suite 后面。

一些最流行的 React 组件库遵循一个通用的命名约定，选择保持它们的布尔属性简单:不使用前缀。我已经写了一篇关于命名布尔变量[的独立文章，在这里](https://medium.com/javascript-in-plain-english/naming-boolean-variables-prefixes-df973d7d7ec3)可以帮助你命名这些类型的道具。

## 6.构造函数中不需要 super()

如果你在构造函数中没有对你的道具做任何事情，你可以安全地从你的构造函数中省略掉你的`super()`。不过，请注意[衍生状态警告](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)。

## 7.保持你的逻辑远离你的 JSX

如果您的 JSX 中有一些逻辑正在运行，无论是回调还是一些元素的呈现，定义一个单独的函数对可读性有很大帮助。

```
// Headache later, polluted with logic
<input onClick={event => {
  // Some logic here
}}/>// No headache later, easy to find and follow
handler = event => {
  // Some logic here
}<input onClick={handler}/>
```

## 8.功能组件可以被称为功能

这当然是一个技巧，如果你在公共项目中使用它，开发人员可能会讨厌你。这严格适用于功能组件。

```
<SomeComponent/> => SomeComponent()
```

你有它！#8 纯粹是一个聚会诡计，如果你想炫耀它，确保告诉其他人它实际上节省了渲染时间(没有任何意义)。请在评论中提出你认为有用的其他建议或技巧。