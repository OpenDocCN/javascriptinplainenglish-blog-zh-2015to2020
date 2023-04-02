# 反应最佳实践—组件和测试

> 原文：<https://javascript.plainenglish.io/react-best-practices-components-and-testing-106714b615cf?source=collection_archive---------6----------------------->

![](img/d44b8eb226cad4f95c56bdc492603af5.png)

Photo by [Monty Allen](https://unsplash.com/@monty_a?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时应该遵循的一些最佳实践。

# 组件名称使用大写字母

我们应该以大写字母开始组件。

这样，我们可以将它们与其他 JavaScript 实体区分开来。

例如，我们应该写像`SelectButton`这样的名字来命名选择按钮。

# 其他命名约定

在 JavaScript 中，大多数东西都是 camelCase。

类是 PascalCase。

属性也是驼色。

所以我们可以尽可能地坚持下去。

# 将有状态方面与呈现分开

我们可以分离有状态和无状态的组件。

无状态组件只渲染道具中的东西，其他什么都不做。

有状态组件有自己的动态状态，它们可以根据任何情况而改变。

此外，我们可以在各种钩子中操纵状态。

我们可以把逻辑放在我们自己的钩子中，这样我们就不必把它们放在我们的组件中。

例如，我们可以将用于呈现表格的放入它自己的组件中。

我们可以在获取数据的组件中使用它，并将它传递给表组件。

例如，我们可以写:

```
import FancyTable from './FancyTable';class Table extends Component {
  state = { loading: true }; componentDidMount() {
    fetchData().then( tableData => {
      this.setState( { loading: false, tableData } );
    } );
  } render() {
    const { loading, tableData } = this.state;
    return loading ? <Loading/> : <FancyTable data={tableData}/>;
  }
}
```

我们有将我们的`tableData`传递给它的`FancyTable`组件来呈现。

通过这种方式，我们将状态操作从渲染中分离出来。

# 与任何单个组件相关的所有文件都应该在一个文件夹中

我们应该将所有助手组件和其他文件放在与组件相同的文件夹中。

通过这种方式，我们可以将大的组件分成更小的组件，从而更容易找到助手代码。

一个组件通常有自己的 CSS、图标、图像、测试和其他文件。

我们应该把它们放在一起，这样我们就可以在一个地方找到它们。

# 使用像 Bit 这样的组件库

像 Bit 这样的组件库帮助我们将 React 组件组织到一个地方。

它们帮助我们重用代码，因为我们可以从那里安装组件包。

我们也可以创建一个包含所有组件包的 NPM 存储库。

然而，我们不能像使用 Bit 那样即时预览它们。

这样，代码可以在不同的项目中重用，节省我们的时间，减少挫折。

# 使用库

代码片段向我们展示了最好的和最新的语法。

它们还帮助我们学习我们不应该错过的最佳实践。

我们可以在互联网上找到它们的来源。

# 为所有代码编写测试

我们应该为所有代码编写测试，这样我们就不必自己手动测试每个部分。

它们让我们知道，即使我们改变了现有的代码，我们的代码仍然在工作。

测试还帮助我们轻松测试新代码。

我们可以将它们放在一个`__Test__`文件夹中，这样我们就可以存放相关的测试。

测试可以分为功能测试和在浏览器中呈现应用程序的测试。

我们可以使用跨浏览器测试工具来测试渲染。

Jest 可以模拟 DOM 来测试 React 组件。

我们可以操作 DOM 来执行点击等操作，并检查结果的呈现。

![](img/75d991473aaaaa6da9a5510783d00e7f.png)

Photo by [Anton Chernyavskiy](https://unsplash.com/@antonchernyavskiy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

分离有状态和无状态组件使得维护和测试变得容易。

此外，测试也很重要。

我们也可以把我们的组件放在一个存储库中，这样我们就可以很容易地重用它们。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**