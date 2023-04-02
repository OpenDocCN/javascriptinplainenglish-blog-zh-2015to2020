# React 提示—错误处理、输入验证等等

> 原文：<https://javascript.plainenglish.io/react-tips-error-handling-input-validation-and-more-8d17ef989be4?source=collection_archive---------1----------------------->

![](img/06d4f608648f0b978268c2edfa8fbd86.png)

Photo by [George Bonev](https://unsplash.com/@spktwo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 将没有值的属性传递给组件

如果组件的值是`true`，我们可以将不带值的布尔属性传递给组件。

例如，我们可以写:

```
<input type="button" disabled />;
```

而不是:

```
<input type="button" disabled={true} />;
```

他们是一样的。

# 通过引用访问子组件功能

我们可以在一个类组件中指定一个引用来访问它的方法。

例如，我们可以写:

```
class App extends React.Component {
  save() {
    this.refs.content.getWrappedInstance().save();
  } render() {
    return (
      <Dialog action={this.save.bind(this)}>
        <Content ref="content"/>
      </Dialog>);
   }
}class Content extends React.Component {
  save() { 
    //... 
  }
}
```

我们有`App`组件，它的`Content`被分配了一个 ref。

然后我们就可以用`getWrapperdInstance`得到组件实例了。

我们可以用它来调用`save`方法。

# getDerivedStateFromError 和 componentDidCatch

两者都是让我们处理类组件中的错误的方法。

`getDerivedStateFromError`是一个静态方法，让我们在错误发生时设置一个状态。

`componentDidCatch`让我们提交副作用并访问`this`，这是组件实例。

例如，我们可以写:

```
class ErrorBoundary extends React.Component {
  state = { hasError: false }; static getDerivedStateFromError(error) {
    return { hasError: true };
  } componentDidCatch(error, info) {
    console.log(error, info);
  } render() {
    if (this.state.hasError) {
      return <h1>error.</h1>;
    } return this.props.children; 
  }
}
```

我们有静态的`getDerivedStateFromError`方法来访问`error`参数。

然后我们可以返回下一次渲染时的状态。

`componentDidCatch`让我们访问组件实例。

它还有误差数据较多的`error`和`info`参数。

`getDerivedStateFromError`使用服务器端渲染。

这是因为它是一个渲染阶段生命周期，可用于服务器端渲染应用程序。

# 允许文件输入选择 React 组件中的相同文件

我们可以让一个文件输入选择 React 组件中的同一个文件，如果我们在点击它之后将它设置为`null`的话。

例如，我们可以写:

```
<input 
  id="upload" 
  ref="upload" 
  type="file" 
  accept="image/*"
  onChange={(event)=> { 
    this.readFile(event) 
  }}
  onClick={(event)=> { 
    event.target.value = null
  }}
/>
```

我们有`onChange` prop，它采用一个函数来读取文件。

在`onClick`属性中，我们将文件输入值设置为`null`。

# 将数字传递给组件

我们可以将一个数字传递给一个组件，方法是在花括号中输入一个数字。

例如，我们可以写:

```
Rectangle width={10} height={20} />
```

# React 路由器中不同的路由路径使用相同的组件

我们可以通过将一个数组传入到`path`属性中，为不同的路由使用相同的组件。

所以我们可以写:

```
<Route exact path={["/add", "/edit"]}>
  <User />
</Route>
```

# Yup 中的条件验证

我们可以通过使用`when`方法用 Yup 验证一个字段的条件性。

例如，我们可以写:

```
validationSchema={yup.object().shape({
  showName: yup.boolean(),
  name: yup
    .string()
    .when("showName", {
      is: true,
      then: yup.string().required("name is required")
    })
  })
}
```

我们有`showName`布尔字段。

我们只在`is`字段中显示的`true`时验证`name`字段。

`then`让我们只在`showName`为`true`时进行验证。

如果是，我们就返回`'name is required'`消息。

# “修复”失败的表单属性类型:您向没有 onChange 处理程序的表单字段提供了选中的属性。警告

要修复这个警告，我们可以在创建控制器组件时添加带有值的`checked`属性。

例如，我们可以写:

```
<input
  type="checkbox"
  checked={this.props.checked}
  onChange={this.onChange} 
/>
```

如果它是一个不受控制的组件，我们可以编写填充`defaultChecked`属性:

```
<input
  type="checkbox"
  defaultChecked={this.props.checked} 
/>
```

# 仅允许在 React 中输入数字

我们可以将 pattern 属性设置为 regex 字符串，以便将输入限制为数字。

例如，我们可以写:

```
<input type="text" pattern="[0-9]*" onInput={this.handleChange.bind(this)} value={this.state.goal} />
```

我们指定了模式，并听取了`inpurt` pro。

那么在`handleChange`方法中，我们可以写:

```
handleChange(evt) {
  const goal = (evt.target.validity.valid) ? 
  evt.target.value : this.state.goal;
  this.setState({ goal });
}
```

我们只设置状态，如果它是有效的，那么我们不会在状态中得到任何无效的输入。

![](img/a1cbac5650e71078fc77751096ff6033.png)

Photo by [Keenan Barber](https://unsplash.com/@keebarber?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在输入处理程序中检查输入的有效性。

此外，我们不必显式地将`true`传递给一个道具。

错误边界组件有特殊的钩子，我们可以在错误发生时运行代码。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**