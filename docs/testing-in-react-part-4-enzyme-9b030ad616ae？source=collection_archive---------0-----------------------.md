# React 中的测试，第 4 部分:酶

> 原文：<https://javascript.plainenglish.io/testing-in-react-part-4-enzyme-9b030ad616ae?source=collection_archive---------0----------------------->

![](img/5c8bc4b45df6fed446d9ccc745de7c8d.png)

Photo by [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/biology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 本文是 React 中测试系列的一部分:
> 
> [React 中的测试，第 1 部分:类型&工具](https://medium.com/javascript-in-plain-english/testing-in-react-part-1-types-tools-244107abf0c6)
> 
> [React 中的测试，第 2 部分:React 测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)
> 
> [React 中的测试，第 3 部分:Jest & Jest-Dom](https://medium.com/javascript-in-plain-english/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b)
> 
> **React 中的测试，第 4 部分:酶**
> 
> [React 中的测试，第 5 部分:使用 Cypress 的端到端测试](https://medium.com/@bryn.bennett/testing-in-react-part-5-end-to-end-testing-with-cypress-bd2bf8d3385f)
> 
> [React 中的测试，第 6 部分:React 测试库、Jest、Enzyme 和 Cypress 的真实测试](https://medium.com/javascript-in-plain-english/testing-in-react-part-6-real-world-testing-with-react-testing-library-jest-enzyme-and-cypress-9c73436d95d8)

我知道我说过我们不打算讨论酶——但是惊喜吧！我们是。虽然我理解测试代码产生的用户体验背后的意图，而不是实际的代码实现，但这两者是不可分割地交织在一起的。正因为如此，我发现 Enzyme 不是 React 测试库的替代品，而是与之配合使用的额外工具，因此值得花一些时间。

# 酶与反应测试库

首先，这两个库有什么不同？我们已经讨论了 RTL(如果你错过了，你可以在这里找到它，但是作为一个快速回顾，RTL 专注于测试代码产生的*体验*。它像用户一样测试代码的呈现。

例如，要提交一个表单，用户可以寻找一个带有文本“submit”的按钮，点击它，然后可能会看到某种呈现给 DOM 的“success”消息。使用 React Testing Library 测试它，您会找到基于“Submit”文本的按钮，单击它，然后查询“success”消息的文本。然而，使用 Enzyme，您将测试“点击”和“成功”消息之间的功能性。

由于这种基础上的分歧，有一个特别重要的对比:酶可以直接测试状态及其操作，而反应测试库不能。这是我选择将 Enzyme 与其他测试工具结合使用的最大原因。有时候你需要直接测试状态，直接测试更新状态。在这样做的过程中，我发现了一些错误和不一致，虽然它们不影响用户体验，但不是好代码，可能会导致负面的 UX 影响。

# 渲染组件

酶测试从实际呈现 React 组件开始，为此，您有三种选择:浅层呈现、完全 DOM 呈现和静态呈现。我们将讨论前两个问题。

## 浅层渲染

> 浅层呈现有助于将组件作为一个单元进行测试，并确保您的测试不会间接断言子组件的行为。

以上来自[酵素文献](https://enzymejs.github.io/enzyme/)。浅层渲染比完全 DOM 渲染重量轻。它应该用于测试范围仅限于被测试组件的测试，并且不需要测试生命周期方法。一般来说，您可以从浅层呈现开始，当它不能满足您的需求时，就转到完整的 DOM 呈现。

```
import { shallow } from 'enzyme';
import MyComponent from './MyComponent';it('renders the component title', () => {
   const wrapper = shallow(<MyComponent />); expect(wrapper.find('.header')).to.have.lengthOf(1);
});
```

## 完全 DOM 渲染

> 完全 DOM 呈现非常适合于这样的用例:您的组件可能会与 DOM APIs 交互，或者需要测试包装在更高级组件中的组件。

完整 DOM 呈现的权重更大，当组件需要访问其自身范围之外的资源时，应该使用完整 DOM 呈现来代替浅层呈现。它更类似于在完整的浏览器环境中进行测试，而不是对单一组件进行单元测试。

```
import { mount } from 'enzyme';
import MyComponent from './MyComponent';it('calls componentDidMount', () => {
   const wrapper = mount(
      <MyComponent
        handleOnChange={handleOnChange}
        foo={bar}
      />
   ); handleOnChange('baz');
   wrapper.update(); expect(wrapper.props().foo).to.Equal('baz');
});
```

# API 参考

在 [Enzyme 文档](https://enzymejs.github.io/enzyme/docs/api/shallow.html)中，你可以找到每种渲染类型的完整调用列表。这些可以用来遍历呈现的组件，断言呈现的组件的某些内容，或者操纵呈现的组件。

## 横越

遍历允许您在组件中找到不同的元素，然后您可以对这些元素进行操作或断言。在这一点上，你应该对 Jest 和 RTL 很熟悉:

*   `[contains(nodeOrNodes)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/contains.html)`:接受一个或多个 DOM 节点，并根据呈现的包装器中是否存在该节点返回一个布尔值。
*   `[children(selector)](https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/children.html)`:可选地接受一个选择器，返回当前包装器的所有子级，或者返回当前包装器的具有给定选择器的所有子级。
*   `[find(selector)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/find.html)`:接受一个选择器，并返回呈现的包装器中具有给定选择器的所有节点。
*   `[findWhere(fn)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/findWhere.html)`:接受谓词函数(返回布尔值的函数)，返回谓词函数为`true`的所有节点。
*   `[getElement()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/getElement.html)`:返回包装后的元素。
*   `[getElements()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/getElements.html)`:返回包装后的元素。
*   `[parent()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/parent.html)`:返回当前包装器的父节点。
*   `[text()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/text.html)`:返回包装组件中呈现的文本字符串。应该谨慎使用，但是可以用于像`contain()`这样的调用。

## 操纵

操纵调用是我发现除了其他工具之外使用 Enzyme 的最有利的理由。这些允许你*改变一些关于渲染组件*实现*的东西*，比如`state`。

*   `[setContext(context)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/setContext.html)`:接受一个上下文对象，设置根组件的上下文，并重新呈现。
*   `[setProps(nextProps[, callback])](https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/setProps.html)`:接受带有新属性和可选回调函数的对象，设置根组件的属性并重新渲染。
*   `[setState(nextState[, callback])](https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/setState.html)` / `state()`:接受具有新状态和可选回调函数的对象，设置根组件的状态并重新呈现。`state()`可以用来访问状态对象。
*   `[simulate(event[, mock])](https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/simulate.html)`:类似于 RTL 的`fireEvent()`，接受一个字符串形式的事件名(即‘click’)和一个可选的模拟对象，模拟包装器根节点上的事件。
*   `[update()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/update.html)`:更新酶组分树以匹配反应组分树。

## 断言

在这一点上，断言也应该看起来很熟悉，并且是关于元素的实际被测试的东西。找到一个元素后，你可以这样写，“它有文本‘欢迎使用我的应用！’".

*   `[contains(nodeOrNodes)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/contains.html)`:接受一个或多个节点，根据包装器是否包含给定的节点返回一个布尔值。
*   `[equals(node)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/equals.html)`:接受一个节点，根据包装器是否等于传入的节点返回一个布尔值。
*   `[hasClass(className)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/hasClass.html)`:接受字符串形式的类名，根据包装的节点是否具有给定的类名返回一个布尔值。
*   `[isEmpty()](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/isEmpty.html)`:如果包装器为空，返回一个布尔值。
*   `[some(selector)](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/some.html)`:接受一个选择器，根据包装器内的任何节点是否包含给定的选择器返回一个布尔值。

## 还有一点

上面有很多关于“被包装的节点”和“包装器”的讨论。明确地说，这可以是从调用返回的任何包装节点——而不仅仅是通常声明并分配给呈现组件的初始`wrapper`变量。这意味着您可以遍历呈现的组件以找到您想要测试的节点，将找到的节点的包装器赋给一个新变量，然后在这个新包装器上使用上面的调用:

```
import { mount } from 'enzyme';
import MyComponent from './MyComponent';it('displays a header with text "Welcome!"', () => {
   const wrapper = mount(<MyComponent />); // Wraps the header node in a new wrapper on which to make calls
   const header = wrapper.find('.header'); expect(header).hasClass('header');
   expect(header).to.equal('<div className="header">Welcome!</div>')
});
```

*现在*我可以说，下一步我们将使用 Cypress 进行端到端测试，之后我们将返回并在真实的应用程序中进行真实的测试实现。

> **之前的**:[React，Part 3: Jest & Jest-Dom](https://medium.com/javascript-in-plain-english/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b) 中的测试
> 
> **下一个**:React 中的[测试，第 5 部分:使用 Cypress 的端到端测试](https://medium.com/@bryn.bennett/testing-in-react-part-5-end-to-end-testing-with-cypress-bd2bf8d3385f)

## 资源

*   [官方酶文档](https://enzymejs.github.io/enzyme/)
*   [使用 Create React 应用程序运行测试](https://create-react-app.dev/docs/running-tests/)