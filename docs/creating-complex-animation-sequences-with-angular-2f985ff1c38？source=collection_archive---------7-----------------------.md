# 使用角度创建复杂的动画序列

> 原文：<https://javascript.plainenglish.io/creating-complex-animation-sequences-with-angular-2f985ff1c38?source=collection_archive---------7----------------------->

![](img/f43ebc9de24b59980597a87b8d623749.png)

Photo by [Erin Wilson](https://unsplash.com/@erinw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在这篇文章中，我们将看看如何创建复杂的动画，动画多个元素和可重用的动画序列。

# 复杂的动画序列

我们可以制作应用于多个元素的动画坐标序列。

我们可以使用以下函数来控制复杂的动画序列:

*   `query` —查找一个或多个 HTML 元素
*   `stagger` —对多个元素的动画应用级联延迟
*   `group` —并行运行多个动画步骤
*   `sequence` —一个接一个地运行动画步骤

# 使用 query()和 stagger()函数制作多个元素的动画

我们使用`stagger`来定义每个被查询项目之间的时间间隔，这些项目和动画元素之间有一个延迟

`query`函数寻找我们想要制作动画的元素。

例如，当我们的页面如下加载时，我们可以在多个 div 中动画显示文本:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,
  state,
  stagger,
  query
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("pageAnimations", [
      transition(":enter", [
        query("div", [
          style({ opacity: 0, transform: "translateY(-100px)" }),
          stagger(-30, [
            animate(
              "500ms cubic-bezier(0.35, 0, 0.25, 1)",
              style({ opacity: 1, transform: "none" })
            )
          ])
        ])
      ])
    ])
  ]
})
export class AppComponent {}
```

`app.component.html`:

```
<div @pageAnimations>
  <div>
    foo
  </div>
  <div>
    bar
  </div>
</div>
```

在上面的代码中，我们使用了`query`函数来设置 div 的样式，这些 div 的不透明度为 0，并使用`cubic-bezier`效果以可变速度过渡。

我们使用`stagger`函数在每个 div 的动画之间创建了 30 毫秒的时间间隔。

然后，当页面加载时，我们应该看到模板中的`foo`和`bar`都是动态的。

# 使用 group()函数的并行动画

我们可以使用`group`函数将元素的动画组合在一起。

例如，我们可以如下使用它:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,
  state,
  group
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("flyInOut", [
      state(
        "in",
        style({
          width: 120,
          transform: "translateX(0)",
          opacity: 1
        })
      ),
      transition("void => *", [
        style({ width: 10, transform: "translateX(50px)", opacity: 0 }),
        group([
          animate(
            "0.3s 0.1s ease",
            style({
              transform: "translateX(0)",
              width: 120
            })
          ),
          animate(
            "0.3s ease",
            style({
              opacity: 1
            })
          )
        ])
      ]),
      transition("* => void", [
        group([
          animate(
            "0.3s ease",
            style({
              transform: "translateX(50px)",
              width: 10
            })
          ),
          animate(
            "0.3s 0.2s ease",
            style({
              opacity: 0
            })
          )
        ])
      ])
    ])
  ]
})
export class AppComponent {}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div @flyInOut *ngIf="show">
  <div>
    foo
  </div>
  <div>
    bar
  </div>
</div>
```

在上面的代码中，我们有带动画的`group`函数调用。

我们有一个用于从`void`到任何东西的转换，另一个用于从任何东西到`void`的转换，分别在项目被添加到 DOM 和从 DOM 中移除时进行动画处理。

在我们的模板中，我们有切换按钮来打开和关闭 div。

然后，当我们打开和关闭 div 时，我们应该会看到传递给`style`函数的参数所指示的翻译效果。

![](img/dda2bd63efe3b93b4cf4dc1b48a84a6f.png)

Photo by [Hans van Tol](https://unsplash.com/@hansvantol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 可重用动画

我们可以通过为动画中指定的值指定变量来创建可重用的动画。

例如，我们可以如下参数化动画样式和计时值:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,
  animation,
  useAnimation
} from "@angular/animations";const transAnimation = animation([
  style({
    height: "{{ height }}",
    opacity: "{{ opacity }}",
    backgroundColor: "{{ backgroundColor }}"
  }),
  animate("{{ time }}")
]);@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      transition("* => void", [
        useAnimation(transAnimation, {
          params: {
            height: 0,
            opacity: 1,
            backgroundColor: "green",
            time: "1s"
          }
        })
      ]),
      transition("void => *", [
        useAnimation(transAnimation, {
          params: {
            height: 0,
            opacity: 1,
            backgroundColor: "red",
            time: "1s"
          }
        })
      ])
    ])
  ]
})
export class AppComponent {}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show" *ngIf="show">
  foo
</div>
```

在上面的代码中，我们用传入的由`style`返回的对象数组通过`animation`函数调用参数化了样式。

然后我们调用了`useAnimation`，将`transAnimation`作为第一个参数传入，并传入了一个具有`params`属性的对象。

在模板中，我们添加了切换按钮和 div 来切换开关。

最后，当我们单击 Toggle 时，当我们单击按钮时，我们会看到 2 个红色和绿色背景的动画。

# 结论

我们可以用`query`函数制作多个元素的动画。

然后我们可以用`stagger`函数错开元素的动画。使用`group`功能制作项目动画。

我们可以使用`sequence`函数一个接一个地运行动画步骤。

为了创建可重用的动画，我们可以使用`animation`函数来创建可重用的动画，然后调用`useAnimation`，将参数作为对象传递给函数的第二个参数来使用它。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的喜爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****