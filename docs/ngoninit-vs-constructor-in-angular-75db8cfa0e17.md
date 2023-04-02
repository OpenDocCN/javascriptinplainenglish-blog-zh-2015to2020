# NgOnInit 与 Angular 中的构造函数

> 原文：<https://javascript.plainenglish.io/ngoninit-vs-constructor-in-angular-75db8cfa0e17?source=collection_archive---------2----------------------->

![](img/0057afa3c84e3d8105eec1957414f663.png)

Photo by Uriel Soberanes on Unsplash

每当我们在 Angular 应用程序中使用组件时，在某一点上，我们会发现，我们编写的一些东西可以合理地放在`ngOnInit`阶段或构造器阶段。这提出了以下问题:

*   什么时候用`ngOninit`什么时候用构造函数？
*   它们之间有什么区别？

首先，让我解释一下什么是构造函数和`ngOnInit` 方法

# 构造器

构造函数是一个函数，每当创建一个类的实例时都会被调用。它不需要任何角度代码。它内置于 Typescript 中。

所以如果我们有类用户

```
class User{
    username:string;
    password:string;

    constructor(){
       console.log("I've been called");
    }
}
```

每当我们创建一个类实例时

```
const user1 = new User();
```

我们的构造函数将被调用

## 如果我们跳过一个构造函数呢？

什么都不会发生。将调用默认的构造函数，如下所示

```
constructor(){}
```

## 组件作为类

Angular 中的组件只不过是一个 typescript 类。所以 Angular 必须创建它，就像我们在用户类上做的那样(可能是更复杂的方式)。

让我们深入研究 Angular 源代码，以检验我们的假设。

在 angular 应用程序中添加子组件

```
[@Component](http://twitter.com/Component)({
  selector: "app-child",
  template: `<h1>I am child</h1>`,
  styleUrls: ["./child.component.scss"],
})
export class ChildComponent implements OnInit {
  constructor() {
    debugger;
  }
}
```

将它添加到视图中的某个位置，运行到浏览器中，并打开开发人员工具。如果我们上一步调用堆栈，我们将看到一个名为`createClass`的方法，其实现如下

Angular 所做的是，它以最简单的方式启动了我们的课程。它只是调用我们的构造函数，如果我们有一些注入，它会尝试解析它们并为我们注入。

所以现在，我们的组件只有注入和我们定义的字段。组件没有任何视图或数据绑定值。只是简单的类及其字段。

现在让我们看看`ngOnInit` 是如何工作的

# ngOnInit 在行动

`ngOnInit` 无非是我们对一个类的自定义方法。对于 Typescript 来说，这并不意味着什么，对于 Javascript 来说，这也不是什么基本概念。它只是你的应用程序和 Angular 框架之间的一个契约。根据合同，您的应用程序和 Angular 同意，无论 Angular 何时调用`ngOnInit` 方法，您的组件都将被完全初始化。我在组件初始化中的意思是什么？

[*在默认变更检测器第一次检查了指令的数据绑定属性之后，并且在检查任何视图或内容子元素之前，立即调用的回调方法。它只在指令实例化时调用一次。*](https://angular.io/api/core/OnInit#methods)

简单来说，这意味着在输入属性被评估之后，组件视图被初始化之前，`ngOnInit` 被调用

因此，如果我们向组件添加一些输入属性

```
...[@Input](http://twitter.com/Input)()
name:string;...
```

并从父组件传递值

```
<app-child name="John Doe"> </app-child>
```

这个值将被评估，我们可以在这个字段上工作，在我们的`ngOnInit` 方法中。

为了让 Angular 实现这一点，它调用了所有组件的变化检测。如果是第一次检查一个组件，它调用`ngOnInit`方法。

记住`ngOnInit`只是一个想象的方法，它可以不叫`ngOnInit`，而叫`ngIamAlive`。

# NgOnInit 与构造函数的区别

总而言之，构造函数是:

*   本机类型脚本/javascript 功能
*   它在组件的任何事件侦听器之前被调用
*   到那时，一个组件只有注入和定义的字段

一个`ngOnInit`是:

*   一个假想的方法，Angular 和用户应用程序都同意。
*   它是在构造函数方法之后调用的
*   当组件初始化完所有数据绑定字段时

# 什么时候使用 NgOnInit，什么时候使用构造函数？

就我个人而言，我每次都使用`ngOnInit`，因为我在一个方法中看到了整个初始化过程。

在执行速度或性能上没有区别，两者具有完全相同的指标

你可以使用一个构造函数，例如，使用`FormBuilder`类构建你的表单组。

但无论如何，在我的整个职业生涯中，我从来没有更喜欢使用构造函数而不是`ngOnInit`

所以我建议你坚持使用 ngOnInit :P