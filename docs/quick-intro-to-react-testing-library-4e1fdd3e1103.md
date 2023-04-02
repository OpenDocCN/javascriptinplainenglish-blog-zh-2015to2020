# React 测试库简介

> 原文：<https://javascript.plainenglish.io/quick-intro-to-react-testing-library-4e1fdd3e1103?source=collection_archive---------5----------------------->

## 随着 web 应用程序的规模和复杂性的增加，为 web 应用程序编写测试变得至关重要

[React 测试库(RTL)](https://github.com/testing-library/react-testing-library) 作为 Airbnb 的酵素替代品发布。与酶不同，React 测试库后退一步，询问我们“如何测试 React 组件以获得对 React 组件的完全信任”。React 测试库不是测试组件的实现细节，而是将开发人员置于 React 应用程序最终用户的角度。

![](img/391e95656e6ff412def38ffb2c24d8b5.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Jest Vs React 测试库

Jest 和 React 测试库都是在测试 React 组件时使用的，但大多数人都将 RTL 混淆为 Jest 的替代品。这是完全不正确的，因为他们需要对方，他们都有自己的具体任务。 [Jest](https://jestjs.io/) 是一个 JavaScript 测试运行器和 JavaScript 库，用于创建、**运行**和**结构化测试**。

默认情况下，create-react-app 附带 Jest 和 react 测试库设置。如果您使用定制的 React 设置，您将不得不自己设置 Jest 和 React 测试库。

```
describe('check truthy and falsy values', () => {
  test('true is truthy', () => {
    expect(true).toBe(true);
 }); test('false is falsy', () => {
    expect(false).toBe(false);
 });
});
```

编写 Jest 测试非常简单。如果我们看上面的例子，描述块是**测试套件。**该试块(也可命名为`it`而非`test`)为**测试用例**。一个测试套件可以有多个测试用例，一个测试用例不一定总是在一个测试套件中。我们编写**断言**(例如`expect`开玩笑)来验证输出是否与我们的异常匹配，异常要么是成功的(绿色)，要么是错误的(红色)。

总之，Jest 是一个测试运行程序，它让您能够从命令行运行测试。框架提供的远不止这些(例如，间谍、模拟、存根等。);但本质上，这就是现在理解我们为什么首先需要 Jest 所需要的一切。目前还没有关于 React 组件的任何信息。

另一方面，React 测试库专门用于测试 React 组件。如上所述，它是酶的替代品，而不是 Jest。

## 使用 React 测试库渲染组件

让我们看看如何在测试中使用 RTL 渲染 react 组件。让我们尝试在下面的应用程序组件上使用 RTL。

```
import React from 'react';const title = 'Hello React';function App() { return <div>{title}</div>;
}export default App;
```

并在 *App.test.js* 文件中进行测试

```
import React from 'react';
import { render } from '@testing-library/react';
import App from './App';describe('App', () => {
    test('renders App component', () => {
    render(<App />);
  });
});
```

RTL 有自己的渲染函数，可以用来渲染你的组件，渲染组件的最终输出，这是你可以用来测试的。

```
import React from 'react';
import { render, screen } from '@testing-library/react';import App from './App';describe('App', () => {
    test('renders App component', () => {
    render(<App />);
    screen.debug();
  });
});
```

使用 npm test 命令运行测试后，您应该会看到应用程序的 HTML 输出。`screen.debug()` 方法本质上是`console.log(prettyDOM())`的捷径。它支持调试文档、单个元素或元素数组。每当您使用 React 测试库为组件编写测试时，您可以首先呈现组件，然后在测试中调试 RTL 渲染器的可见内容。这样，您可以更自信地编写测试。

```
<body>
  <div>
    <div>
      Hello React
    </div>
  </div>
</body>
```

React 测试库的伟大之处在于它不太关心实际的组件。由于它只关心使用不同 React 特性( [useState](https://www.robinwieruch.de/react-usestate-hook) 、[事件处理程序](https://www.robinwieruch.de/react-event-handler)、 [props](https://www.robinwieruch.de/react-pass-props-to-component) )和概念([受控组件](https://www.robinwieruch.de/react-controlled-components))的渲染组件的输出，所以在渲染最终输出时没有区别。

React 测试库用于像人一样与 React 组件进行交互。人们看到的只是 React 组件呈现的 HTML，所以这就是为什么您将这个 HTML 结构视为输出，而不是两个单独的 React 组件。

## 选择元素

在你渲染了你的 React 组件之后，RTL 为你提供了不同的搜索功能来抓取元素。然后，这些元素被用于断言或用户交互。

```
import React from 'react';
import { render, screen } from '@testing-library/react';
import App from './App';describe('App', () => {
    test('renders App component', () => {
    render(<App />);
    screen.getByText('Search:');
  });
});
```

使用 screen.debug()函数，我们可以清楚地看到最终输出中出现了哪些元素。在你知道了 HTML 的结构之后，你可以开始使用 RTL 的 screen object 函数来选择元素。然后，所选元素可以用于用户交互或断言。我们将做一个断言来检查元素是否在 DOM 中。

```
import React from 'react';
import { render, screen } from '@testing-library/react';
import App from './App';describe('App', () => {
    test('renders App component', () => {
    render(<App />);
    expect(screen.getByText('Search:')).toBeInTheDocument();
  });
});
```

`getByText`函数接受一个字符串作为输入，就像我们现在使用的一样，但也接受一个正则表达式。字符串参数用于精确匹配，而正则表达式可用于部分匹配，这通常更方便。

类似于`getByText`函数，React 测试库提供了以下函数来选择最终 HTML 中的元素:

*   getByText
*   getByRole
*   getByLabelText
*   getByPlaceholderText
*   getByAltText
*   getByDisplayValue

*查询方式*变体:

*   queryByText
*   queryByRole
*   queryByLabelText
*   queryByPlaceholderText
*   queryByAltText
*   queryByDisplayValue

按变量查找:

*   findByText
*   findByRole
*   findByLabelText
*   findByPlaceholderText
*   findByAltText
*   findByDisplayValue

## React 测试库:触发事件

我们已经看到了如何使用 React 测试库来渲染和搜索元素，以检查它是否已经被渲染。但是用户交互呢？React 用于构建面向大量用户交互的动态 web 应用程序，因此基于用户交互事件测试 React 组件是必要的。

有两种方法可以使用 RTL 在渲染组件上激发事件。第一种方法是使用 fireEvent()函数来模拟最终用户的交互。让我们看看这对于我们的输入字段是如何工作的:

```
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import App from './App';describe('App', () => {
    test('renders App component', () => {
    render(<App />);
    screen.debug();

    fireEvent.change(screen.getByRole('textbox'), {
    target: { value: 'JavaScript' },
  }); screen.debug();
 });
});
```

fireEvent()函数有两个参数，一个元素(这里是文本框角色的输入字段)和一个事件(这里是值为“JavaScript”的事件)。

测试组件上用户交互的第二种方法是使用扩展的用户事件库，它构建在 React 测试库本身附带的 fireEvent 之上。userEvent API 比 fireEvent API 更能模拟实际的浏览器行为。例如，`fireEvent.change()`仅触发一个`change`事件，而`userEvent.type`触发一个`change`事件，还会触发`keyDown`、`keyPress`和`keyUp`事件。

```
import React from 'react';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import App from './App';describe('App', () => {
    test('renders App component', async () => {
    render(<App />);await screen.findByText(/Signed in as/); expect(screen.queryByText(/Searches for JavaScript/)).toBeNull(); await userEvent.type(screen.getByRole('textbox'), 'JavaScript'); expect( screen.getByText(/Searches for    JavaScript/)).toBeInTheDocument();
  });
});
```

## 结论

React 测试库通过测试最终的输出行为而不是组件的内部逻辑来简化测试过程。通过触发事件，您不仅可以测试呈现的输出，还可以测试组件的变化。