# 角度动画过渡和触发器

> 原文：<https://javascript.plainenglish.io/angular-animations-transitions-and-triggers-83110b873f02?source=collection_archive---------2----------------------->

![](img/8810cb47e488b43835b9b2829a39510d.png)

Photo by [Ray Hennessy](https://unsplash.com/@rayhennessy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们看看如何在运行`*ngIf`时应用转换和触发器。

# 预定义状态和通配符匹配

我们可以使用星号来匹配任何动画状态。这对于定义不管 HTML 元素的开始或结束状态如何都适用的转换非常有用。

例如，我们可以如下使用星号:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  state,
  style,
  transition,
  animate,
  trigger
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      state(
        "open",
        style({
          height: "250px",
          opacity: 1,
          backgroundColor: "pink"
        })
      ),
      state(
        "closed",
        style({
          height: "100px",
          opacity: 0.5,
          backgroundColor: "green"
        })
      ),
      transition("* => closed", [animate("1s")]),
      transition("* => open", [animate("0.5s")])
    ])
  ]
})
export class AppComponent {
  isOpen = true;toggle() {
    this.isOpen = !this.isOpen;
  }
}
```

`app.component.html`:

```
<button (click)="toggle()">Toggle</button>
<div [@openClose]="isOpen ? 'open' : 'closed'">
  {{ isOpen ? 'Open' : 'Closed' }}
</div>
```

在上面的代码中，我们有:

```
transition("* => closed", [animate("1s")]),
transition("* => open", [animate("0.5s")])
```

上面的代码表明，当我们从任何状态转换到`closed`状态时，我们制作了一秒钟的动画。

当我们从任何东西过渡到`open`状态时，我们会制作半秒钟的动画。

# 对多个转换状态使用通配符状态

在我们的转换中可以有两个以上的状态。使用星号在多个状态之间转换，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  state,
  style,
  transition,
  animate,
  trigger
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      state(
        "open",
        style({
          height: "250px",
          opacity: 1,
          backgroundColor: "pink"
        })
      ),
      state(
        "inProgress",
        style({
          height: "130px",
          opacity: 0.75,
          backgroundColor: "orange"
        })
      ),
      state(
        "closed",
        style({
          height: "100px",
          opacity: 0.5,
          backgroundColor: "green"
        })
      ),
      transition("* => closed", [animate("1s")]),
      transition("* => open", [animate("0.5s")]),
      transition("* => inProgress", [animate("2.5s")]),
      transition("inProgress => closed", [animate("1.5s")]),
      transition("* => open", [animate("0.5s")])
    ])
  ]
})
export class AppComponent {
  states = ["open", "closed", "inProgress"];
  index = 0;
  state = "open"; changeState() {
    this.index = (this.index + 1) % this.states.length;
    this.state = this.states[this.index];
  }
}
```

`app.component.html`:

```
<button (click)="changeState()">Toggle</button>
<div [@openClose]="state">
  {{ state }}
</div>
```

在上面的代码中，我们有多个转换状态，并使用通配符来指定动画长度。

我们用`changeState`方法循环遍历各个状态。

我们可以使用`void`状态为进入或离开页面的元素配置转换。

它可以与通配符结合使用。它的工作原理如下:

*   `* => void` —当元素离开视图时应用，不管它在离开之前是什么状态
*   `void => *` —当元素进入视图时应用，不管它进入时呈现什么状态
*   `*`状态匹配包括`void`在内的任何状态。

![](img/a8263c197b0430629333b196917f4848.png)

Photo by [Hawin Rojas](https://unsplash.com/@hawin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 制作进入和离开视图的动画

我们可以通过样式化`in`状态来制作进入和离开视图的动画。

然后，我们将`void`和通配符之间的转换制作成动画，反之亦然，以便分别在添加、移除和添加元素时运行动画。

我们可以这样做:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  state,
  style,
  transition,
  animate,
  trigger
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("flyInOut", [
      state("in", style({ transform: "translateX(0)" })),
      transition("void => *", [
        style({ transform: "translateX(-100%)" }),
        animate(100)
      ]),
      transition("* => void", [
        animate(100, style({ transform: "translateX(100%)" }))
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
  foo
</div>
```

在上面的代码中，我们有一个`flyInOut`触发器，它有一个从`void`到`*`的转换，在插入带有`*ngIf`的 div 时应用。

`in`状态显示了 div 的样式。

然后，在移除 div 时应用另一个转换。

在模板中，我们有一个 div 和一个开关按钮，div 的`*ngIf`里面有单词“foo”。

然后，当我们单击切换按钮时，我们会看到单词“foo”先向右移动，然后消失，因为我们有:

```
transition("* => void", [
  animate(100, style({ transform: "translateX(100%)" }))
])
```

因为 div 正在用`*ngIf`移除。

# :输入和:保留别名

`:enter`是`void => *`的简称，`:leave`是`* => void`的简称。

所以我们可以写:

```
transition ( ':enter', [ ... ] );  // alias for void => *
transition ( ':leave', [ ... ] );  // alias for * => void
```

# 结论

我们可以通过添加`* => void`和`void => *`过渡来制作`*ngIf`过渡的动画。

`*`代表任何状态，`void`是一个元素离开屏幕时的状态。

`:enter`和`:leave`分别是`void => *`和`* => void`的简称。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****