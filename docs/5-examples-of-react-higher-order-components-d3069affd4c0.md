# 5 反应高阶组分的例子

> 原文：<https://javascript.plainenglish.io/5-examples-of-react-higher-order-components-d3069affd4c0?source=collection_archive---------2----------------------->

![](img/01ca668f80d9743969b95b54e5a93268.png)

# **为什么是高阶组件(HOC)**

冗余代码在编程领域不受欢迎，高阶组件(HOC)通常用于减少冗余

*   HOC 是一个以一个分量作为输入，输出一个分量的函数。

**【输入分量===输出分量；仅逻辑变化使用变化使用 HOC]**

## 每当发生 ***装载*** 时，1ST_ HOC 向服务器发送事件

```
// withMountEvent.jsfunction withMountEvent(InputComponent, componentName) { **// 1** return class OutputComponent extends Component { componentDidMount() { sendMountEvent(componentName); } render() { return <InputComponent {...this.props} />; } };}// MyComponent.jsxfunction MyComponent() { // ...}export default withMountEvent(MyComponent, 'MyComponent'); **// 2**
```

**// 1 :** `withMountEvent`功能是特设的。换句话说，一个叫做`sendMountEvent`的普通函数是使用`withMountEvent`函数实现的

**// 2 :** 如果一个组件想要使用一个名为`sendMountEvent`的公共函数，则该组件作为 HOC 的输入

## 发送“已安装”是否发生的 2ND_ HOC

```
function withHasMounted(inputComponent) { return class OutputComponent extends React.Component { state = { hasMounted: false, }; componentDidMount() { this.setState({ hasMounted: true }); } render() { const { hasMounted } = this.state; return <inputComponent {...this.props} hasMounted={hasMounted} />; **// 1** } };}
```

**//1 :** `hasMounted`属性被传递给输出组件

**有用案例:**在一个服务器渲染的项目中，有一种情况是你想只从客户端渲染一些组件。如果组件是在挂载生命周期之后**呈现的，那么它只从客户端呈现，所以上面的代码变得非常有用**

## 基于登录状态呈现不同组件的 3RD_ HOC

```
function withOnlyLogin(InputComponent) { return function({ isLogin, ...rest}) { if (isLogin) { return <InputComponent {...rest} />; } else { return <p>You are not authenticated</p>; } };}
```

*   如果**是 Login === true** ，则渲染输入组件，否则显示**“您未通过身份验证”**
*   简单的代码，但是没有`withOnlyLogin` HOC，你可能需要一个代码来检查每个组件的登录状态(这是非常多余的)
*   在 HOC 中，可以使用一些属性并传递其余的属性`...rest`

## 用于调试的 4TH_ HOC

```
function withDebug(InputComponent) { return class OutputComponent extends InputComponent { render() { return ( <React.Fragment> <p>props: {JSON.stringify(this.props)}</p> **// 1** <p>props: {JSON.stringify(this.props)}</p> {super.render()} **// 2** </React.Fragment> ); } };}
```

**// 1 :** 使用`this`访问输入组件的状态和属性

**// 2:** 调用`render`方法输入组件。换句话说，`withDebug` HOC 一直显示输入组件的道具&状态

**HOC 使用类继承**

*上面的例子使用了使用类继承的 HOC*

*   OutputComponent `OutputComponent`可以访问 InputComponent `InputComponent`的实例
*   换句话说，OutputComponent 可以访问输入组件的 props、状态、变量、方法、生命周期的实例

```
function withSomething(InputComponent) { return class OutputComponent extends InputComponent { // ... };}
```

## 第 5 个 _ 多个 hoc

```
withDubg(withDiv(myComponent));
```

*   可以使用多个 hoc，但是使用太多的 hoc 对性能没有好处&使调试变得困难

## **用简单英语写的 JavaScript 的一个注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。