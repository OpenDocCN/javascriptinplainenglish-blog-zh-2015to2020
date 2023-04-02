# React 中的 useRef

> 原文：<https://javascript.plainenglish.io/implementing-useref-in-react-732908aa1998?source=collection_archive---------1----------------------->

## 用户参考文献简介

![](img/76a1b05f1c26511927b67e6db5ec0734.png)

Photo by Clement H. (Unsplash)

在做一个个人项目的时候，我实现了`useRef`。它是 React 提供的一个钩子，相当于与类组件一起使用的`createRef`。

> `useRef`钩子是一个函数，它返回一个可变的`ref`对象，该对象的`.current`属性用一个参数初始化，(`initialValue`)。返回的对象将在组件的整个生存期内保持不变。([挂钩参考](https://reactjs.org/docs/hooks-reference.html#useref))

```
const refContainer = useRef(initialValue);
```

在我的项目中,`useRef`钩子有两个主要用途:访问 DOM 节点/元素和存储可变信息。让我们探索如何在 React 应用程序中实现`useRef`。

# 访问 DOM 节点/元素

在进一步潜水之前，我们先讨论一下`ref`是什么。它是一个用在 HTML 元素或组件标签上的属性。它为我们提供了一种在 React 中引用 DOM 节点的方法。

下面是一个在`div`元素中使用`ref`的例子。

```
<div className="sample" ref={divRef}>Sample Div</div>
```

还可以在 React 组件内部使用`ref`,但是只能使用父组件的 render 方法。

```
<DivComponent ref={divComponentRef} />
```

在常规 Javascript 中，这相当于语法，

```
const divElement = document.querySelector(".sample")
```

## useRef 和 Ref 的示例

在 React 中，`ref`属性将允许我们引用该元素，并为我们提供对其方法的访问。下面是功能组件中`ref`和`useRef`的示例。

我们有两个 DOM 元素，`input`和`button`。在按钮上，我们提供了一个带有回调函数`buttonClick`的事件处理程序`onClick`。当我们将一个 ref 对象传递给`input`时，React 会将`.current`属性设置为相应的 DOM 节点。因此，当我们为`textInputRef`创建`useRef`时，我们可以传入`null`或者什么都不传。

像`.focus()`方法一样，`textInputRef.current`将可以访问`input`元素的方法。当点击`button`时，回调函数调用那个`.focus()`方法，该方法聚焦在`input`元素上。

## 转发参考

假设`input`是组件内部的一个元素，但是`textInputRef`仍然会在`SampleComponent`中被引用。简单地传入一个`textInputRef`作为组件的支撑本身是不行的。为了将它传递下去，我们必须利用 React 的另一个资源，`forwardRef`。

> React forwarding 是一个选择性加入的特性，它允许一些组件接受它们接收到的 ref，并将其进一步传递给子组件。([反应参考转发文件](https://reactjs.org/docs/forwarding-refs.html))

让我们来看看这个会是什么样子。让我们重新构建我们之前的例子。

SampleComponent.js

在这个例子中，我们已经删除了`input`元素，并用一个导入的组件`AnotherComponent`替换了它。如前所述，我们可以利用组件上的`ref`属性，并且我们正在传入`textInputRef`。现在让我们探索一下`AnotherComponent`由什么组成。

AnotherComponent.js

该组件包含一个`h1`元素和我们之前使用的原始`input`元素。您可能会注意到函数所在的`React.forwardRef()`。该函数接收从父组件传递来的参数`props`和`ref`。

使用`ref`的第二个参数只在使用`React.forwardRef()`调用定义组件时存在。常规函数或类组件不会收到`ref`参数，也不会出现在`props`中。`ref`参数用于`input` `ref`属性，其作用与之前一样，即`SampleComponent`中的`textInputRef.current`引用了`input`。

# 存储可变信息

钩子不仅仅用于 DOM 引用。ref 对象是一个通用容器，它的`.current`属性是可变的，可以保存任何值，类似于类的实例属性( [Hooks FAQ](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables) )。

在 React 组件中，有两种方法可以在重新渲染之间保存数据:

*   在一个`state`变量中，更新状态将导致组件的重新呈现，
*   在`ref`中，改变`.current`属性不会导致重新渲染。

它们的共同点是，在任何重新渲染之后，它们都能记住它们的数据。更好的问题是我们什么时候会真正使用`useRef`而不是`useState`？存储数据时，React 应用程序可能不需要重新呈现所有内容。假设我们有以下的例子:

这里，我们同时使用了`useState`和`useRef`。`message`状态负责跟踪我们在`input`标签中输入的内容，这需要重新呈现组件以更新显示在`input`字段中的消息。`sentMessage` ref 跟踪发送的消息数量，这不一定需要触发重新呈现。

注意`useRef`值是如何改变的。它没有像`useState`那样自带一套方法。要更改该值，我们将直接重新分配存储在`useRef`的`.current`属性中的值。重要的一点是，改变`ref`是一种副作用，应该在使用 React 钩子时在`useLayoutEffect`或`useEffect`内部完成。

# 摘要

钩子允许我们在函数组件中创建可变变量。这对于访问 DOM 节点/React 元素，以及存储可变变量而不触发重新呈现非常有用。`useRef`更像是一个逃生出口，应该只在必要的时候使用，尤其是当它可以通过声明来完成的时候。

本指南是对`useRef`挂钩的快速介绍。钩子的其他实现和用例可以在 React 文档中引用。

感谢您的阅读！

# 参考

*   [https://reactjs.org/docs/refs-and-the-dom.html](https://reactjs.org/docs/refs-and-the-dom.html)
*   https://reactjs.org/docs/hooks-reference.html#useref
*   [https://react js . org/docs/hooks-FAQ . html # is-there-something-like-instance-variables](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables)
*   [https://reactjs.org/docs/forwarding-refs.html](https://reactjs.org/docs/forwarding-refs.html)