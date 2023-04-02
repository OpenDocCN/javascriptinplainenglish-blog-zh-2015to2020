# 我阅读了整个 React API。以下是我对新开发者的建议

> 原文：<https://javascript.plainenglish.io/i-read-the-entire-react-api-here-is-my-advice-to-new-developers-d040507e6c23?source=collection_archive---------0----------------------->

![](img/2612126b66e4f227438ec56c853ad480.png)

*我阅读了整个* [***React 顶级 API***](https://reactjs.org/docs/react-api.html)**这里是我对新手学习 React 和有经验的程序员采用 React 的建议。**

# *介绍*

*关于 React(或 ReactJS ),首先要克服的一个问题是对它的大肆宣传。“用于构建用户界面的 JavaScript 库”(不是一个框架)，通常是由大多数 React 专家、大师甚至核心团队开发人员从一个非常高级的角度来呈现的。似乎人们期望提升他们的编程思维来理解概念和原理。*

*请注意，React 不是一个框架，而是一个库，因为它依赖第三方库来提供一些核心功能，如路由，不像 Angular 或 Vue.js 这样的框架有这样的内置功能。所以它不是一个完整的框架。然而，它是前端库和框架的金童，因为它在应用高级编程概念方面非常简单。在松耦合和微服务的时代，前端库(与 Angular 和 Vuejs 等其他 JavaScript 框架处于同一水平)是构建复杂前端的最接近惯用 JavaScript 的哲学。不拘泥于 Angular 的 TypeScript/MVC 架构或者 Vue.js '耦合 HTML。*

*更重要的是，react 是一个脸书开源项目，由 J**ordan walker，**创建，由一些最积极的年轻开发人员领导——Dan abra mov，他经常神秘地解释库背后的概念，几乎可以在(社交)媒体的任何 React 主题上看到，Brain Vaughn 从图形设计过渡到编程，构建了 React 虚拟化，并在 React 核心团队中找到了一份工作*

# *反应过来道*

*任何一个来 React 的人都应该理解的高层次观点源于这样一个事实，React 既不是一个框架，也不是一种编程语言，而是一个库，它的开发者选择了他们认为更适合于构建响应性和功能性用户界面的某些原则。这些原则包括*基于组件的设计、继承之上的组合、自顶向下的单向数据流，以实现功能反应式编程中的一致性。**

*它基于一个声明性的范例，而不是命令性的范例。此外，React 结合了面向对象编程(OOP)的主要范例，最好使用现代 JavaScript、ES6 和更高版本来实现，尽管它对 ES5 具有向后兼容性。*

*因为 React 更喜欢组合而不是继承，所以你不需要实现任何接口，在[React 团队拒绝](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)使用 Mixins(接口的高阶构造)之后，它们就被淘汰了。对于 Java 一族来说，接口是语言固有的一部分，它已经在 Java 8 接口中采用了 Mixins。因此，这样一个来自基于类的继承范例的人最需要掌握 JavaScript，学习动态类型，以及 JavaScript 语言的其他精彩特性，比如知道除了变量之外，一切都是函数。您可以将函数传递给普通 JSON 格式的方法和对象，并进行回调等等。*

# *一切都是组成部分*

*如果你是一名经验丰富的程序员，来自基于类的继承语言，如 Java、C#，并且你了解像 Spring Boot 这样的框架，你会发现 React 的主要构件是 *React。组件*类，它定义了你要实现的方法，其中一个是 *render()，*，它对于任何组件来说都是强制性的，它会呈现自己，包括它在网页上包含的其他组件。并非所有组件都可以声明一个呈现方法，只有那些将组件呈现(读取插入)到 DOM 并利用生命周期方法和内部状态的组件，其他组件可以将它们的状态作为道具。它们可以像普通的 JavaScript 变量一样被声明，并且必须被初始化或在函数中作为 JSX、JavaScript XML 返回，并且包含在包含 render 方法的另一个组件中。因此，对于理解应该重写哪些方法来获得父组件类中存在的一些功能，您没有任何困难。*

*对于编程新手来说，你要学习两件事，OOP 的基于类的继承和基于原型的继承。例如，如果一个人仍然在 React 中使用类。他们需要理解对象到类的建模(OOP)和某些概念，比如静态关键字。一个典型的例子是 React 组件中的*静态 getderivedstatefromprops* ，其中*这个引用类的*关键字不能被访问。因为静态方法不是要调用它的对象的成员，但是可以在类级别访问。同样，当您编写启动 React 应用程序的引导程序 *ReactDOM.render()* 时，会调用 *ReactDOM* 函数的 render 方法。编程新手的学习曲线更加陡峭。*

*新手可能会感到困惑，因为现在有三种编写 React 组件的方式都是有效的。(在 Angular[非 AngularJS]中，它是严格的打字稿。)它们是:*

1.  *基于类的组件*
2.  *功能组件*
3.  *类型脚本功能组件*

*在 ES5 之后采用了基于类的组件，其中每个组件都可以是一个类，并且拥有使用 *this.state{}* 声明的类变量。该类在其构造函数中接受道具(属性)，该构造函数必须调用从父组件传递给它的*超级(道具)*。*

*假设您正在开发一个待办事项应用程序，您在一个页面上有两三个待办事项卡片，您创建了一个包含待办事项的*待办事项*组件，您将让您的父组件进行数据库调用并获取所有待办事项的数据并显示，您让每个*待办事项*组件更新其内容并保存到数据库。所以你传递给 todos 的初始数据是道具，每个 todo 的数据是它的状态。你会遇到如何使用 **PureComponents** 、**高阶组件(HOC)** 和 Redux 之类的助手库来管理状态。*

***功能组件**是现今编写 React 组件的首选方式。它们是作为 React 版本 16 的一部分发布的。它们是惯用的 JavaScript。为了操纵它们的状态，引入了[钩子](https://reactjs.org/docs/hooks-intro.html)。钩子是用来“钩”入功能组件状态的函数，因为它们是无状态的，它没有 *this，*因此没有类似类的状态。一个钩子用*声明，使用*作为第一个单词。如*使用状态*。如果需要，它返回状态和设置器，或者只返回状态。*

*例子是:*

```
**const [count, setCount] = useState(0)**
```

*我们初始化 count = 0。我们包含了一个用于更新状态的设置器。我们不应该在组件中使用 count +=1，因为它们会引入错误。相反，我们做 setCount(count+1)。钩子是如此的惯用，以至于它是纯 JavaScript 的，所以人们不必再为太多的类知识而烦恼。*如果你是 JavaScript 专业人士，相信我，一个月就能学会 React。任何东西都可以用钩子来操纵。生命周期方法反应。使用*使用效果*挂钩可以模仿所提供的组件，挂钩有以下变体:**

```
*// (1) On Mount and every render
useEffect (() => { 
  dosomething()
});// (2) Only on Mount
useEffect (() => {
  dosomething()
}, []);// (3) On Mount/every time state of count changes
useEffect (() => {
  dosomething()
}, [count]);// (4) UseEffect with cleanup
useEffect (() => {
  dosomething();
  return clearSomething(){};
});*
```

*注意四个 useEffect 挂钩的区别。第一个没有第二个参数(在挂载和每次重新渲染时运行)，另一个有一个空数组(只在挂载时运行)，第三个有数组中的 *count* (在挂载和每当 *count* 的状态改变时运行)，最后一个返回一个函数，该函数用于在卸载组件时进行清理。它们用于模拟生命周期方法——componentdimount、componentDidUpdate 和 componentWillUnmount——我们需要在类中管理组件的安装、更新和卸载。第四个用于卸载，注意返回的函数。其他生命周期方法也可以用钩子实现- [getDerivedStateFromProps 可以用组件状态](https://codesandbox.io/s/react-getderivedstatefromprops-to-hooks-y9boj?from-embed)和 Props 之间的比较来实现。shouldComponentUpdate 可以用 React 实现[。备忘录](https://stackoverflow.com/a/57405307/905494)防止重新渲染，它使用记忆。*

*关于功能组件和类的争论在 React 生态系统中很常见。尽管功能组件无处不在，但仍有一些特性没有像[错误边界](https://reactjs.org/docs/error-boundaries.html)那样的功能实现来显式捕捉组件中的错误。有些人认为功能组件比类组件更快的说法是一个神话。*

***类型脚本功能组件**是对功能组件的增强。引入静态类型的需求在 TypeScript 中已经成为可能，在 TypeScript 中，所有变量和参数都必须在声明和函数参数中给定一个类型。[类型检查](https://reactjs.org/docs/typechecking-with-proptypes.html)为了类型安全，在类型脚本中必须对类和函数组件中的属性类型进行检查，但对接口进行检查。*

# *反模式*

*您会经常听到一个流行词，反模式。React 中的反模式是对指导如何实现 React 以获得最佳结果的成文和不成文原则的偏离。偏差会导致糟糕的编程实践。一个例子是改变子组件中的属性。这样做是不可取的，因为它打破了自上而下的流程。*

# *状态管理*

*React 固有地附属于 Redux(这是变化的),它是一个状态管理库，事实上 React 作业是与 Redux 一起发布的。现在，虽然我们有其他状态管理库，如 Flux 和 Mobx，但 Redux 是默认选择。我们知道大多数前端应用程序管理状态，Redux 提供了一个存储、reducers 和动作来帮助实现这一点。在选择学习项目时，选择使用 Redux 的项目。这将有助于完成学习。*

# *专家们*

*最后，React 是一个很好的前端库(read framework)。我不知道 Angular 和我现在知道的 React 一样多，但我已经完成了 stackblitz 教程。学习 React 最好的地方之一是 egghead . io。React 的主要开发者之一 Dan Abramov 在那里有视频教程。然后是拥有自己网站的[肯特·C·多兹](https://kentcdodds.com/)。还有[塔妮娅](https://www.taniarascia.com/redux-react-guide/)。你可以通过谷歌找到成千上万的其他人。你实际需要的是开始思考 JavaScript 和 OOP(基于类和基于原型)包括 DOM API 的知识，而不是真正的深入。React 在 DOM API 之上提供了一个抽象，所以你不能直接操作 DOM，但是如果你愿意的话，可以通过 *refs* 来操作，但是你需要知道它是如何工作的。通过 ReactDOM，我们能够将 JSX 插入到 web 浏览器中，因此它是平台和浏览器独立的。在 React 中你基本上可以做任何你想做的事情。在您变得更高级并开始构建复杂的用户界面之前，您可能不会担心底层的连接和样板代码。您可以浏览 React GitHub 页面，亲自了解一些事情。*

# *结论*

*我不想深入探讨，因为这意味着对 React 世界的一个概述，而这是一篇文章或一本书所不能涵盖的。我甚至还不太了解。我正在努力学习，争取在 2020 年底前成为一名 React 摇滚明星。但是我上面所阐述的来自于边做边学，事实上我用 React 实现了一个计算器，阅读了整个顶级 API，并且一直在 Stackoverflow 上回答问题。我也做过一个 React 原生应用，是有人启动的，我完成了。*

*我相信我已经给了 React 世界一个**鸟的** - **视角**。相信我，掌握了这些概念，在不到 6 个月的时间里，React 的所有魔法和原理对你来说听起来都会很正常。一年前写这个教程的时候我还不明白这些东西。我还记得我第一次读[官方的 React 教程](https://reactjs.org/tutorial/tutorial.html)，它听起来像希腊语，但是今天，它很有意义。*

*jikagon syt ziry(" Go it " in[High Valyrian](https://lingojam.com/EnglishtoValyrianTranslator))。*

*这篇文章已经被翻译成中文，可以在 InfoQ 上找到。*