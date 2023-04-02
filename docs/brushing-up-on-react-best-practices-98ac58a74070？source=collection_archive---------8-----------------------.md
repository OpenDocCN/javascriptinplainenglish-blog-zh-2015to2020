# 复习 React 最佳实践

> 原文：<https://javascript.plainenglish.io/brushing-up-on-react-best-practices-98ac58a74070?source=collection_archive---------8----------------------->

## 保持你的代码干净、模块化和干燥

*正在进行的系列* [*的一部分温习 React*](https://medium.com/@mdazmainamin/brushing-up-on-react-basics-18ad67528b85) *。*

![](img/0496db2cc5be7db99024b022a4bcbd3c.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 容器和视图组件也称为智能和非智能组件

这是迄今为止我最喜欢的反应模式，并加强了单一责任的神圣编码规则。想法很简单:将您的逻辑(即业务逻辑或获取数据)封装到一个组件(容器或智能组件)中，将您的表示逻辑封装到另一个组件(视图或哑组件)中，后者的唯一职责是获取数据并呈现 html。容器组件将完成所有繁重的工作，并呈现传入所需数据的视图组件。

丹·阿布拉莫夫*曾经*是这种模式的大力支持者，但是在 [React Hooks](https://reactjs.org/docs/hooks-intro.html) 发明之后，他已经缓和了他的说教。我从他的中帖(下面的链接)的更新中了解到的是，他反对*强制*这个模式，这是有道理的。永远不要强迫一种模式。只有当它有意义时才跟随它。模式就像语言一样，是实现特定结果的工具。

即使有挂钩，我也喜欢这种图案。我喜欢组件的*单一责任主体*的想法。智能和非智能组件意味着更小的组件，这反过来意味着它们更容易理解和编写测试。如果我想为我的表示逻辑编写测试，我应该不需要担心嘲笑一堆东西来绕过钩子。如果您将表示和核心逻辑捆绑在一起，那么无论您试图测试什么，您都必须模拟整个组件。

多向男人本人学习:[智能 vs 哑巴组件](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)。

# 高阶组件

高阶分量这个名字来源于高阶函数，它以其他函数作为自变量。HoC 接受一个组件作为一个`arg`，添加一些逻辑并返回另一个添加了逻辑的组件。这个想法是“增加的逻辑”可以与任何其他组件不加区分地重用，从而保持您的代码“干燥”。HoC 也融合了 React 的全部功能。

# 受控组件

受控组件处理 HTML 表单元素，如和<select>。您用一个定制组件包装这些元素，并规定它们的行为，就像当用户点击一个单选按钮时，使用包装组件的state会发生什么。为什么这么问？这与更有效地从 DOM 获取数据有关。大概吧。从这里阅读更多内容。一个组件应该是一个独立的模块React 背后的整个理念是创建可重用和模块化的组件。这确保了整个应用程序的可维护性和一致性。我喜欢把我的组件源代码、测试代码和css文件放在同一个目录下。这样可以更容易地找到该组件所需的一切。你不必去野外狩猎。此外，它强化了组件是可重用性来源的观点。如果您想在不同的项目中使用您的组件，您只需复制该目录并使用该组件。或者更好的是，发布您的组件库，然后在您的整个产品套件中重用该库。创建包装第三方组件这有助于整个应用程序的一致性。假设您正在使用第三方 carousel 组件。将其包装在自定义组件中，并添加您自己的修改和配置。现在，当您需要在其他地方再次使用它时，您只需使用。您不需要再次添加您的修改或配置。如果你想使用不同的配置，只需将它们作为道具传入即可。import Carousel from 'carousel-number-one';import './myCarousel.less';const carouselConfig = { alignment: 'vertical'};export default function MyCarousel(props) { return (<div className='my-carousel'> <Carousel config={carouselConfig}/> </div>);}使用 propTypes 和 defaultPropsJS 的一个缺点(或者优点，取决于你问谁)是它不是类型安全的。当然，您可以使用 TypeScript，但这不是本文档的一部分。你可以给你的组件增加一点类型安全并使它们更健壮的一个方法是使用 propTypes 。使用 React 的静态类属性defaultProps。如果没有向组件传递任何属性，组件将使用该组件。更新状态时，始终使用 prevState很容易忘记 setState 是异步的，因此当前状态可能不是您所想的那样。因此，当您调用 setState 时，最安全的做法是使用 prevState 来更新较新的状态。this.setState((prevState) => { return { numOfClicks: prevState.numOfClicks + 1 } });正在进行的系列 的一部分，温习 React 。</select>