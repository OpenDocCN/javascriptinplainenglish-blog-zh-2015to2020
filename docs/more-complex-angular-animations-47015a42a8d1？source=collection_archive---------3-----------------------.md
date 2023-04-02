# 更复杂的角度动画

> 原文：<https://javascript.plainenglish.io/more-complex-angular-animations-47015a42a8d1?source=collection_archive---------3----------------------->

![](img/7b12536356141d3fa06d5c3f733d3ada.png)

Photo by [George Gvasalia](https://unsplash.com/@ggvasalia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究数值变化的转换。

# :过渡中的增量和减量

当数值增加或减少时，我们可以使用选择器值`:increment`和`:decrement`开始转换。

例如，我们可以如下使用它:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { BrowserAnimationsModule } from "@angular/platform-browser/animations";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, BrowserAnimationsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import {
  trigger,
  transition,
  query,
  style,
  stagger,
  animate
} from "@angular/animations";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  animations: [
    trigger("filterAnimation", [
      transition(":increment", [
        query(
          ":enter",
          [
            style({ opacity: 0, width: "0px" }),
            stagger(50, [
              animate("300ms ease-out", style({ opacity: 1, width: "*" }))
            ])
          ],
          { optional: true }
        )
      ]),
      transition(":decrement", [
        query(":leave", [
          stagger(50, [
            animate("300ms ease-out", style({ opacity: 0, width: "0px" }))
          ])
        ])
      ])
    ])
  ]
})
export class AppComponent {
  arr = [];
  add() {
    this.arr.push(this.arr.length + 1);
  } remove() {
    this.arr.pop();
  }
}
```

`app.component.html`:

```
<button (click)="add()">Add</button>
<button (click)="remove()">Remove</button>
<div [@filterAnimation]="arr.length">
  <div *ngFor="let a of arr">{{a}}</div>
</div>
```

在上面的代码中，我们使用了`query`函数来查找内部元素。`stagger`功能用于将每个动画延迟给定的时间。

`style`函数指定动画每个阶段的风格。

在模板中，我们设置`filterAnimation`来观察`arr.length`中的变化。

然后，当我们单击 Add 时，我们将会看到添加了来自`:increment`过渡的缓动动画的数字。

同样，当数字被删除时，我们从`:decrement`转换中单击 Remove 会得到动画。

# 转换中的布尔值

我们可以通过观察布尔值的变化来创建动画。

例如，我们可以如下使用它:

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
export class AppComponent {}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : ''}}
</div>
```

在上面的代码中，我们有带有样式的`true`和`false`状态。

然后我们添加了一个 500 毫秒的动画过渡。

最后，在我们的模板中，我们应用了我们的`openClose`触发器，它在我们单击切换按钮时被触发。

当我们点击按钮时，我们应该看到不同高度的绿色和黄色之间的 div 切换。文本也会打开和关闭。

# 多个动画触发器

我们可以为一个组件定义多个动画触发器。我们可以将动画触发器附加到不同的元素上，元素之间的父子关系会影响动画运行的方式和时间。

![](img/ffd63971ba852f02a68687ba25280c7a.png)

Photo by [Steven Roe](https://unsplash.com/@steveroe_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 禁用 HTML 元素上的动画

我们可以使用`@.disabled`触发器来禁用元素上的动画。

例如，我们可以将上面的示例更改如下:

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
  isDisabled = true;
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@.disabled]="isDisabled">
  <div [@openClose]="show ? true: false">
    {{show ? 'foo' : ''}}
  </div>
</div>
```

在上面的代码中，我们在`AppComponent`中将`isDisabled`设置为`true`。

然后在模板中，当`isDisabled`被设置为`true`时，我们用`[@.disabled]=”isDisabled”`来禁用外部 div 中的动画。

因此，当我们单击切换按钮时，我们不会看到动画。

## 禁用所有动画

我们可以用`@HostBinding`装饰器禁用组件中的所有动画。

为此，我们可以:

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
  @HostBinding("@.disabled")
  isDisabled = true;
}
```

`app.component.html`:

```
<button (click)="show = !show">Toggle</button>
<div [@openClose]="show ? true: false">
  {{show ? 'foo' : ''}}
</div>
```

在上面的代码中，我们有:

```
@HostBinding("@.disabled")
isDisabled = true;
```

在`AppComponent`中。

然后我们不再需要模板中的`@.disabled`绑定来禁用动画。

# 结论

`:increment`和`:decrement`分别观察数值的增加和减少，并相应地显示动画。

当某物分别为`true`或`false`时，我们可以让`true`和`false`状态产生动画效果。

我们可以在模板或组件中禁用带有`@.disabled`绑定的动画。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****