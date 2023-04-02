# React 应用程序中文件结构的简单方法

> 原文：<https://javascript.plainenglish.io/solving-the-file-structure-problem-in-react-apps-4b5b5dca775b?source=collection_archive---------2----------------------->

![](img/1fc1314e46b76808f280e36f934fbf7a.png)

Photo by [Maarten van den Heuvel](https://unsplash.com/@mvdheuvel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## React 中文件结构的典型问题陈述很简单:“把这个*东西*放在哪里最有意义？”让我们一劳永逸地解决这个问题！

React 应用程序的文件结构可以是痛苦和突出的，也可以是有利和适度的。如果您像大多数开发人员一样，您会希望找到适合您的特定应用程序的最佳方法。React 允许您灵活地使用它，例如，可以使用少量的包或管理状态的方式，这使得文件结构成为一个需要解决的棘手问题。

不管是什么库、框架还是语言，文件结构都是很难的。除非有一个非常固执的约定，即*应该像在某些框架中一样被*遵循，否则在一个应用的整个生命周期中，你将很难确定一个结构。如果我们通过理解什么时候*最*是一个问题:当代码的关系没有明显地被文件结构反映出来时，来接近问题的解决方案，关于如何解决结构问题的关注或正在进行的讨论可以成为过去。

## 关注文件关系(或托管原则)

从团队而不是个人的角度来看问题，开始问自己在开发文件结构时，团队中最大的冲突可能是什么。您可能会发现，文件结构在组设置中很难形成的原因通常是因为当涉及到什么放在哪里以及如何命名文件夹或文件时，观点冲突。有些人可能希望在名为“components”的文件夹中包含根级别的所有组件。其他人可能选择使用“公共”文件夹，该文件夹将组件存放在“组件”文件夹中。变化无穷！然而，如果我们从编写代码的人的需求或关注点移开，看看代码内部的关系，我们就可以开始开发一个*才有意义*的文件结构。

## 方法

让我们来看一个利用关系的示例文件结构。*注意:这个特殊的例子使用 Typescript 和 Redux 进行状态管理，但是也可以应用于使用 Javascript 的 React 应用程序。只需将* `*.ts/.tsx*` *的文件扩展名替换为* `*.js/.jsx*` 。

```
.
├── actions
│   ├── index.ts
│   └── someAction
│       ├── someAction.test.ts
│       └── someAction.ts
├── components
│   ├── SomeComponent
│   │   ├── SomeComponent.test.tsx
│   │   ├── SomeComponent.tsx
│   │   └── someComponent.scss
│   └── index.ts
├── constants
│   ├── index.ts
│   └── someFeature.ts
├── containers
│   ├── dashboard
│   │   ├── Dashboard.test.tsx
│   │   ├── Dashboard.tsx
│   │   └── dashboard.scss
│   └── login
│       ├── Login.test.tsx
│       ├── Login.tsx
│       ├── components
│       │   ├── SomeComponent
│       │   │   ├── SomeComponent.test.tsx
│       │   │   ├── SomeComponent.tsx
│       │   │   └── someComponent.scss
│       │   └── index.ts
│       └── login.scss
├── fonts
│   └── SomeFont.ttf
├── reducers
│   ├── index.ts
│   └── someReducer
│       ├── someReducer.test.ts
│       └── someReducer.ts
├── styles
│   ├── animations.scss
│   ├── index.scss
│   └── variables.scss
├── types
│   ├── index.ts
│   └── someFeature.ts
└── utils
    ├── index.ts
    └── someUtil.ts
```

## 外卖食品

1.  如果我们检查一下`actions`文件夹或`reducers`的构造方式，我们可以很快理解这些文件夹保存了我们所有的 reducers 和 actions，但是它们在它们自己不同的文件夹中。这是因为，在这个例子中，虽然所有的 reducers 和动作都影响应用程序全局，但它们都是自己的单元，因此它们都位于自己的子文件夹中。他们有自己的测试，做特定的事情。可以这样想，这可能适用于开发工作环境:所有的开发人员都在同一栋大楼(全局文件夹)下，但是他们每个人都坚持使用自己的办公桌、应用程序团队或功能单元(子文件夹)，并且他们每个人都有自己的笔记本电脑、键盘或监视器(文件)。
2.  **与多个其他代码片段有关系的代码(或者你可以认为这是一个文件中的代码与另一个文件中的代码相关)位于根级别** 通过这样做，我们可以向任何人发出信号，告诉他们这些位于最高级别的*事物*会向下渗透到应用程序的多个位置。
    然而，这不仅仅是在根层次上。如果我们检查一下`containers`文件夹(可以重命名为`pages`、`views`等)中发生了什么。根据您的偏好)，我们还注意到发生了封装关系。那里还有另一个`components`文件夹。唯一构成该特定容器的每个组件都是一个基本特性，它只会在该容器中使用，而不会在其他地方使用。关系是显而易见的；如果这些组件存在于应用程序的根级别之外，但只在一个容器中使用过，你可能会发现自己想知道为什么文件夹遍历比它必须要复杂，或者为什么它与没有关系的*事物*共享空间。
3.  **嵌套不一定是个问题** 这个例子提供了一个 React 应用程序文件结构，它在每个全局文件夹中最多有三层嵌套，这很有用，这样你就不会迷路，但是它说明了应用关系优先结构的简单副作用。如果我们[保持我们的组件简单](https://medium.com/javascript-in-plain-english/8-react-tips-tricks-1f1a429d7da0)，我们的容器文件夹，嵌套最深的那个，将永远不会超过三层。
4.  索引文件是为了干净的导入而存在的。如果您为导入使用别名，这可能是不必要的。

为您的 React 项目想出完美的文件结构并不是不可能的，只是需要一点实验。如果上述内容对您来说太复杂，React 文档还提供了一些关于文件结构的建议:

> 如果你觉得完全卡住了，开始把所有文件放在一个文件夹里。最终，它会变得足够大，以至于您想要将一些文件从其余文件中分离出来。到那时，你将有足够的知识来判断哪些文件是你最常一起编辑的。通常，将经常一起更改的文件放在彼此靠近的地方是一个好主意。

如果有人认为在 React 应用中实现文件结构只有一种方法，那么他们就大错特错了。文件结构因项目和团队而异。您可能会发现上面关于开发文件结构的讨论可能有用，但是当涉及到您自己的代码库时，可能需要一些实验性的推理。请记住，有效的文件结构通常依赖于能够以最有效、最优雅的方式完成以下两件事:

*   文件夹遍历和能够绕过深层嵌套的副作用
*   反映跨代码的影响(如何在页面或其他地方使用组件或模块)，无论是由通用性还是由关系强加的

[1]: [反应文件](https://reactjs.org/docs/faq-structure.html)