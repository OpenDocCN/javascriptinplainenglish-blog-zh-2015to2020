# 揭秘 mapStateToProps，供初学者反应和还原

> 原文：<https://javascript.plainenglish.io/demystifying-mapstatetoprops-for-beginners-to-react-and-redux-27136721a3c6?source=collection_archive---------4----------------------->

## 从我自己的教学经验来看，`mapStateToProps`和`connect`的概念对于初学者来说是最难理解和重复的。

![](img/c74b754c9e031d45600231af58667f98.png)

**TLDR；去看看 Dan Abramov 在 connect.js 上的[解释。](https://gist.github.com/gaearon/1d19088790e70ac32ea636c025ba424e)**

## 介绍

`mapStateToProps`是一个函数。`connect`是高阶成分。那有帮助吗？可能不会，但请记住这一点。至少这一个:

> `mapStateToProps`是一个函数。

## 使用

如果你正在学习 React 和 Redux，你可能已经见过很多次了:

```
export default connect(mapStateToProps)(MyComponent);
```

让我们把它分解成更小的代码片段和一些伪代码。

```
const ConnectedComponent = connect(mapStateToProps)(MyComponent);
export default ConnectedComponent;
```

首先，`connect`创建一个新的 React 组件，它是导出的组件。

```
import MyComponent from './MyComponent';//...
<MyComponent test="hello" />
```

这里要记住的最重要的事情是，即使你从`./MyComponent`导入，你也没有使用你直接编写的组件。您实际上正在使用`connect`创建的组件:`ConnectedComponent`。

## 说明

这个新组件有什么特别之处？通过添加额外的道具来表演一些魔术。

当你使用`<MyComponent test="hello" />`时，你实际上是在做`<ConnectedComponent test="hello" />`。您的`MyComponent`正在`ConnectedComponent`内部使用:

```
*// in the ConnectedComponent*
render() {
 *// This is NOT how actually works. Keep up with me for now.*
  return <MyComponent { ...this.props } />;
}
```

一旦清楚了你创建的组件`MyComponent`实际上在别的地方被使用，我们就可以使用一些魔法了。有趣的是，在`ConnectedComponent`中，我们可以访问一些东西:

*   `store`:Redux 店。也就是说，我们可以访问`getState`。
*   `mapStateToProps`:您在创建`ConnectedComponent`时使用的函数。

让我们使用它们:

```
*// in the ConnectedComponent*
render() {
 *// as stated above, we have access to the Redux Store*
  const reduxState = store.getState();
 *// Here comes the cool part!*
  const moreProps = mapStateToProps(reduxState, this.props);
 *// The return value of the FUNCTION `mapStateToProps` will be   passed as props to the component.*
  return <MyComponent { ...this.props } { ...moreProps } />;
}
```

在`ConnectedComponent`内部，我们传递给`connect`的函数`mapStateToProps`正在被使用。参数为 Redux 状态，道具传递给`ConnectedComponent`。在这种情况下是`{ test: 'hello' }`。

`mapStateToProps`的重要部分是返回值。它返回的值将作为道具传递给`MyComponent`。这就是为什么`mapStateToProps`的返回值需要是一个对象。

## 结论

使用`connect`创建一个新组件。这就是它被称为高阶元件的原因。

这个新组件调用我们的`mapStateToProps`函数和我们传递给`connect`的`MyComponent`。

这个新的`ConnectedComponent`可以访问 Redux 存储，这就是为什么它可以使用`getState`来获取状态，并在调用我们的`mapStateToProps`函数时传递它。

## 警告

这个解释没有涉及到`connect`的第二个参数，这个参数叫做`mapDispatchToProps`。也不是对`connect`的完整解释。

如需进一步解释，请查看本文开头提到的丹·阿布拉莫夫的解释。是`connect`的终极解释。一旦你理解了这一点，你就可以开始了。

**感谢阅读，快乐编码！**

如果你喜欢这篇文章，可以考虑加入我在 GIMTEC 的时事通讯。

[](https://www.gimtec.io/) [## GIMTEC

### 训练营后教育。我希望在我完成训练营时就拥有的每周时事通讯和课程。

www.gimtec.io](https://www.gimtec.io/)