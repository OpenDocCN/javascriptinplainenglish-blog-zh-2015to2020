# 反应提示-获取窗口尺寸并创建下拉列表

> 原文：<https://javascript.plainenglish.io/react-tips-getting-window-dimensions-and-creating-dropdowns-55231ad49952?source=collection_archive---------8----------------------->

![](img/a876fa1698832710478bdc8d5c47cd12.png)

Photo by [Lucija Ros](https://unsplash.com/@lucija_ros?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个流行的创建网络应用和移动应用的库。

在本文中，我们将了解一些编写更好的反应应用程序的技巧。

# 使用反应下拉更改事件

我们可以将事件处理程序传递给`onChange`道具，将状态设置为所选下拉列表的值。

例如，我们可以写:

```
import React, { Component, Fragment } from 'react';class Select extends Component{
  constructor(){
    this.state = {
      options: [
        {
          name: 'Select…',
          value: null,
        },
        {
          name: 'Foo',
          value: 'foo',
        },
        {
          name: 'Bar',
          value: 'bar',
        },
        {
          name: 'Baz',
          value: 'baz',
        },
      ],
      value: '',
    }
  }; handleChange = (event) => {
    this.setState({ value: event.target.value });
  }; render() {
    const { options, value } = this.state; return (
      <>
        <select onChange={this.handleChange} value={value}>
          {options.map(item => (
            <option key={item.value} value={item.value}>
              {item.name}
            </option>
          ))}
        </select>
      </>
    );
  }
}
```

我们从`this.state`得到`options`和`values`。

我们在`render`方法中渲染一个下拉列表。

我们映射`options`并将它们呈现为选项元素，其中`value`属性是值，而`item.name`被显示。

`handleChange`方法调用带有`event.target.value`的`setState`，该值被选中。

在功能组件中，我们可以使用`useState`钩子来获取和设置值。

例如，我们可以写:

```
const Select = ({
  options
}) => {
  const [selectedOption, setSelectedOption] = useState(options[0].value); return (
    <select
      value={selectedOption}
      onChange={e => setSelectedOption(e.target.value)}>
        {options.map(o => (
          <option value={o.value}>{o.name}</option>
      ))}
    </select>
  );
};
```

我们调用`useState`钩子返回分配给`selectedOption`变量的最新状态。

`setSelectedOption`是设置状态的功能。

`options`从道具传入，我们将它的第一个值设置为初始值。

然后我们用从`options`数组道具创建的选项返回一个 select 元素。

我们传入一个回调到`onChange`以将选项设置为我们选择的选项。

# 在“反应”中获取视口或窗口高度

如果我们有一个类组件，我们可以在我们的组件生命周期方法中倾听`resize`事件。

例如，我们可以写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { width: 0, height: 0 };
    this.updateWindowDimensions = this.updateWindowDimensions.bind(this);
  }

  componentDidMount() {
    this.updateWindowDimensions();
    window.addEventListener('resize', this.updateWindowDimensions);
  }

  componentWillUnmount() {
    window.removeEventListener('resize', this.updateWindowDimensions);
  }

  updateWindowDimensions() {
    this.setState({ width: window.innerWidth, height: window.innerHeight });
  }

  render(){
    //...
  }
}
```

我们在构造器中设置了初始的`width`和`height`。

然后在`componentDidMount`中，我们添加了带有`addEventListener`的事件侦听器，并传入我们的`updateWindowDimensions`作为事件处理方法。

该方法只是通过从`window`中获取`innerWidth`和`innerHeight`属性来设置窗口的宽度和高度，并用`setState`进行设置。

当组件卸载时，我们调用`removeEventListener`来移除`resize`监听器。

如果我们创建一个函数组件，我们可以创建自己的钩子来获取窗口尺寸。

例如，我们可以写:

```
import { useState, useEffect } from 'react';function getWindowDimensions() {
  const { innerWidth: width, innerHeight: height } = window;
  return {
    width,
    height
  };
}function useWindowDimensions() {
  const [windowDimensions, setWindowDimensions] = useState(getWindowDimensions()); useEffect(() => {
    function handleResize() {
      setWindowDimensions(getWindowDimensions());
    } window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []); return windowDimensions;
}const Component = () => {
  const { height, width } = useWindowDimensions(); return (
    <div>
      width: {width} ~ height: {height}
    </div>
  );
}
```

我们有`useWindowDimensions`钩子和`getWindowDimensions`辅助函数来获取窗口或视口的宽度和高度。

在钩子中，我们使用`useState`钩子返回最新状态，使用`setWindowDimensions`方法设置维度。

在`useEffect`钩子中，我们调用了`setWindowDimensions`函数来设置窗口调整大小时的宽度和高度。

我们通过添加一个带有`addEventListener`的事件监听器来监听`resize`事件，就像我们在前面的例子中所做的那样。

我们的清理代码在我们在`useEffect`回调中返回的函数中。

我们像在前面的例子中那样调用了`removeEventListener`。

`useEffect`的第二个参数是一个空数组，所以`useEffect`回调只在钩子加载时运行。

尺寸的最新值在钩子中返回。

我们将大部分逻辑放在钩子中，这样我们就可以使用自定义钩子来获得窗口的宽度和高度。

![](img/74ffcdd5cf8ed4dcbed0839442bc6083.png)

Photo by [Chen Yi Wen](https://unsplash.com/@yiwen0316?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在下拉列表中添加一个更改监听器，以获取最新选择的值，并将其设置为状态。

有多种方法可以观察组件的宽度和高度变化。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**