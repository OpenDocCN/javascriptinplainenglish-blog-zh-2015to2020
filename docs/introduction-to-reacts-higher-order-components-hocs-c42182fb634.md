# React 的高阶组件(hoc)

> 原文：<https://javascript.plainenglish.io/introduction-to-reacts-higher-order-components-hocs-c42182fb634?source=collection_archive---------1----------------------->

## 借助用例理解 React HOCs

![](img/66407055678dbe9e7a2ecbb45d18bd08.png)

[**techno 漏斗**](https://www.youtube.com/channel/UCo-h1M-5M6Y5D4Lgut8ge4w) 带来另一篇关于**React**中高阶元件的文章。高阶函数是一个以一个函数作为输入参数或者返回一个新函数作为输出。在 React 的上下文中，[高阶组件(hoc)](https://reactjs.org/docs/higher-order-components.html)是将一个组件作为输入并返回另一个组件作为输出的组件。

React 16.8 引入了高阶分量的概念。HOC 是一种重用组件逻辑的高级技术。hoc 不是某些 React APIs 的一部分，但是这个概念来自 React 的组合特性。组合是指将一种成分包含在另一种成分中。

有关高阶分量的视频教程，请参考以下内容:

React 的复合特性允许我们创建包含更多子组件的组件。简单的组合示例是一个简单的 web 页面，包含多个组件，如`<HeaderComponent />`、`<FooterComponent />`、`<NavigationComponent />`和`<ContentContainer />`。所有这些组件组合在一起形成一个单一的网页。

React 进一步扩展了功能，使组件可以将一个组件作为输入，或者将一些组件作为输出返回。

关于组合的细节可以在 React 文章“[组合与继承](https://reactjs.org/docs/composition-vs-inheritance.html)”中找到

# 向现有组件添加功能

高阶组件专注于扩展现有组件的功能。它将一个组件作为输入，并返回一个新组件，该组件包装了作为参数传递的输入组件。包装器可以向作为输入的原始组件提供一些附加数据或功能。

# 确保代码的可重用性

当我们在应用程序中使用多个组件时，可能会出现组件的某些部分可重用的情况(例如，进行异步数据调用)。高阶组件旨在提取代码的公共部分作为包装组件的一部分，包装组件可以被其他组件重用。

# 使用示例代码

## 用例场景

让我尝试借助一个简单的场景来解释这个用例:

我们的员工管理应用程序有两个组件。每个组件都需要呈现一些雇员数据。两个组件所需的数据可从同一逻辑/功能获得。这两个组件都需要调用数据的公共函数。

现在记住上面的场景，让我们看看如何解决所需的使用场景，确保我们可以共享公共逻辑。

## 员工基本详细信息组件

这是应用程序所需的第一个组件。它显示员工的基本信息。上面给出的代码创建了一个简单的功能组件，它将雇员的详细信息作为输入参数`props`，并显示雇员的基本详细信息(`name`、`age`、`designation`)。所以输入的`props`应该有指定的道具属性。

## 员工薪金详细信息组件

我们正在创建另一个组件(如上所述),向用户显示与工资相关的雇员信息。该功能组件将一个`props`参数作为输入，并向用户显示与工资相关的信息(`salary`和`bonus`)。

## 员工详细信息的通用逻辑/功能

这两个组件都需要包含必需字段的对象文字。`<ShowEmployeeBasicDetails />`组件期望在`props`对象中提供姓名、年龄和称谓属性。而`<ShowEmployeeSalaryDetails />`组件期望薪水和奖金属性作为`props`对象的一部分。

上面给出的是一个获取两个组件所需细节的函数。它包含所有必需的字段。

## 解决 HOCs 的问题

因为两个组件所需的数据都可以从一个函数调用中获得，所以我们可以重用代码来获取两个组件的数据。可重用的代码可以作为一个单独的包装组件被分离出来，我们的主要组件可以作为一个参数传递给它。这个包装器组件提供了额外的功能来获取数据，并将这些数据提供给作为参数传递的任何组件。(`ShowEmployeeBasicDetails`和`ShowEmployeeSalaryDetails`)。包装函数被称为高阶组件。

[https://gist.github.com/Mayankgupta688/ff74c2afd77b216158a7de72d2098e4c](https://gist.github.com/Mayankgupta688/ff74c2afd77b216158a7de72d2098e4c)

在上面的代码中，我们创建了一个函数`higherOrderComponent`，它将一个组件作为输入并返回另一个组件。当呈现返回的组件时，构造函数执行获取组件的公共数据的公共任务。数据作为`props`提供给包装的组件。

[https://gist.github.com/Mayankgupta688/f82d3dd53f56f64a626af245cb4c18dc](https://gist.github.com/Mayankgupta688/f82d3dd53f56f64a626af245cb4c18dc)

# 总结和结论

我再解释一下。`ShowEmployeeBasicDetails`和`ShowEmployeeSalaryDetails`组件作为输入传递给包装器组件。这些包装的组件需要`props`数据。包装函数中指定的构造函数调用函数来获取数据，并将数据保存到状态变量中，这些数据可供组件使用。因此，我们减少了代码，因为两个组件都不需要在每个组件中有单独的数据获取代码。

高阶组件提供了一种很好的方式，将常见的功能包装在一个包装器组件中，并使它们对包装的组件(作为参数传递的组件)可用。

我希望这篇文章能够阐明高阶元件的基础知识。谢谢！

下面是[提琴手](https://jsfiddle.net/Mayankgupta688/4wcjgLqf/18/)的工作代码:

[https://jsfiddle.net/Mayankgupta688/4wcjgLqf/18/](https://jsfiddle.net/Mayankgupta688/4wcjgLqf/18/)

## 进一步阅读

[](https://bit.cloud/blog/introducing-component-compare-easily-review-component-changes-l4qyxtoo) [## 比特博客

### 组件驱动软件的官方博客。围绕现代组件驱动的 web 开发的文章…

比特云](https://bit.cloud/blog/introducing-component-compare-easily-review-component-changes-l4qyxtoo) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) ***。*****