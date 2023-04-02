# 角度模板语法介绍

> 原文：<https://javascript.plainenglish.io/brief-intro-to-angular-template-syntax-e05fe4f8c1b2?source=collection_archive---------6----------------------->

![](img/9323bd8f0f10f5d3abdd3d3a89b0b733.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究角度模板语法。

# 模板语法

Angular 模板使用经过修改的 HTML 语法，并添加了一些特定于框架的语法。

几乎所有的 HTML 语法都是有效的语法，除了`script`标签。

`script`标签会被忽略，如果有警告会出现在控制台上。

# 插值和模板表达式

我们可以在模板中插入 JavaScript 表达式，把它们放在双花括号中。

例如，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  name: string = "foo";
}
```

`app.component.html`:

```
{{name}}
```

在模板中显示来自`AppComponent`的`name`字段。

我们应该在屏幕上看到“foo”这个词。

Angular 评估`name`字段的值，然后用该值替换占位符。

此外，我们可以在那里放置表达式。例如，我们可以写:

```
{{1 + 1}}
```

我们看到屏幕上显示的是 2。

我们可以从组件文件调用一个方法，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  getName() {
    return "foo";
  }
}
```

`app.component.html`:

```
{{getName()}}
```

在上面的代码中，我们得到了屏幕上显示的由`getName`返回的值。

然后我们看到屏幕上显示“foo”。

我们可以通过在传递给`@Component`装饰器的对象中设置`interpolation`选项，将由双花括号分隔的插值更改为其他内容。

例如，我们可以通过编写以下代码将内插分隔符更改为单方括号:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
  interpolation: ["[", "]"]
})
export class AppComponent {
  getName() {
    return "foo";
  }
}
```

`app.component.html`:

```
[getName()]
```

然后我们仍然得到我们期望的“foo”显示。

# 模板表达式

模板表达式给出一个值，并出现在双花括号中。

它可以是绑定目标的属性。它可以是 HTML 元素、组件或指令。

我们不能使用具有或促进副作用的表达方式，如:

*   作业(例如`=`、`*=`、…)
*   如`new`、`typeof`、`instanceof`等操作符。
*   用`;`或`,`链接表达式
*   递增和递减运算符`++`和`--`
*   其他一些 ES2015+操作员

也不支持像`|`、`?`或`!`这样的按位运算符和模板表达式运算符。

![](img/bf9263d9c1bdf48b35a230b8ca3b36e4.png)

Photo by [mahdis mousavi](https://unsplash.com/@dissii?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 表达式上下文

表达式上下文通常是组件实例。例如，如果我们有:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent {
  getName() {
    return "foo";
  }
}
```

还有`app.component.html`:

```
{{getName()}}
```

然后`AppComponent`是表达式上下文，`getName`是指`AppComponent`中的`getName`方法。

# 表达指南

表达式应该简单。如果有复杂的逻辑，它应该在组件中。

它们应该运行得很快，这样才不会拖慢我们的应用程序。

此外，表达式不应该改变除了目标属性值以外的任何应用程序状态。

视图应该在单个渲染过程中保持稳定。

它们应该没有副作用。

因此，幂等表达式是理想的，因为它满足我们上面的那些标准。

这些准则的唯一例外是`*ngFor`。它有一个`trackBy`功能，可以在迭代对象时处理对象的引用不平等。

# 模板语句

模板语句是对绑定目标引发事件的响应，如元素、组件或指令。

它在更改组件中的字段时会产生副作用。

下面是一个模板语句的示例:

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

在上面的代码中，`app.component.html`中的`toggle()`是一个模板语句的例子，因为它调用了`AppComponent`中的`toggle`方法来更改`this.showText`的值。

它在响应点击事件时会这样做。

同样，不允许用于表达式的 JavaScript 代码也不允许用于语句。

# 结论

我们使用模板来显示来自组件的数据并接受用户输入。

它由 HTML 语法以及模板表达式和语句组成，模板表达式和语句是从 HTML 代码运行的 JavaScript 代码。