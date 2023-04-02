# React 组件中的提升状态

> 原文：<https://javascript.plainenglish.io/react-components-lifting-state-up-59175b9efc9e?source=collection_archive---------8----------------------->

![](img/15c74a611bdad594b1ed2e5987392311.png)

Photo by [Daniel Frank](https://unsplash.com/@fr3nks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将研究提升状态的原理。

# 提升状态

我们应该把任何共享的状态提升到它们最接近的共同祖先。

这样，一个状态可以通过 props 传递给多个子组件来共享。

例如，如果我们想构建一个用于转换长度的计算器，我们可以编写以下代码:

```
import React from "react";
import ReactDOM from "react-dom";class LengthInput extends React.Component {
  constructor(props) {
    super(props);
    const { length } = this.props;
    this.state = { length };
  }
  handleChange(e) {
    this.props.onChange(e);
  } componentWillReceiveProps(props) {
    const { length } = props;
    this.setState({ length });
  } render() {
    const { unit } = this.props;
    return (
      <>
        <label>Length ({unit})</label>
        <input
          value={this.state.length}
          onChange={this.handleChange.bind(this)}
          placeholder={this.props.unit}
        />
        <br />
      </>
    );
  }
}class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { lengthInMeter: 0, lengthInFeet: 0 };
  } handleMeterChange(e) {
    this.setState({ lengthInMeter: +e.target.value });
    this.setState({ lengthInFeet: +e.target.value * 3.28 });
  } handleFeetChange(e) {
    this.setState({ lengthInFeet: +e.target.value });
    this.setState({ lengthInMeter: +e.target.value / 3.28 });
  } render() {
    return (
      <div className="App">
        <LengthInput
          length={this.state.lengthInMeter}
          onChange={this.handleMeterChange.bind(this)}
          unit="meter"
        />
        <LengthInput
          length={this.state.lengthInFeet}
          onChange={this.handleFeetChange.bind(this)}
          unit="feet"
        />
      </div>
    );
  }
}const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

在上面的代码中，我们有一个长度转换器，当我们在米字段中键入时，它将米转换为英尺，反之亦然。

我们所做的是将长度保存在`App`组件中。这就是为什么这个原理被称为提升状态。

因为我们在`App`组件中保留了长度，所以我们必须将它们传递给`LengthInput`组件。

为此，我们向他们传递道具。此外，我们传入单元，并将更改处理程序函数传递给我们的`LengthInput`组件，这样它们就可以更新`App`组件中的状态。

在`handleFeetChange`和`handleMeterChange`功能中，我们根据`LengthInput`组件中输入的值设置状态。

我们在两个函数中调用`this.setState`来设置状态。每次调用`setState`时，都会调用`render`，以便将最新状态传递给我们的`LengthInput`组件。

在`LengthInput`组件中，我们有`componentWillReceiveProps`钩子，它将获得最新的属性值，然后相应地设置`length`状态。

`this.state.length`设置为`LengthInput`中`input`元素的值，因此将显示输入的值。

将状态提升到父组件的好处是我们只需要保存状态的一个副本。此外，我们不必在不同的子组件中重复处理状态。

输入值保持同步，因为它们是从同一状态计算的。

![](img/715283a7234b79ec3efd7e207c08e713.png)

Photo by [Daniel Frank](https://unsplash.com/@fr3nks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

当编写 React 应用程序时，建议我们将子组件之间的共享状态移动到它们的父组件中。这叫做提升状态。

我们希望这样做，这样我们就不必重复处理子组件中的数据，并且我们只有一个真实的来源。

为此，我们可以将数据和功能作为道具传递给子组件。这样，父组件中的函数可以从子组件中调用。

然后用从 props 传来的最新值更新子组件的状态，我们可以使用`componentWillReceiveProps`钩子从 props 更新状态。