# 如何学会正确反应

> 原文：<https://javascript.plainenglish.io/how-to-learn-reactjs-the-right-way-for-self-taught-developers-87d7cc8c1d32?source=collection_archive---------3----------------------->

## 对于自学的开发人员

![](img/d9d3072fb09e1deffd10efea7a90f92b.png)

Photo by [Evan Dvorkin](https://unsplash.com/@evphotocinema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> "最重要的是继续前进的勇气。"— Ann Adaya

如果您已经学习 Javascript 很长一段时间了，那么最好是接触不同的 Javascript 库或框架，其中一个，也可能是前端开发中最流行的一个，就是 ReactJS。

有人说 React 帮助开发人员成为更好的 Javascript 开发人员，我同意这种说法，在我有机会学习 React 之前，我从来没有深入理解过 Javascript，我尝试过使用 ExtJS，这是另一种 Javascript 框架，尽管它不是开源的，而且仅仅知道不是开源就有很大的学习难度。

然而，当你学习 React 时，你应该思想开放，你需要忘记很多要学的东西，如果你真的想成为一名 React 开发者，你必须适应 React 思维，保持开放，有耐心，最重要的是享受旅程，这才是一切值得的。

本文旨在作为您学习 ReactJS 的指南。我们将解决反应堆世界中的不同领域、不同特性和技术，让这些成为您前进的指南。

从基础到高级，再到使用其他几种工具，让您的反应应用程序尽可能强大。

> "首先要确定目的地，只有这样你才能找到正确的地图."

# 什么是 REACTJS？

根据定义，**reactor . js 是一个开源的 Javascript 库，用于构建用户界面，尤其是单页应用程序。它用于处理网络和移动应用的视图层。**

要开始 ReactJS:您首先需要使用您的终端安装 React globally，只需键入**NPM install-g create-React-app**并等待软件包完成安装。

慢慢地但肯定地接受下面的主题，理解它们，知道它们的功能、重要性和理解整体流程，不时地看一看更大的图景，这样你就可以登上梯子，而不仅仅是在你深入反应时跳到任何东西上。

请记住，这只是一个指南，并对您需要注意的反应堆的要点和特性做了一些简单的介绍，您的工作是学习它们，研究它们并进一步了解它们。

# 反应堆基础

首先学习每一种编程语言、库和框架的基础知识的重要性怎么强调都不为过。以下是您在学习过程中需要学习和理解的属性和主题。

**create-react-app——是构建 react 应用程序的起点。**

您可以在您的终端上使用 create-reaction-app 来生成包含最基本启动要求的启动包。

**组件—** 组件是所有 React 应用程序的构建模块，从基础到由一个或多个组件组成的更复杂的应用程序。

在 Javascript 中，可以把它想象成一个类或函数，它可以接受输入并根据 UI 的外观返回元素。

有两种不同的成分需要反应，它们是:

**类组件—** 是有状态组件。类是创建组件最常见的方式之一，因为它提供了更广泛的功能和更多的控制，尤其是在使用生命周期方法时。

```
class sampleClassComponent extends React.Component {
  render() {
    return <h1>Hello World!</h1>;
}//and this is class component using state
class myComponent extends React.Component {
 constructor(props) {
     super(props);
         this.state = {
         message: 'Hello World!',

       };
  }render() {
     return (
       <div>
          <h1>{this.state.message}</h1>
       </div>
      ) 
    }
}
```

**功能组件—** 最简单也是最基本的组件就是功能组件，可以写成 2 种方式，你可以用老的方式或者干脆用 ES6 的 arrow 函数语法。

**功能组件是无状态的。**

```
//Don't forget to use comments all the time too!
function sampleFunction(props) {
  return <h1> Hello World! </h1>
}or you can use an arrow function ES6, whichever you prefer and what suits the requirements.//this is an arrow function syntax, better and shorter :)
const sampleFunction = (props) => <h1> Hello World! </h1>;class sampleClassComponent extends React.Component {
  render() {
    return <h1>Hello World!</h1>;
}
```

jsx — 就是在 Javascript 中编写 HTML，jsx 就是你想要的 UI 的语法。

**props —** 是 React 中使用的特殊关键字，是属性的简写。

**‘Props’**用于将数据从一个组件传递到另一个组件，需要记住的重要一点是，Props 只能由父组件传递到子组件，而**不能**反之亦然。

**State —** 是所有组件的中心，它具体决定了组件的渲染和行为方式。

最简单地说，我可以解释的是，状态就像你的组件或你的应用程序的当前行为。例如，您的应用程序要显示您所在地区的当前温度，您决定简单地显示蓝色表示冬季，黄色表示夏季，就这样。

所以基本上，如果是夏天，那么你的应用程序的状态将是夏天，并将显示黄色，如果是冬天，那么它将显示蓝色，我希望这是有意义的。

```
class Student extends React.Component {
 constructor(props) {
     super(props);//this is where you define your state
         this.state = {
         name: 'Ann',
         subject: 'Math'
       };
  }render() {
     return (
       <div>
          <h2>My name is {this.state.name}</h1>
          <h4>and my subject today is {this.state.subject}</h4
       </div>
      ) 
    }
}
```

**useState —** 这是专门用在功能组件上的，我提到的功能组件是无状态的，通过使用 useState，功能组件可以变成有状态的。

**useEffect —** 和 useEffect 也是专门为功能组件做的，它帮助功能组件利用以前只有类组件才有的生命周期钩子。

需要知道的一件重要的事情是钩子只用于功能组件，比如 useState 和 useEffect。

setState — 可能是更新应用程序状态的唯一有效的方式，或者说是最好的方式。

我能使用的最简单的例子是让你试着想象当你试图登录一个网站，所以你填写你的登录凭证，一旦你点击登录按钮，也就是当 setState()被触发时，你会看到加载部分和 boom 你登录了，其中网站的状态已经从欢迎请登录页面变成你的登录页面或你的仪表板或个人资料，明白了吗？

**组件生命周期方法—** 每个反应组件都有其生命周期。有几种生命周期方法你需要完全理解，从文档中阅读并找到教程，记住理解它们是如何工作的，如何运行，使用哪一种，最重要的是知道它们的时间。

*   componentWillMount()
*   componentDidMount()
*   componentWillReceiveProps()
*   应组件更新()
*   componentWillUpdate()
*   componentDidUpdate()

**列表和关键点—** 您将要做和应该做的第一个项目的基本教程之一应该是关于包含列表的项目，例如待办事项列表、食品杂货列表，甚至包括 React CRUD 的项目。

我会说这是一种非常温和的从 Javascript 过渡到 reaction 的方式。

一旦你熟悉了这些基础知识，你就可以慢慢地但肯定地开始，进入 Reactjs 更高级的领域。

这里有一些你需要学习的概念，通过阅读文献，并做一些研究来进一步理解它们。同样，我强烈建议你在进入下一个我们将讨论的话题之前先了解 Reactjs 的基础知识。

# 高级反应堆

**上下文—** 这是数据传递到不同组件的地方，也是 Redux 发挥作用的地方。在这里，我们可以创建全局变量，这样当我们传递道具时，我们就可以很容易地跳过组件树，我们不必在每一级手动传递它，这就像从祖父母组件跳到子组件一样。

**高阶组件—** 这是 reaction 中关于重用组件逻辑的高级技巧，当一个我们称之为高阶组件的组件函数接受一个组件并返回一个新组件时，就会发生这种情况。

**参考文献—** 参考文献允许您访问 DOM。“反应”中最重要的一条规则是不要强制操纵 DOM，但是，有些情况下我们必须这样做，所以“参考文献”来解救我们。

**错误边界—** 帮助捕获错误，以便在出现异常时能够正确设置 UI 回退。由于 React 处理 DOM 的方式不同，捕获错误的方式也不同。

**门户—** 门户帮助您呈现 React 应用程序根元素之外的 UI。想象这个的最好方法是想象一个门，它会带你去不同地方或国家的神奇地方。

**HTTP 请求—**API 来了。您可以使用 fetch 或类似 Axios 的工具来帮助处理 GET 和 POST 请求。了解更多并最好理解这一点，因为了解和拓宽您的事物视角非常关键，处理数据从未像现在这样重要，数据变得越来越昂贵，并且由于 ReactJS 及其第三方工具，这些功能从未像现在这样清晰和简单。

# 增强 ReactJS 应用的其他技术

**Redux** —基本操作你的 React app 的状态管理。要知道 redux 是一项独立的技术，它广泛应用于不同的 Javascript 库，如 Angular，它不是 React 的一部分，但它会使你的 React 应用程序非常强大，大多数复杂的应用程序使用 Redux 来更好地处理事情，最重要的是状态。

Redux 一开始有点辛苦，但是一旦你能理解，你的人生就再也不一样了。

Apollo 客户端 —也和 GraphQL 一起处理状态管理。Apollo Client 还可以与 Vue.js 等其他 Javascript 库和框架协同工作。

**GraphQL —** 另一个 API 工具。GraphQL 是 API 的查询语言，它是一种关于向谁请求数据的查询语法。

**React-Router** —每当用户从网站或应用程序的不同页面进入时，用于不同组件的转换。

**材质 UI，Ant 设计**用于造型包。

**React Hook Forms —** 这是一个可以用来在应用程序中轻松使用表单的包。你可以查看它的 Github repo 来获得更多关于如何使用这个包的细节。

**Cypress — f** 或者说测试你的 React 应用程序，在我目前的工作中，除了开发应用程序之外，我们还被赋予了测试通过外包构建的应用程序的任务，即使你是一名开发人员，了解不同的测试技术也是很有用的，Cypress 对 React 应用程序来说很棒，很简单，只需编写简单的脚本就可以轻松测试你的应用程序。

**Typescript —** 据说是一个自文档化的代码。typescript 现在越来越受欢迎，一些高级开发人员鼓励开发人员使用 Typescript，包括我的高级开发人员，我目前也在学习使用 Typescript。

其中一个好处是，Typescript 使代码更容易阅读、调试和理解，它使您的代码看起来清晰明了，基本上可以编写更好的 Javascript。

**Story Book —** 另一个特别适合前端开发人员的好工具，是一个用于开发 UI 组件的开源工具，有助于使构建更有条理。

> “要谦虚，要有教无类，永远保持学习。”

非常感谢您的阅读！**关注我获取更多自学的开发者作品和灵感**，你也可以在 [Instagram](https://instagram.com/womencodes_) 上找到我，到时见！

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**