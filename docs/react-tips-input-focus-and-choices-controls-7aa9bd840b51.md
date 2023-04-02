# 反应提示—输入焦点和选择控件

> 原文：<https://javascript.plainenglish.io/react-tips-input-focus-and-choices-controls-7aa9bd840b51?source=collection_archive---------3----------------------->

![](img/60e3ded3320738d9b7b79971e8f95a01.png)

Photo by [Cenk Batuhan Özaltun](https://unsplash.com/@c_b_ozaltun?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是构建现代交互式前端 web 应用最常用的前端库。它还可以用来构建移动应用程序。

在本文中，我们将了解如何将输入和复选框和单选按钮的值绑定到状态。

# 渲染后将焦点设置在输入上

为了聚焦一个输入，我们必须使用原生 DOM 元素`focus`方法。该方法可用于输入元素，因此我们可以调用它。

当组件渲染时，我们可以使用`useEffect`钩子来运行一些东西。如果我们传入一个空数组作为第二个参数，那么我们传入`useEffect`的回调只在组件第一次加载时运行。

例如，我们可以编写以下代码来实现这一点:

```
import React from "react";export default function App() {
  const input = React.createRef();
  React.useEffect(() => input.current.focus(), []);
  return (
    <div className="App">
      <input ref={input} />
    </div>
  );
}
```

在上面的代码中，我们有用`createRef`方法创建的`useEffect`钩子和`input` ref，它们被传递到输入的`ref`道具中。

然后在`useEffect`回调中，我们调用`input.current.focus()`来调用我们 input 元素的`focus`方法。

最后，当我们加载页面时，我们会看到当`App`如我们所愿加载时，输入被聚焦。

# 检验盒

要创建复选框，我们必须将复选框值设置为`checked`属性的值。然后`onChange`处理程序将获取检查的值，然后将其设置为我们作为`checked`属性的值传入的状态。

例如，我们可以编写以下代码来实现这一点:

```
import React from "react";export default function App() {
  const [selected, setSelected] = React.useState({});
  const handleInputChange = e => {
    const { name, checked } = e.target;
    setSelected(selected => ({
      ...selected,
      [name]: checked
    }));
  };
  return (
    <div className="App">
      <form>
        <label>
          apple:
          <input
            name="apple"
            type="checkbox"
            checked={selected.apple || false}
            onChange={handleInputChange}
          />
        </label>
        <label>
          orange:
          <input
            name="orange"
            type="checkbox"
            checked={selected.orange || false}
            onChange={handleInputChange}
          />
        </label>
        <label>
          grape:
          <input
            name="grape"
            type="checkbox"
            checked={selected.grape || false}
            onChange={handleInputChange}
          />
        </label>
      </form>
      <p>
        {Object.keys(selected)
          .filter(k => selected[k])
          .join(", ")}
      </p>
    </div>
  );
}
```

让复选框正常工作是很棘手的。在我们的`handleInputChange`函数中，我们必须确保`e.target`的`name`和`value`属性必须从同步上下文中访问，所以它不能在我们传递给`setSelected`的回调中。如果我们不这样做，我们会得到一个警告，说“由于性能原因，这个合成事件被重用”

在`setSelected`函数中，我们扩展了`selected`的现有属性，然后将`e.target`中的`name`和`checked`值更新到`setSelected`回调中返回的对象中。

`name`值来自每个复选框的`name`属性的值。

为了消除“在 ReactJS 中，一个组件正在将一个文本类型的非受控输入更改为受控输入”的错误，我们必须为每个`checked`属性设置一个默认值。在上面的代码中，我们将它们设置为`false`。

![](img/3c46e279bd1d4dacab90453ec672750e.png)

Photo by [Naadir Shahul](https://unsplash.com/@naadirshah?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 单选按钮

单选按钮类似于复选框，但一次只能选中一个组中的一个单选按钮。例如，我们可以编写以下代码，将单选按钮的选中值绑定到如下状态:

```
import React from "react";export default function App() {
  const [selected, setSelected] = React.useState("");
  const handleInputChange = e => {
    setSelected(e.target.value);
  };
  return (
    <div className="App">
      <form>
        <label>
          apple:
          <input
            name="fruit"
            type="radio"
            value="apple"
            checked={selected === "apple"}
            onChange={handleInputChange}
          />
        </label>
        <label>
          orange:
          <input
            name="fruit"
            type="radio"
            value="orange"
            checked={selected === "orange"}
            onChange={handleInputChange}
          />
        </label>
        <label>
          grape:
          <input
            name="fruit"
            type="radio"
            value="grape"
            checked={selected === "grape"}
            onChange={handleInputChange}
          />
        </label>
      </form>
      <p>{selected}</p>
    </div>
  );
}
```

在上面的代码中，我们有设置为表达式的`checked`属性，该表达式检查`selected`属性中的值是否设置为给定值。

然后在`handleInputChange`函数中，我们可以通过写`setSelected(e.target.value)`来调用`setSelected`，因为单选按钮集`e.target.value`被设置为单选按钮的值。

因此，当我们单击单选按钮时，将显示`selected`值，并且我们还会看到，当我们单击单选按钮时，单选按钮会改变选择。

`checked`属性控制选择哪个单选按钮的渲染，p 标签中显示的值用`handleInputChange`函数更新。

# 结论

在 React 中设置复选框的值很复杂。如果我们没有在复选框的`checked`属性中设置默认值，我们将在控制台中得到警告。如果我们在状态改变函数的回调中访问`e.target`,我们也会得到警告。

在 React 中单选按钮更容易处理，因为我们只需在单击单选按钮时将值设置为字符串。

聚焦输入很容易，因为我们可以将一个 ref 附加到一个输入上，然后对它调用`focus`。

## 来自简明英语团队的说明

简明英语刚刚推出了一个 YouTube 频道！我们希望您现在就通过 [**订阅来支持我们！**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)