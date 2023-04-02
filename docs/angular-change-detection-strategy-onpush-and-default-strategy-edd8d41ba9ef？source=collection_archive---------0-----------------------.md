# 角度变化检测策略— onPush 和默认策略

> 原文：<https://javascript.plainenglish.io/angular-change-detection-strategy-onpush-and-default-strategy-edd8d41ba9ef?source=collection_archive---------0----------------------->

## 了解 Angular 中的 onPush 和默认变化检测策略。

TechnoFunnel 提供了另一篇关于**角度变化检测策略**的文章。Angular 中有两种变化检测策略(**默认和 onPush** )。我们将寻找使用这些策略的优点和缺点。

![](img/d663dd896d5bd55a4c95cacef9b501c1.png)

Angular Change Detection Strategies (onPush and Default)

# 什么是角度变化检测？

Change Detection in Angular

角度变化检测负责使组件动态化。在变化检测周期中，Angular 寻找所有的绑定，重新执行所有的表达式，将其与以前的值进行比较，如果检测到变化，它将把变化传播到 DOM 元素。

**角度变化检测**在以下情况下执行:

1.  角度状态变量有更新
2.  在角度组件内部调用事件
3.  组件的@输入值已更新

# Angular 中的变化检测策略是什么？

**角度变化检测策略**是跟踪组件更新并触发组件重新渲染的方法。角度主要有 **2 种变化检测策略。我们可以为装饰器内部的组件配置变更检测策略。**

1.  默认策略
2.  onPush 策略

## **默认变化检测策略**

默认的更改检测策略在组件创建时应用于组件。如果未配置组件策略，它将被标记为默认策略。在这种策略中，变更检测周期在组件内部发生的每一个事件上运行。

1.  元素的点击事件
2.  通过异步调用接收数据
3.  触发 setTimeout 和 setInterval

让我们寻找与这种默认变更检测策略相关的问题:

上面给出了两个组件:ParentComponent 和 ChildComponent。父组件包含一个属性“ **counter** ”，每次用户单击按钮时都会更新该属性。每次“计数器”的值更新时，组件都会重新呈现。该组件还包含一个子组件。子组件独立于“**计数器**”数据，因为我们没有使用@Input 将数据传递给子组件。

[https://gist.github.com/Mayankgupta688/188168fc729b86d973af0db80d534925](https://gist.github.com/Mayankgupta688/188168fc729b86d973af0db80d534925)

让我们来看看 ChildComponent 的实现，ChildComponent 是一个静态组件，它不会对父数据的更改产生任何影响。即使父组件的计数器值更新，子组件的视图也应该保持不变。在下面的组件中，我们没有在@Component 装饰器中提供任何策略。默认配置为“**默认变化检测策略**

[https://gist.github.com/Mayankgupta688/a96ed64f0150493388981e4caee00245](https://gist.github.com/Mayankgupta688/a96ed64f0150493388981e4caee00245)

在**默认变化检测策略**中，每当我们更新 ParentComponent 中的计数器时，ChildComponent 生命周期也被触发重新渲染，即使计数器的变化对子组件的视图没有影响，这个子组件也被重新渲染。子组件独立于父组件的数据。

在上面的代码中，每次子组件渲染时，它的控制台都会记录消息“App Rerendered”。每当我们在控制台窗口中看到该消息时，这意味着子组件也正在被重新呈现。您可以查找下面给出的在线编辑器链接，以测试默认变更检测策略的行为。

[](https://stackblitz.com/edit/default-change-detection-strategy?file=src/app/app.component.ts) [## 默认-更改-检测-策略-堆栈

### 导出到 Angular CLI 的 Angular 应用程序的启动项目

stackblitz.com](https://stackblitz.com/edit/default-change-detection-strategy?file=src/app/app.component.ts) 

[https://codesandbox.io/s/default-change-detection-strategy-cspjl?file=/src/main.ts](https://codesandbox.io/s/default-change-detection-strategy-cspjl?file=/src/main.ts)

这个**默认策略的性能较差**，因为即使没有影响，子组件也需要额外的渲染周期。

# onPush 变化检测策略

为了解决上述问题，我们采用了“**on push**”**检测策略**。在这个变化检测策略中，子组件并不总是被脏检查，如果父元素正在更新没有作为“@Input”属性传递给子组件的值，那么子组件不应该被脏检查。

## onPush 更改检测的优势

*   在子组件中没有不必要的脏检查
*   更快的组件重新渲染

在上面的代码中，我们更新了子组件的变更检测策略。由于更改检测策略更新为 OnPush，因此如果父组件的属性更新，组件将不会刷新/重新呈现。在上面的代码中，因为我们没有更新任何@Input 属性，所以组件不会重新呈现，这样性能会更好。

## 小心对象变异:

作为@Input 属性接收的对象不应该变异。当从父组件接收@Input 对象时，它作为引用被接收。如果原始对象是从父元素变异而来的，则引用不会在子组件中更新，因此@Input 组件不会接收新的引用，也不会被更新，因为@Input 属性中的对象仍然是相同的(引用)。

在这种情况下，没有机制可以通知子组件@Input 的一个属性已经更新。我们应该用更新的属性创建一个新的对象，这样@Input 属性的引用就会更新，子组件会得到组件更新的通知，从而重新呈现组件。

[](https://stackblitz.com/edit/onpush-change-detection-strategy?file=src/main.ts) [## onpush-change-detection-strategy-stack blitz

### 导出到 Angular CLI 的 Angular 应用程序的启动项目

stackblitz.com](https://stackblitz.com/edit/onpush-change-detection-strategy?file=src/main.ts) 

[https://codesandbox.io/s/onpush-change-detection-strategy-rlxhf](https://codesandbox.io/s/onpush-change-detection-strategy-rlxhf)

OnPush 策略的实现参考上面的代码。

## 进一步阅读

[](https://plainenglish.io/blog/create-an-employee-satisfaction-survey-using-angular-and-store-results-in-a-mongodb-collection) [## 使用 Angular 创建员工满意度调查，并将结果存储在 MongoDB 集合中

### 一步一步的教程来建立一个员工满意度调查使用 Angular 和 SurveyJS，一个免费的，开源的…

简明英语. io](https://plainenglish.io/blog/create-an-employee-satisfaction-survey-using-angular-and-store-results-in-a-mongodb-collection) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) ***。*****

*****对缩放您的软件启动感兴趣*** *？检查* [***电路***](https://circuit.ooo/?utm=publication-post-cta) *。***