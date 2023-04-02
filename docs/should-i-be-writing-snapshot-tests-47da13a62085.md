# 何时编写笑话快照测试

> 原文：<https://javascript.plainenglish.io/should-i-be-writing-snapshot-tests-47da13a62085?source=collection_archive---------0----------------------->

## 从哪里开始，知道何时使用它们

![](img/22f88b4ae08e4639cab0c245642c8888.png)

Photo by [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 当我在研究测试框架，特别是那些与 React 集成的框架时，Jest 是一个明显的选择。

Jest 的特征之一是一种称为快照测试的新工具。起初，这听起来很有趣，但在做了更多的研究并看到开发者的许多不同意见后，我变得谨慎起来。

以下是我们团队对快照测试的看法以及我们从经验中学到的东西:

*   什么时候写，
*   为什么要写它们，
*   如何利用它们来防止退化？

![](img/62c3b04466cedc0deb052741f013c89b.png)

React + Jest = a match made in heaven!

# 快照测试是 Jest 的一个特性，它允许您测试 Javascript 对象。

它们与 React 组件配合良好，因为当您渲染组件时，您可以查看 DOM 输出，并在测试运行时创建*“快照”*。这些类型的测试有助于防止回归，因为如果有任何东西导致组件改变，这个测试就会抓住它。根据我们团队的经验，当您可以将道具传递给功能组件并测试有限数量的案例时，快照测试是最有效的。

## 何时写入快照测试

*   如果组件不经常更新
*   如果一个组件不是太复杂
*   如果很容易看到您实际测试的内容

这些规则非常模糊，但应该可以很快帮助您决定快照测试是否合适。

如果您有一个测试经常更新的组件的快照测试，那么您需要不断更新附带的测试。如果您养成了只更新这些测试而不仔细检查输出文件的坏习惯，这些测试就会失去它们的价值。

如果一个组件太复杂，就很难确定实际测试的是什么。如果在渲染之前有大量的业务逻辑和条件语句，那么没有使用该功能的人怎么知道发生了什么呢？

编写快照测试时要记住的最重要的规则是给测试命名。如果您不能轻松准确地命名您的测试，很可能它不应该是快照测试。

# 快照测试示例

**spinner.spec.js** 使用[反应-测试-库](https://medium.com/@chrisgirard/testing-components-with-jest-and-react-testing-library-d36f5262cde2)

```
// Spinner component
const Spinner = props => (
 props.loading ?
  <div className={`icon icon-spin text-center ${props.size} ${props.color}} />
  : null)

export default Spinner;// Spinner spec
describe('Spinner snapshot test ', () => {
    it('should render a large blue spinner', () => {
        const props = {
            loading: true,
            size: "large",
            color: "blue"
        };const container = render(<Spinner {...props} />);
        expect(container.firstChild).toMatchSnapshot();
    })
})
```

**spinner.spec.js.snap**

```
// Jest Snapshot v1exports[`<Spinner /> Snapshot Tests renders as expected 1`] = `
<div
  class="icon icon-spin icon-large icon-blue"
/>
```

这是呈现微调器组件的快照测试的简单示例。查看。捕捉输出文件并看到一个清晰的测试名称，您可以很容易地看到我们期望渲染一个大的蓝色微调器组件。

在这种情况下，快照测试的好处是，如果有人将微调器从 div 更改为 span，测试将失败。如果我们没有编写快照测试，我们可能只会断言大小和颜色应用正确。下面是两个快照断言的示例。如果加载是假的，那么我们不应该渲染组件并返回 null。

**spinner.spec.js**

```
it('should render a large blue spinner', () => {
    const props = {
        loading: true,
        size: "large",
        color: "blue"
    };const container = render(<Spinner {...props} />);
    expect(container.firstChild).toMatchSnapshot();
})it('should return null if loading is false', () => {
    const props = {
        loading: false
    };const container = render(<Spinner {...props} />);
    expect(container.firstChild).toMatchSnapshot();
})
```

**spinner.spec.js.snap**

```
// Jest Snapshot v1exports[`<Spinner /> should render a large blue spinner 1`] = `
<div
  class="icon icon-spin icon-large icon-blue"
/>;exports[`<Spinner /> should return null if loading is false 1`] = `
null;
```

看 spinner.spec.js，不清楚测试的是什么。这就是为什么验证 spinner.spec.js.snap 文件并检查输出如此重要。

# 更新快照测试

我们第一次创建快照测试时，它会创建一个. snap 引用文件，并且每次都针对这个文件运行您的测试。如果我们的组件被更改，我们必须手动更新快照测试。

```
jest --updateSnapshot
```

## 走向

希望您能从我们的快照测试经验中获得一些要点和建议。当将这种类型的测试引入到您自己的项目中时，请记住:

*   创建并遵循您自己的关于何时编写快照测试的规则
*   验证组件是否在输出文件中正确呈现
*   在代码审查中要严格

我们的团队使用 [React 测试库](https://medium.com/@chrisgirard/testing-components-with-jest-and-react-testing-library-d36f5262cde2)，我们在测试中不使用浅层渲染。当编写快照测试时，如果我们的父组件有许多子组件，那么这些子组件也会被渲染。一方面，我们创建了一个测试来快速验证整个组件是否被渲染，另一方面，由我们来检查组件是否被正确渲染。

通过这个过程，我们的团队在审查这些类型的测试时变得更加严格。我们确切地看看发生了什么变化及其原因。我们倾向于编写快照测试，作为共享 React 组件的*总括*。

拍照快乐！📸