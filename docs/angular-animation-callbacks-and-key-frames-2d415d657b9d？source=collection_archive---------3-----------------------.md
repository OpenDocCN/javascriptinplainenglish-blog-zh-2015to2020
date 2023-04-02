# 角度动画回调和关键帧

> 原文：<https://javascript.plainenglish.io/angular-animation-callbacks-and-key-frames-2d415d657b9d?source=collection_archive---------3----------------------->

![](img/a3b5223d859056480d7dfb9948035e93.png)

Photo by [Regine Tholen](https://unsplash.com/@designbytholen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们来看看动画回调和关键帧。

# 动画回调

动画`trigger`在开始和结束时发出回调。

例如，我们可以通过编写以下代码来记录事件的值:

`app.component.ts`:

```
import { Component, HostBinding } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,
  state
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      state(
        "true",
        style({ height: "200px", opacity: 1, backgroundColor: "yellow" })
      ),
      state(
        "false",
        style({ height: "100px", opacity: 0.5, backgroundColor: "green" })
      ),
      transition("false <=> true", animate(500))
    ])
  ]
})
export class AppComponent {
  onAnimationEvent(event: AnimationEvent) {
    console.log(event);
  }
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div
  [@openClose]="show ? true: false"
  (@openClose.start)="onAnimationEvent($event)"
  (@openClose.done)="onAnimationEvent($event)"
>
  {{show ? 'foo' : ''}}
</div>
```

在上面的代码中，我们有:

```
(@openClose.start)="onAnimationEvent($event)"
(@openClose.done)="onAnimationEvent($event)"
```

分别在动画开始和结束时调用`onAnimationEvent`回调。

然后在我们的`onAnimationEvent`回调中，我们记录了`event`参数的内容。

它对调试很有用，因为它提供了关于动画的状态和元素的信息。

# 关键帧

我们可以在动画中添加关键帧来创建比两阶段动画更复杂的动画。

例如，我们可以这样写:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,  
  keyframes
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      transition('true <=> false', [
        animate('2s', keyframes([
          style({ backgroundColor: 'blue' }),
          style({ backgroundColor: 'red' }),
          style({ backgroundColor: 'orange' })
        ]))
    ])
  ]
})
export class AppComponent {
  onAnimationEvent(event: AnimationEvent) {
    console.log(event);
  }
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : 'bar'}}
</div>
```

在上面的代码中，我们在`AppComponent`中添加了不同风格的关键帧。

它们将按照为正向状态转换列出的顺序运行，为反向状态转换列出的顺序运行。

然后，当我们单击切换时，我们会看到颜色随着文本的变化而变化。

# 抵消

关键帧包括一个偏移，该偏移定义动画中每次样式更改发生的点。

偏移量是从零到一的相对度量。它们标志着动画的开始和结束。

这些是可选的。忽略偏移量时，会自动分配偏移量。

例如，我们可以如下指定偏移:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,  
  keyframes
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      transition('true <=> false', [
        animate('2s', keyframes([
          style({ backgroundColor: 'blue', offset: 0 }),
          style({ backgroundColor: 'red', offset: 0.6 }),
          style({ backgroundColor: 'orange', offset: 1 })
        ]))
    ])
  ]
})
export class AppComponent {
  onAnimationEvent(event: AnimationEvent) {
    console.log(event);
  }
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : 'bar'}}
</div>
```

在上面的代码中，我们向我们的`style`参数对象添加了`offset`属性来改变颜色变化的时间。

与之前相比，颜色变化应该在时间上稍微偏移。

# 有脉动的关键帧

我们可以使用关键帧，通过在整个动画中的特定偏移处定义样式来创建脉冲效果。

要添加它们，我们可以按如下方式更改关键帧的不透明度:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,  
  keyframes
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      transition('true <=> false', [
        animate('1s', keyframes ( [
          style({ opacity: 0.1, offset: 0.1 }),
          style({ opacity: 0.6, offset: 0.2 }),
          style({ opacity: 1,   offset: 0.5 }),
          style({ opacity: 0.2, offset: 0.7 })
        ]))
    ])
  ]
})
export class AppComponent {
  onAnimationEvent(event: AnimationEvent) {
    console.log(event);
  }
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : 'bar'}}
</div>
```

在上面的代码中，我们有具有`opacity`和`offset`属性的`style`参数对象。

`opacity`差异将产生脉动效果。

`offset`将改变`opacity`变化的时间。

然后，当我们点击切换，我们应该看到脉动效果。

![](img/194c8bf95ba491d69b531d35a9ce9aa2.png)

Photo by [Josh Felise](https://unsplash.com/@jfelise?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用通配符的自动属性计算

我们可以将 CSS 样式属性设置为通配符来进行自动计算。

例如，我们可以使用通配符，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  style,
  animate,
  state
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("openClose", [
      state("in", style({ height: "*" })),
      transition("true => false", [
        style({ height: "*", backgroundColor: "pink" }),
        animate(250, style({ height: 0 }))
      ]),
      transition("false => true", [
        style({ height: "*", backgroundColor: "yellow" }),
        animate(250, style({ height: 0 }))
      ])
    ])
  ]
})
export class AppComponent {
  onAnimationEvent(event: AnimationEvent) {
    console.log(event);
  }
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : 'bar'}}
</div>
```

在上面的代码中，我们将样式的`height`设置为通配符，因为我们不想将高度设置为固定高度。

然后，当我们单击“切换”时，我们会看到颜色框随着动画的运行而增长和收缩。

# 结论

我们可以给动画添加回调来调试动画，因为我们可以在那里记录值。

要制作更复杂的动画，我们可以使用关键帧。

偏移可用于更改动画关键帧的计时。

我们可以使用通配符来自动设置 CSS 样式值。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果你有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上你的中级用户名，我们会将你添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****