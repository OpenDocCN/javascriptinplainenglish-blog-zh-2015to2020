# 回应技术面试问题

> 原文：<https://javascript.plainenglish.io/react-v16-technical-interview-questions-1d9b85e6167d?source=collection_archive---------2----------------------->

![](img/855c478dfd34506d5624211862486a50.png)

Photo by [Ryan Moreno](https://unsplash.com/@ryanmoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.什么是纯组件

*   除了为您处理`shouldComponentUpdate`方法之外，`PureComponent`与`Component`完全相同
*   当道具或状态改变时，`PureComponent`会对道具和状态做一个浅显的比较

# 2.反应状态是什么

*   组件的状态是一个对象，它包含一些在组件的生命周期中可能会改变的信息
*   状态用于组件内部的内部通信
*   状态类似于道具，但它是

**状态 vs 道具**

1.  私人的
2.  完全由组件控制(即，除了拥有和设置它的组件之外，任何组件都不能访问它

# 3.React 中的道具是什么

*   道具是组件的输入
*   属性是从父组件传递到子组件的数据

**React 中道具的用途**

1.  将自定义数据传递给组件
2.  触发状态变化

# 4.状态和道具有什么区别

**道具**

*   类似于函数参数，属性被传递给组件

**状态**

*   状态在组件中的管理类似于在函数中声明的变量

# 5.为什么我们不应该直接更新状态

*   如果您尝试直接更新状态，那么它将不会重新呈现组件
*   相反，`setState()`安排更新组件的状态对象

# 6.如何在 JSX 回调中绑定方法或事件处理程序

## **> 1 在构造函数中绑定**

## > 2 公共类字段语法

*   如果您不喜欢使用`bind`方法，那么可以使用`public class fields`语法来正确绑定回调

## 回调中超过 3 个箭头函数

*   你可以在回调中直接使用`arrow functions`

## 重要*

*   如果回调作为 prop 传递给子组件，这些组件可能会进行额外的重新呈现

# 7.React 中的合成事件是什么

*   `SyntheticEvent`是围绕浏览器本地事件的跨浏览器包装器
*   类似于`stopPropagation()`和`preventDefault()`，除了事件在所有浏览器中的工作方式相同

# 8.什么是“关键”道具，在元素数组中使用它们有什么好处

*   `key`是一个特殊的字符串属性，在创建元素数组时应该包含它
*   关键字有助于识别哪些项目发生了更改、被添加或被删除

## **重要***

*   如果项目的顺序可能会改变，不建议使用索引键
*   这可能会对性能产生负面影响，并可能导致组件状态出现问题

# 9.如何创建参考文献

## **有两种方法**

## >第一种方法

*   可以使用`React.createRef()`方法创建引用，并通过`ref attribute`将其附加到 React 元素
*   为了在整个组件中使用`refs`，只需将`ref`分配给构造函数中的实例属性

## >第二种方法

*   使用引用回调(不管 React 版本如何)

**例如，**

*   搜索栏组件的输入元素访问如下:

# 10.什么是前锋参考

*   引用转发是一个特性，它允许某个组件接收一个引用，并将其进一步传递给一个子组件

# 11.什么是受控组件

*   在后续用户输入时控制表单中的输入元素的组件

**例如，**

*   要用大写字母写所有的名字，我们可以使用 handleChange

# 12.什么是不受控组件

*   不受控制的组件是那些在内部存储它们自己的状态的组件，当你需要的时候，你可以使用一个 **ref** 来查询 DOM 以找到它的当前值
*   换句话说，不受控制的组件就像传统的 HTML

# 13.React 中的提升状态是什么

*   当几个组件需要共享相同的变化数据时，建议将共享状态延续到它们最近的共同祖先

# 14.什么是高阶组件

*   HOC 是一个接受一个组件并返回一个新组件的函数
*   基本上，这是一种源自 React 的组合性质的模式

## 特殊福利

1.  代码重用
2.  渲染劫持
3.  状态抽象和操作
4.  道具操作

# 15.什么是语境

*   上下文提供了一种通过组件树传递数据的方式，而不必在每一层手动向下传递属性(即 Redux、Flux……)

**例如，**

*   许多组件需要在应用程序中访问经过身份验证的用户、本地首选项、UI 主题

# 16.什么是儿童道具

*   Children 是一个道具`this.prop.children`，它允许你将组件作为数据传递给其他组件
*   放在组件开始和结束标签之间的组件树将作为`children`属性传递给该组件

**React API 中可用于此道具的方法数量**

1.  React.children.map
2.  做出反应。Children.forEach
3.  做出反应。儿童.计数
4.  做出反应。仅限儿童
5.  做出反应。儿童. toArray

# 17.使用带 props 参数的超级构造函数的目的是什么

*   在调用`super()`方法之前，子类构造函数不能引用`this`
*   将 props 参数传递给`super()`调用的主要原因是为了访问子构造函数中的`this.props`

# 18.什么是和解

*   当组件的属性或状态改变时，React 通过比较来决定是否需要实际的 DOM 更新
*   当它们不相等时，React 将更新 DOM
*   这个过程被称为**对帐**

# 19.如何用动态键名设置状态

*   使用计算的属性名

# 20.为什么片段使用在容器上

*   稍微快一点
*   一些 css 机制，如 Flexbox 和 CSS Grid，有特殊的父子关系，在中间添加 div 会很难保持理想的布局

# 21.React v16 中的错误界限是什么

*   错误边界是捕捉子组件树中任何位置的 JavaScript 错误，记录这些错误，并显示一个后备 UI 而不是崩溃的组件树的组件
*   如果一个类组件定义了一个名为`componentDidCatch(error, info)`或`static getDerivedStateFromError()`的新的生命周期方法，它就会变成一个错误边界

# 22.` getDerivedStateFromprops()`生命周期方法的目的是什么

*   新的静态`getDerivedStateFromProps()`生命周期方法在组件实例化之后和重新呈现之前被调用
*   它可以返回一个对象来更新状态，或者返回 null 来表示新的 props 不需要任何状态更新
*   `componentDidUpdate()`涵盖了`componentWillReceiveProps()`的所有用例

# 23.` getSnapshotBeforeUpdate()`生命周期方法的目的是什么

*   `getSnapshotBeforeUpdate()`在 DOM 更新之前调用生命周期方法
*   这个生命周期方法和`componentDidUpdate()`一起涵盖了`componentWillUpdate()`的所有用例

# 24.为什么需要将函数传递给 setState()

*   这背后的原因是`setState()`是一个异步操作
*   出于性能原因，React 批处理状态更改，因此在调用`setState()`后，状态可能不会立即更改
*   这意味着在调用`setState()`时不应该依赖当前状态，因为你不能确定状态将会是什么
*   解决方案是将一个函数传递给`setState()`，并将之前的状态作为参数

# 25.React 中的严格模式是什么

*   `React.StrictMode`是突出应用程序中潜在问题的有用组件
*   它为其子代激活附加检查和警告(仅适用于开发模式)

**重要***

*   在本例中，严格模式检查仅适用于`<ComponentOne>`和`<ComponentTwo>`组件

# 26.什么是反应混合

*   Mixins 是一种将组件完全分离以获得共同功能的方法
*   不应使用混频器，可以用高阶元件(HOC)代替

# 27.为什么“isMount()”是反模式，正确的解决方案是什么

*   `isMount()`的主要用例是避免在组件被卸载后调用`setState()`,因为它会发出警告

**解决方案**

*   理想情况下，任何回调都应该在卸载之前在`componentWillUnmount()`中取消

# 28.为什么组件名要以大写字母开头

*   组件的名称必须以大写字母开头
*   否则，React 将抛出一个错误，作为无法识别的标签，因为只有 HTML 元素和 SVG 标签可以以小写字母开头
*   您可以定义以小写字母开头的组件类

*   **但是，**导入的时候应该有大写字母

# 29.React v16 支持自定义 DOM 属性吗？

*   反应 v16 == >否之前
*   反应后 v16 == >是
*   这对于提供特定于浏览器的非标准属性、尝试新的 DOM APIs 以及与第三方库集成非常有用

# 30.你能在不调用 setState 的情况下强制组件重新呈现吗

*   默认情况下，当组件的状态或属性改变时，组件将重新呈现
*   如果你的`render()`方法依赖于其他数据，你可以通过调用`forceUpdate()`告诉 React 组件需要重新渲染

# 31.在 React 使用 ES6 类中,“super()”和“super(props)”有什么区别

*   当你想访问`constructor()`中的`this.props`时，你应该将道具传递给`super()`方法

## **超级(道具)**

## 超级()

## *重要*

*   在`constructor()`之外，两者将为`this.props`显示相同的值

# 32.如何访问属性引号中的属性

*   React 不支持属性值内的变量插值

```
// NOT WORK
<img src='images/{this.props.image}'>
```

## 解决办法

1.  花括号内的 JS 表达式

```
<img src={'images/' + this.props.image}>
```

2.模板字符串

```
<img src={`images/${this.props.image}`}>
```

# 33.什么是用形状反应原型数组

*   如果你想将一个对象数组传递给一个具有特定形状的组件，那么使用`React.PropTypes.shape()`作为`React.PropTypes.arrayOf()`的参数

> 感谢您的阅读，并希望您在事业上取得成功

![](img/6d23722524894cd36df0b714175dc200.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)