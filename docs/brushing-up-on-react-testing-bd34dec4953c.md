# 复习 React:测试

> 原文：<https://javascript.plainenglish.io/brushing-up-on-react-testing-bd34dec4953c?source=collection_archive---------10----------------------->

## 最常见的 React 测试模式的快速示例。

*正在进行的系列的一部分* [*复习 React*](https://medium.com/@mdazmainamin/brushing-up-on-react-basics-18ad67528b85) *…*

![](img/07c3e63c2c52904b03f3a8b57bb235c2.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

我假设您已经有了一个安装了所有工具的测试环境:karma + webpack 和 enzyme。

如果没有，这些是很好的资源:

*   [如何测试与 Jest &酶的反应](https://www.robinwieruch.de/react-testing-jest-enzyme)
*   [使用带有因果报应的酶](https://airbnb.io/enzyme/docs/guides/karma.html)

# 基本测试(测试状态和道具)

我们想要测试 react 组件指南[中定义的按钮组件。](https://medium.com/@mdazmainamin/brushing-up-on-react-basics-18ad67528b85)

```
import React from 'react';
import { shallow } from 'enzyme';
import Button from './Button';

function render(props) {
	return shallow();
}

describe("Button component tests", () => {
    it("renders the prop correctly", () => {
        props = {
            buttonName: 'mockButton'
        };

        const renderedComp = render(props);

        expect(renderedComp.props().buttonName).toEqual(props.buttonName);
    });

    it("has correct initial state", () => {
        const renderedComp = render({});
        expect(renderedComp.state().numOfClicks).toEqual(0);
    });
});
```

# 测试用户交互

```
it("increase numOfClick by 1 when button is clicked", () => {
    const renderedComp = render({});
    expect(renderedComp.state().numOfClicks).toEqual(0);

    const button = renderedComp.find('button');
    button.simulate('click');

    expect(renderedComp.state().numOfClicks).toEqual(1);    
});
```

总是尽可能模拟用户交互，比如模拟点击。

# 测试类方法是否被调用

```
it("increase numOfClick by 1 when button is clicked", () => {
    const functionToBeCalled = spyOn(Button.prototype, 'handleClick')

    const renderedComp = render({});
    const button = renderedComp.find('button');
    button.simulate('click');

    expect(functionToBeCalled).toHaveBeenCalled();    
});
```

# 要记住的事情

*   您可以嵌套`describe`块。你的描述块应该像一个测试套件。
*   每个`it`模块应该测试一项功能。
*   使用`beforeAll()`和`beforeEach()`。
*   `shallow`渲染可以完成 90%的任务。但是，有时候你需要使用`mount`在测试中完全安装你的 react 组件。要了解更多关于他们的差异，请访问这里。