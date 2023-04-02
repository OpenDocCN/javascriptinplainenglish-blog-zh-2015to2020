# 我在最初几年使用 React 时犯的错误

> 原文：<https://javascript.plainenglish.io/mistakes-i-made-while-using-react-for-the-first-few-years-1b834ea4e5ad?source=collection_archive---------2----------------------->

*这是我个人的错误报告*

![](img/fe08af790df4346981bd83ced75dbadc.png)

Photo by [Charles Deluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# `1\. "history of undefined”`错误

*   当我在我的应用程序中使用`react-router-dom`作为 React 路由器时，我经常遇到`history of undefined`错误👊
*   此**错误的解决方案👊**是使用`withRouter`作为组件的包装器(HOC 方法)

## **解决方案**

```
import { withRouter } from 'react-router-dom';
import { connect } from 'react-redux';class App extends Component {
  ...
  render() {
  } export default connect(
    (state) => ({ ... }),
    (dispatch) => ({ ... }) )withRouter(App));
}
```

# `2\. "Expected an assignment or function call and instead saw an expression”`错误

*   此**错误**错误**👊当您用花括号({})将 arrow 函数的主体括起来，但不返回任何值时，会发生**

**例如**

```
const def = (props) => { <div></div> };
```

*   这不会返回任何值

## 解决办法

```
**// solution**// Instead of const def = (props) => { <div></div> };**// Change to**const def = (props) => { return (<div></div>); };**// OR**const def = (props) => <div></div>;
```

# `3\. "Error: Invariant failed: You should not use <Switch> outside a <Router>”`错误

*   我打赌你曾经面对过这个**错误👊**在使用 React 时至少要使用一次(尤其是，`react-router-dom`)
*   `Switch`类似于 JavaScript 中的`switch`语句，用于根据不同的条件执行不同的动作

```
switch (5) {
  case 0 :
    // do something
  case 1 :
    // do something
  ...}
```

## **解决方案**

*   `react-router-dom`中的`Switch`需要在每个`Route`的顶部

```
import { Switch, Route } from 'react-router-dom';<Switch>
  <Route path = '/path1' component={ component1 } />
  <Route path = '/path2' component={ component2 } />
  <Route path = '/path3' component={ component3 } />
  ...</Switch>
```

# `4\. "Parsing error: Lexical declaration cannot appear in a single-statement context"`

*   此**错误**错误**👊**发生在你使用`let`的时候(在 Chrome 中)
*   举个例子，

```
function TestError() {
    if (true) {
        let k2 = new Object("ss");
    }
}**// is being minified as**function TestError()
{
  if (1)
    let k2 = new Object("ss")
}
```

*   而 Chrome 上面的代码会得到**错误** **👊**

## 解决办法

*   不要使用更不用说

```
function TestError()
{
  if (1) {
    let k2 = new Object("ss") }}**// OR**function TestError()
{
  if (1)
    Object("ss")
}
```

# `5\. Higher-order component (HOC) ERROR`

*   当你试图将一组**道具**从父组件传递到子组件时，你经常使用`map`来迭代地发送一组道具(因为 React 不允许**道具**在一个数组中)
*   许多 React 开发人员在将一组道具传递给子组件时会犯一个常见错误。但是，如果您发现了两个代码之间的差异，就没有问题了👌

```
render() {
  { organizationData.map(schoolData => { <MSCList schoolID = {schoolData.schoolID} schoolName = {schoolData.schoolName} alignment = {schoolData.alignment} classList = {schoolData.class} /> }) }}**vs.**render() { { organizationData.map(schoolData => ( <MSCList schoolID = {schoolData.schoolID} schoolName = {schoolData.schoolName} alignment = {schoolData.alignment} classList = {schoolData.class} /> )) }}
```

*   你注意到了吗👀区别？

```
.map(schoolData => { 
```

*   大括号被替换为

```
.map(schoolData => (
```

*   圆括号
*   使用花括号({)会导致**错误** **👊**

![](img/de7fbdf61a9d3826b7ce5f8f8b606e65.png)

Photo by [Howard Riminton](https://unsplash.com/@howier?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)