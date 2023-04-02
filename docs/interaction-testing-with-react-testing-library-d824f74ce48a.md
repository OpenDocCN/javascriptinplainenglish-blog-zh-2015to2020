# 使用 React 测试库进行交互测试

> 原文：<https://javascript.plainenglish.io/interaction-testing-with-react-testing-library-d824f74ce48a?source=collection_archive---------3----------------------->

## React 组件测试的窗口

![](img/c4fb5d998fd8a36cb58f547a7a359dec.png)

Photo by [David Travis](https://unsplash.com/@dtravisphd) on [Unsplash](https://unsplash.com/)

# 我的旅程

测试很复杂。我从来都不擅长这个。很长时间以来，我只关注基本的功能输入输出单元测试。为什么？因为它们很简单——不需要呈现 HTML，不需要查询 DOM 元素，不需要与上述 DOM 元素交互。但是当然，React 组件测试对于任何成熟的代码库都是必要的。终于到了我坐下来想清楚的时候了。

这时我发现了 [React 测试库](https://testing-library.com/docs/intro)。突然间，一切似乎变得简单多了。所有我遇到的，但不理解的，让我推迟 React 组件测试的复杂性都消失了。希望同样的事情也会发生在你身上。

# 交互测试

顾名思义，交互测试测试与 React 组件的交互。它可以被认为是 React 组件的单元测试。您的测试将假装是用户——通过输入内容、点击按钮等与组件交互——并检查应该发生的事情是否发生。

以这个简单的`Counter`组件为例。

```
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      {
        count > 0 && (
          <button onClick={() => setCount(0)}>
            Decrement
          </button>
        )
      }
    </div>
  );
}
```

用户可以与该组件进行两次交互——递增计数器或递减计数器(如果`count`大于 0)。单击相应的按钮后，组件将显示新值。

我们将使用这个示例组件来介绍如何使用 React 测试库进行交互测试。

# 翻译

因此，让我们开始了解 React 测试库提供的工具。React 组件测试的第一个基础是渲染。这就像调用 React 测试库中包含的`render`函数一样简单。

```
import { render } from '[@testing](http://twitter.com/testing)-library/react';test('render', () => {
  render(<Counter />);
});
```

瞧！就是字面意思。说真的。谁知道呢？

# 询问

好了，第一关已经过了。现在我们已经呈现了我们的组件，我们必须得到 HTML 元素进行交互。幸运的是， [React 测试库通过`render`函数响应提供了许多简单而整洁的查询](https://testing-library.com/docs/dom-testing-library/api-queries)。在我们的测试中，`getByText`会帮助查询供我们点击的按钮。

```
test('queries existence', () => {
  const { getByText } = render(<Counter />);
  const increment = getByText('Increment');
});
```

如果呈现的话，可以做类似的事情来获得减量按钮。如果 not(当`count`为 0 时)，`getByText('Decrement')`将抛出一个错误，导致测试自动失败，即使我们还没有测试任何东西！在这种情况下，我们可以使用`queryByText`来尝试和查询按钮。如果找不到元素，`queryByText`将返回`null`。

```
test('queries non-existence', () => {
  const { queryByText } = render(<Counter />);
  const decrement = queryByText('Decrement');
});
```

# 点火事件

是时候互动了！React 测试库提供了一个`fireEvent`函数，包括对几乎所有 DOM 事件的[支持——键盘、鼠标、动画等。因为我们已经为增量和减量按钮定义了`onClick`，我们将使用 click 事件。](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js)

```
import { fireEvent, render} from '[@testing](http://twitter.com/testing)-library/react';test('fireEvent', () => {
  const { getByText } = render(<Counter />);
  const increment = getByText('Increment');
  fireEvent.click(increment);
});
```

触发正确的事件很重要——否则，您期望的交互将不会发生。触发的事件必须与附加到元素的事件侦听器类型相同。否则，将不会触发事件监听器。在我们的`Counter`组件中，按钮附带了`onClick`事件监听器，因此只会被点击事件触发。这与浏览器实现不同，在浏览器实现中，`click`事件也会触发`mousedown`、`mouseup`和其他事件。

# 验证

恭喜你，你现在是 React 测试库的专家了！渲染、查询和触发事件几乎是您真正需要了解的所有具体的 React 测试库工具。

等等，但是我们的测试还没有完成！是的，但是现在您已经学会了完成我们的测试所需的所有工具。

让我们回顾一下我们需要处理的交互:

*   点击增量按钮，我们的计数器将增加
*   点击减量按钮，我们的计数器将减少

我们现在知道了如何触发按钮上的 click 事件，但是我们如何验证我们的 counterchanged 是否如预期的那样？更多查询！

```
test('validation existence', () => {
  const { getByText } = render(<Counter />);
  const increment = getByText('Increment');
  fireEvent.click(increment);
  expect(getByText('Count: 1')).toBeTruthy();
  const decrement = getByText('Decrement');
  fireEvent.click(decrement);
  expect(getByText('Count: 0')).toBeTruthy();
});
```

就像我们如何查询按钮一样，我们可以查询组件来查看我们期望的更新是否发生了。在我们点击 increment 按钮之后，我们的组件被更新以显示新的`count`。通过查询我们期望的新计数值，我们可以检查它的存在，以确定`Counter`是否如预期的那样运行。

太好了！我们刚刚进行了测试，以确保按钮按预期工作。但是，对于我们的`Counter`组件的完整测试覆盖，我们还需要确保减量按钮仅在`count`大于 0 时呈现。使用同样的技术，但是使用`queryByText`，我们可以测试这个。

```
test('validation non-existence', () => {
  const { getByText, queryByText } = render(<Counter />);
  expect(queryByText('Decrement')).toBeNull()
  const increment = getByText('Increment');
  fireEvent.click(increment);
  expect(queryByText('Decrement')).toBeTruthy()
});
```

我们首先通过验证`queryByText`响应为空来确保减量按钮不存在。然后，我们递增计数器并验证递减按钮是否存在。

就是这样！我们的`Counter`组件已经过全面测试，我们可以非常自信地说，它完全符合预期。

# 屏幕记录

哦等等！React 测试库还提供了一个更有价值的工具:`screen.debug`。这个函数将记录 DOM 结构。默认情况下，它会记录`document.body`。

```
import { render, screen } from '[@testing](http://twitter.com/testing)-library/react';test('debug default', () => {
  render(<Counter />);
  screen.debug();
  // output:
  //   <body>
  //     <div>
  //       <div>
  //         <p>
  //           Count:
  //           0
  //         </p>
  //         <button>
  //           Increment
  //         </button>
  //       </div>
  //     </div>
  //   </body>
});
```

您还可以传入一个 DOM 元素来专门记录这一点。

```
import { fireEvent, render, screen } from '[@testing](http://twitter.com/testing)-library/react';test('debug element', () => {
  const { getByText } = render(<Counter />);
  const increment = getByText('Increment');
  screen.debug(increment);
  // output:
  //   <button>
  //     Increment
  //   </button>
});
```

使用这个工具，您可以直观地检查和理解组件的 DOM 结构，从而可以制定测试组件所需的查询。

# 最后的想法

React Testing Library 极大地简化了，并且，由于缺乏更好的术语，把它简化成你我都能理解和使用的东西。它提供的四个工具——渲染、查询、触发事件和屏幕记录——涵盖了交互测试的基础。既然您已经了解了这一点，那么就出去确保您的 React 组件没有错误！

# 资源

*   [官方 React 测试库文档](https://testing-library.com/docs/react-testing-library/intro)
*   [本文 Github repo](https://github.com/mjchang/medium/tree/master/interaction-testing)
*   [本文的 code sandbox](https://codesandbox.io/s/github/mjchang/medium/tree/master/interaction-testing?file=/src/demo.test.js)