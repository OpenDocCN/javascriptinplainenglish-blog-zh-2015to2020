# React 中的最佳实践

> 原文：<https://javascript.plainenglish.io/best-practices-in-react-94ccbde2220e?source=collection_archive---------3----------------------->

![](img/4863e76e08fb5607f0608ac3748de89e.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是前端开发人员中最流行的框架之一。你可能开发了许多 React 应用程序，或者你可能是一个新手，每次我们都想开发最好的。在那篇文章中，从设计、绑定和钩子等新概念的角度评估了一些最佳实践。

希望你会喜欢:)

**组件设计**

在进入组件设计之前，让我们看看组件是如何在 React 中呈现的。

*   当它属性改变时
*   当它的状态改变时
*   当其父级渲染时，每个子级都被重新渲染

这里要说明的一点是关于第三项的。让我们指出问题，然后描述如何解决问题。

```
<ParentComponent>
  <ChildComponent />
</ParentComponent>
```

假设我们有一个类似上面例子的组件设计。当 ParentComponent 被重新渲染时，它的子组件将被重新渲染，不管它是否应该被重新渲染。这将导致不必要的重新渲染，并会降低你的前端性能。

我们做什么呢

> 避免从 React 扩展组件。组件，并尝试选择功能组件，因为你现在可以有钩子的力量。

你有 3 个选择来避免不必要的重新渲染。

> 虽然据说 React 真的很快，而且不怕不必要的重新渲染，但这取决于你的项目。如果你的项目是实时更新的，数据获取如此频繁，你应该开始考虑不必要的重新渲染。

*   如果从 React 扩展组件。组件，覆盖*shouldcomponentdupdate*函数。这个函数返回一个布尔值，根据返回值，你的组件将被重新渲染或者不被渲染。然而，问题是你必须将每个属性值与之前的值进行比较，以检查是否有改变，你的组件是否会被重新渲染。所以，每当你添加一个新道具时，你也必须把这个对比添加到你的检查表中。

```
shouldComponentUpdate(nextProps, nextState){
   return nextProps.id !== thisp.rops.id || nextState.name !== this.state.name;
}
```

> 不要忘记在比较语句中添加新状态和属性值

*   扩展你的组件*做出反应。纯组件*。PureComponent 内部运行 shouldComponentUpdate 函数。然而，这样做的问题是，如果你的属性或状态模型由复杂的嵌套 JSON 组成，它可能总是返回 true。

> 做出反应。如果您的状态和属性值由原始值或者非嵌套的 JSON 对象组成，那么 PureComponent 是很好的选择。对于嵌套的 JSON 模型，它可能总是返回 true，并进行不必要的重新渲染。

*   我推荐的是使用功能组件，忘记上面提到的项目。更简单的组件，不考虑组件的生命周期。

> 有了 hook 的实现，您可以尽可能多地使用功能组件，这将为您提供比 React 更好的性能。成分

你可以在 [codesandbox](https://codesandbox.io/embed/component-render-performance-990q7?fontsize=14) 上查看工作示例。每次你点击按钮，你可以看到哪些组件重新呈现在控制台上。

关于 [codesandbox](https://codesandbox.io/embed/component-render-performance-990q7?fontsize=14) 的真实例子

**抽象化**

React 改变了开发者的思维方式。它通过建议您将组件分成更小的部分，迫使前端开发人员创建可重用的组件。

> 不要害怕将你的组件分割成小块。尽可能创造小的作品。这些将帮助您创建更多的可重用组件。

**高阶组件(HOC)**

HOC 是通过添加新特性返回新组件的函数，但不改变包装组件的内部状态。为了保持一致性，组件必须为相同的输入提供相同的输出。因此，当您在 HOC 中更改组件的内部状态时，您可能会失去组件输出的一致性。

```
const withReversedLabel = WrappedComponent => {
  return class extends React.Component {
    render() {
      const label = this.props.label
        .split("")
        .reverse()
        .join("");
      return <WrappedComponent label={label} />;
    }
  };
};
```

关于 [codesandbox](https://codesandbox.io/embed/hoc-jnck2?fontsize=14) 的真实示例

**包装组件**

包装组件的语法不同于 HOC。在 HOC 中，通过向现有组件添加新功能来返回新组件，而在包装组件设计中，通过添加新功能来呈现子组件。

```
const Card = ({ children }) => <div className="card">{children}</div>;
const Content = () => <div>My content</div>;
const Content2 = () => <div>My content 2</div>;function App() {
  return (
    <>
      <Card>
        <Content />
      </Card>
      <Card>
        <Content2 />
      </Card>
    </>
  );
}
```

你得到了什么？

*   你得到了更多基于组件的语法
*   您可以限制 WrapperComponent 中的子计数
*   您刚刚创建了一个模板 card 组件，您可以在 card component 中呈现您想要的任何内容

关于 [codesandbox](https://codesandbox.io/embed/wrapper-component-mcbxb?fontsize=14) 的真实例子

**渲染道具**

在这种模式中，您可以在另一个组件中共享一个组件的属性。它类似于 HOC，但语法更强大。

假设您有一个名为 Fetch 的组件，可以管理 rest 调用。在这个组件中，您将处理加载、错误和数据状态。另一个名为 Students 的组件需要获取数据，您不希望将该业务包含在 Student 组件中，但希望使用 fetch 组件。

> Render props 是一种将组件作为函数参数发送到包装类的模式。

```
<Fetch 
      url="abc.com"
>
      {({ loading, error, data }) => {
        if (loading) {
          return <div>Loading!</div>;
        } else if (error) {
          return <div>{error}</div>;
        } else {
          return <Student data={data} />;
        }
      }}
</Fetch>
```

关于 [codesandbox](https://codesandbox.io/embed/render-props-8b26p?fontsize=14) 的真实例子

**功能绑定**

让我们先定义问题。大多数开发人员开始使用 arrow 函数，因为它简单，不需要在构造函数中绑定，而且比在构造函数中绑定函数性能更好。但是，如果使用不当，它会影响性能。

```
// bad examplefunction MyComponent = () => {
   return <button onClick={() => callMe()}>Click me</button>
}
```

这里的问题是，每次组件重新呈现时，都会为 callMe 函数创建一个新的引用。

为了解决这个问题，您可以将函数从 onClick 事件处理程序中移出。

```
function MyComponent = () => {
   const handleClick = () => console.log(1); return <button onClick={handleClick}>Click me</button>
}
```

但是，当我像上面那样使用函数时，如何绑定自定义值呢？

假设您有一个列表，并且希望将您的 id 与按钮的 onClick 功能绑定。

```
function App() {
  const handleClick = id => console.log(id);return datas.map(data => (
    <button onClick={handleClick.bind(null, data.id)}>Click me!</button>
  ));
}
```

[代码沙箱](https://codesandbox.io/embed/function-binding-ilc0q?fontsize=14)上的实时示例

**使用钩子&记忆**

在 v16.8 中，钩子进入了功能组件的反应生命周期，为类组件提供动力。有很多内置钩子可以使用，比如 useState、useMemo、useCallback 等..，您还可以定义自己的钩子，以便在功能组件中使用。

**使用状态**

在钩子出现之前，功能组件被称为无状态功能组件，因为它们没有状态。然而，在钩子出现之后，事情发生了变化，无状态功能组件的名字变成了功能组件。

**如何使用 useState？**

重要的是，在使用 useState 时，它的行为不同于在基于类的组件中使用的 setState()函数。当您使用这个. setState()函数时，它会将发送的值与其已经存储的状态合并，而 useState 只是用新的状态覆盖它的整个状态。

下面以最简单的方式显示了 useState 的用法。

```
const [myState, setMyState] = useState(1); 
```

> 不要忘记初始化您的状态值

**反应备忘录**

如第一节所述，每当父组件的状态/属性改变时，它的所有子组件都会被重新呈现。但是，在 React 中定义了一个函数，您可以使用它来避免不必要的重新渲染。

response . memo 只是记忆组件的道具，并且在内部每当你的道具改变时会重新渲染你的组件。它类似于为基于类的组件描述的应该组件更新方法，但是这个方法只针对功能组件。

你可以在活生生的例子中看到。有一个父组件和两个子组件，一个由 React.memo 包装，另一个没有。每当你点击父组件的按钮来触发状态改变时，你会看到功能组件没有反应。尽管它的状态或属性没有改变，但是备忘录将会被重新呈现。

[代码沙箱](https://codesandbox.io/embed/hardcore-panini-8y2dz?fontsize=14)上的实时示例

*如果你喜欢我的文章，你可以鼓掌和跟着我来支持我。
我也在*[*LinkedIn*](http://www.linkedin.com/in/muratcatal)*上，欢迎所有的邀请。*