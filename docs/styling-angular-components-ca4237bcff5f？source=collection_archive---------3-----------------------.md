# 角度元件造型指南

> 原文：<https://javascript.plainenglish.io/styling-angular-components-ca4237bcff5f?source=collection_archive---------3----------------------->

![](img/86bdfbacfad8881ee19ee4fd4ea4d3e4.png)

Photo by [Hans van Tol](https://unsplash.com/@hansvantol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在这篇文章中，我们将看看风格的角度组件的方法。

# 组件样式

角度组件使用 CSS 样式。关于 CSS 的一切都适用于角度组件。

我们还可以将组件样式与组件结合起来，这允许比常规样式表更模块化的设计。

# 使用组件样式

除了组件中的模板内容，我们还可以定义 CSS 样式。

一种方法是在我们的组件中使用`styles`属性。

例如，我们可以这样写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styles: [
    `
      h1 {
        font-size: 50px;
      }
    `
  ]
})
export class AppComponent {}
```

`app.component.html`:

```
<h1>Foo</h1>
```

然后我们应该在屏幕上看到 50 像素大的‘Foo’。

嵌套在模板中的任何组件或者投射到组件中的任何内容都不会继承`styles`中的样式。

我们可以在每个组件中使用最有意义的 CSS 类名和选择器。

类名和选择器不会与应用程序其他部分的类名和选择器冲突。

应用程序中其他地方的更改不会影响当前组件的更改。

我们可以把每个组件的 CSS 代码和组件的 TypeScript、HTML 代码放在一起，让项目结构更干净。

此外，我们可以更改或删除 CSS 代码，而无需搜索整个应用程序来查找代码的其他使用位置。

# 特殊选择器

Angular 应用程序可以使用特殊的选择器来设计组件。

它们来自样式化阴影 DOM 的选择器。

## :主机

`:host`伪类选择器的目标是托管组件的元素中的样式，而不是组件模板中的元素。

例如，我们可以写:

```
:host {
  display: block;
  border: 3px solid black;
}
```

我们可以通过使用如下函数形式，使用给定的选择器来设计宿主样式:

```
:host(.active) {
  border-width: 1px;
}
```

## :主机上下文

我们可以使用`:host-context`在组件视图之外的某些条件下应用样式。

它的工作方式和`:host`选择器一样，可以使用函数形式。

对于给定的选择器，它将在祖先组件中一直查找到文档根。

例如，我们可以写:

```
:host-context(.theme-light) h2 {
  background-color: #fff;
}
```

## `/deep/`、`>>>`和`::ng-deep (Deprecated)`

我们可以使用 `/deep/`、`>>>`和`::ng-deep`通过禁用规则的视图封装将样式应用到组件的外部。

例如，我们可以写:

```
:host /deep/ h3 {
  font-weight: bold;
}
```

# 组件元数据中的样式文件

我们可以设置`styleUrls`指向一个样式表文件来样式化一个组件。

例如，我们可以这样写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {}
```

`app.component.css`:

```
h1 {
  font-size: 50px;
}
```

`app.component.html`；

```
<h1>Foo</h1>
```

在上面的代码中，我们将样式放在`app.component.css`中，并将我们的`styleUrls`指向`app.component.ts`中的那个文件。

我们可以在`styles`和`styleUrls`中指定多个样式文件。

# 模板内联样式

我们可以在模板中放置`style`标签来添加模板内嵌样式。

例如，我们可以写:

`app.component.html`:

```
<style>
  h1 {
    font-size: 50px;
  }
</style>
<h1>Foo</h1>
```

上面的代码将把 h1 的字体大小设置为 50 像素。

# 模板链接标签

我们可以将链接标签添加到模板中，以引用其他样式。

例如，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html"
})
export class AppComponent {}
```

`app.component.html`:

```
<link rel="stylesheet" href="./app.component.css" />
<h1>Foo</h1>
```

我们看到我们从`AppComponent`中移除了`styleUrls`，并放置了一个链接标签来引用 CSS 文件。

样式应该从我们在链接标签中引用的文件中应用。

![](img/014db0c5db3f1d4f7bf1c231bd671ccd.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# CSS @imports

我们可以按照标准的 CSS `@import`规则导入 CSS 文件。

例如，我们可以编写以下代码:

`foo.css`:

```
h1 {
  font-size: 50px;
}
```

`app.component.css`:

```
@import url("./foo.css");
```

`app.component.html`:

```
<h1>Foo</h1>
```

# 外部和全局样式文件

我们可以在`angular.json`中添加外部全局样式，以将其包含在构建中。

注册 CSS 文件，可以放入`styles`部分，默认为`styles.css`。

# 非 CSS 样式文件

Angular CLI 支持在 SASS、LESS 或 style 中构建样式文件。

我们可以在`styleUrls`中用适当的扩展名(`.scss`、`.less`或`.styl`)来指定这些文件，如下例所示:

`app.component.scss`:

```
h1  {
  font-size: 70px;
}
```

`app.component.html`:

```
<h1>Foo</h1>
```

# 查看封装

我们可以通过在组件代码中设置`encapsulation`来控制视图封装是如何工作的。

以下是视图封装的可能性:

*   `ShadowDom`视图封装使用浏览器的本机影子 DOM 实现。组件的样式包含在 Shadow DOM 中
*   `Native`视图封装使用浏览器的本机影子 DOM 实现的现已弃用的版本
*   `Emulated`是默认选项。它通过预处理和重命名 CSS 代码来模拟影子 DOM 的行为，从而有效地将 CSS 限定在组件的视图范围内
*   `None`表示 Angular 不进行视图封装。这些样式被添加到全局样式中。

我们可以如下更改`encapsulation`设置:

`app.component.ts`:

```
import { Component, ViewEncapsulation } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.scss"],
  encapsulation: ViewEncapsulation.Native
})
export class AppComponent {}
```

# 结论

我们可以使用 CSS 来设计有角度的组件。此外，它支持 SASS、LESS 和 Stylus 进行样式设置。

默认情况下，样式的范围是组件的本地范围。然而，我们可以改变它。

我们也可以通过`style`和`link`标签在模板中包含内嵌样式。