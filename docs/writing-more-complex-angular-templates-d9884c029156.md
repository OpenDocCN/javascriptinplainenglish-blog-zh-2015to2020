# 编写更复杂的角度模板

> 原文：<https://javascript.plainenglish.io/writing-more-complex-angular-templates-d9884c029156?source=collection_archive---------0----------------------->

![](img/42878fa2bfff79e29ce33e7a75da3bd2.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将更详细地研究角度模板。

# 语句上下文

语句仅引用组件中的内容，如事件处理方法和组件实例的其他成员。

语句上下文通常是组件实例。

比如`(click)=”toggle()”`中的`toggle`方法调用引用的是`AppComponent`，也就是语句上下文。

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  showText: boolean = false;
  toggle() {
    this.showText = !this.showText;
  }
}
```

`app.component.html`:

```
<button (click)="toggle()">Toggle</button>
<p *ngIf="showText">foo</p>
```

模板上下文名称优先于组件上下文名称。

例如，在下面的代码中:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  names: string[] = ["John", "Jane"];
  deleteName(name) {
    const index = this.names.findIndex(n => n === name);
    this.names.splice(index, 1);
  }
}
```

`app.component.html`:

```
<div *ngFor="let name of names">
  {{name}} <button (click)="deleteName(name)">Delete</button>
</div>
```

那么`app.component.html`中的`name`指的是模板输入变量，而不是组件的`name`属性。

# 陈述指南

模板语句不能引用全局名称空间中的任何内容。他们不能引用`window`或`document`。

还有，他们不能调用`console.log`或者`Math.min`。

我们还应该避免编写复杂的模板语句。逻辑应该在组件中，而不是在模板中。

# 绑定语法

数据绑定控制用户在屏幕上看到的内容，特别是应用程序数据值。

我们应该使用绑定框架来简化数据显示。

Angular 提供了多种数据绑定。它们可以分为以下几类:

*   从源到视图
*   从视图到源
*   双向序列—查看要查看的源

像`{{name}}`这样的插值是从源到视图的单向绑定。

像`(click)`这样的事件是从视图到源的一种方式。

双向绑定用类似`[(name)]`的语法表示。

除了插值之外的绑定类型在等号的左边有一个目标名称，或者被标点符号`[]`或`()`包围，或者以前缀`bind-`、`on-`或`bindon-`开头。

# 数据绑定和 HTML

我们可以使用数据绑定将动态数据添加到 HTML 中。

例如，我们可以为静态 HTML 编写:

```
<div class="special">foo</div>
<img
  src="https://images.unsplash.com/photo-1515879218367-8466d910aaa4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80"
/>
<button disabled>Save</button>
```

如果我们想让 HTML 变得动态，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  isUnchanged: boolean = true;
}
```

`app.component.html`:

```
<button [disabled]="isUnchanged">Save</button>
```

在上面的代码中，我们有一个`isUnchanged`布尔字段，它设置按钮中`disabled`属性的值。

由于`isUnchanged`是`AppComponent`中的`true`，按钮中的`disabled`将是`true`。

![](img/6d055c473afd28276af455fed6cd7dee.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# HTML 属性与 DOM 属性

属性是由 HTML 定义的。属性是从 DOM 节点访问的。

一些 HTML 属性与属性有一对一的映射，比如`id`。

有些属性没有相应的属性，如`aria`属性。

有些 DOM 属性没有对应的 HTML 属性，比如`textContent`。

HTML 属性和 DOM 属性是不同的东西，即使它们有相同的名称。

HTML 属性的唯一作用是初始化元素和指令状态。

模板绑定处理属性和事件，而不是特性。

例如，以下输入元素由一些属性组成:

```
<input type="text" value="Sarah">
```

`type`和`value`都是属性。

要设置`disabled`属性，我们可以使用`attr.disabled`属性进行如下设置:

```
<input [attr.disabled]="condition ? 'disabled' : null">
```

另一方面，如果我们使用`disabled`属性，我们可以写:

```
<input [disabled]="condition">
```

# 绑定类型和目标

数据绑定的目标是 DOM 中的某个东西。目标取决于绑定类型。它可以是元素、组件、指令或事件，有时也可以是属性名。

属性是在元素、组件或指令上设置的。

例如:

```
<div [ngClass]="{'foo': isFoo}"></div>
```

使用`ngClass`指令动态设置 div 的类。如果`isFoo`为真，则`foo`类被设置。

事件包括从元素、组件或指令发出的事件。

例如，如果我们有:

```
<button (click)="onSave()">Save</button>
```

然后，我们将按钮的 click 事件绑定到组件中的`onSave`事件处理程序。当我们点击保存按钮时，`onSave`将被调用。

我们可以向输入添加双向绑定，如下所示:

```
<input [(ngModel)]="name">
```

指令支持从输入到组件的双向绑定。

我们在模板中引用`name`的地方都能看到输入的值。

我们还可以将数据绑定到一些属性，如`aria`属性，如下所示:

```
<button [attr.aria-label]="photo">Photo</button>
```

类和样式也可以以如下特殊方式绑定:

```
<div [class.green]="isGreen">Green</div>
<button [style.color]="isGreen? 'red' : 'green'">
```

我们可以用后跟类名的`class`属性和后跟我们想要设置的属性的`style`属性来设置 div 的类。

# 结论

我们可以用角度模板以多种方式绑定数据，将数据从组件传输到模板，反之亦然。

有单向和双向绑定。单向用于大多数事情，双向用于输入。

我们可以用特殊的方式绑定一些属性，比如类、风格和`aria`。