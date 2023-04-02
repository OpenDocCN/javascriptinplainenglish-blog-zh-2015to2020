# 如何在 Angular 中使用指令

> 原文：<https://javascript.plainenglish.io/using-built-in-angular-directives-849c4e7fd882?source=collection_archive---------4----------------------->

![](img/be8152654e602d691be5ecc70e633892.png)

Photo by [Regine Tholen](https://unsplash.com/@designbytholen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何使用一些内置的角度指令。

# 内置指令

Angular 有一些内置的指令。内置指令包括属性指令和结构指令。

# 内置属性指令

属性指令改变 HTML 元素、属性、特性和组件的行为。

最常见的属性是:

*   `NgClass` —添加或删除 CSS 类
*   `NgStyle` —添加或删除一组 HTML 样式
*   `NgModel` —向 HTML 表单元素添加双向数据绑定。

# NgClass

我们可以用`ngClass`指令同时添加或删除一些 CSS 类。

例如，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  canSave = true;
  isUnchanged = false;
  isSpecial = true;
  classes = {
    saveable: this.canSave,
    modified: !this.isUnchanged,
    special: this.isSpecial
  };
}
```

`app.component.html`:

```
<div [ngClass]="classes">foo</div>
```

在上面的代码中，我们在`AppComponent`中创建了`classes`字段，这样我们就可以将`saveable`、`modified`和`special`类应用于`app.component.html`中的 div。

当类中的键值为真时，应用 3 个类。

因此，当我们写下:

```
[ngClass]="classes"
```

在`app.component.html`中，我们将看到应用于 div 的类。

如果我们只想切换一个类，我们也可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  isGreen = true;
}
```

`app.component.html`:

```
<div [ngClass]="isGreen ? 'green' : null">foo</div>
```

然后我们将`green`类应用于`app.component.html`中的 div，因为`isGreen`在`AppComponent`中被设置为`true`。

# NgStyle

我们可以使用`ngStyle`为一个元素添加多种样式。这对于在元素上设置动态样式很有用。

要使用它，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  isBig = true;
}
```

`app.component.html`:

```
<div [style.font-size]="isBig ? '25px' : '12px'">foo</div>
```

在上面的代码中，我们将`AppComponent`中的`isBig`字段设置为`true`。

此外，我们将`style.font-size`设置为一个表达式，该表达式检查`isBig`是否为真，然后相应地设置大小。

因此，div 的字体大小应该是 25px。

我们也可以用它来设置多种风格。为此，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  canSave = true;
  isUnchanged = false;
  isSpecial = true;
  styles = {
    "font-style": this.canSave ? "italic" : "normal",
    "font-weight": !this.isUnchanged ? "bold" : "normal",
    "font-size": this.isSpecial ? "24px" : "12px"
  };
}
```

`app.component.html`:

```
<div [ngStyle]="styles">foo</div>
```

在上面的代码中，我们在`AppComponent`中有`styles`字段，用各种 CSS 属性名作为键。它们将根据表达式中的值来设置。

然后在`app.component.html`中，我们用`ngStyle`指令将 div 的样式设置为`styles`对象中的样式。

因此，我们应该看到斜体，粗体和 24px 大文本。

![](img/ad81ff8c11a4dd84f39e5ff112001340.png)

Photo by [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# [(ngModel)]:双向绑定

我们可以使用`ngModel`在模板和组件之间进行双向绑定。

这对于获取输入数据并显示它很有用。

例如，我们可以如下使用它:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { FormsModule } from "@angular/forms";import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  name: string;
}
```

`app.component.html`:

```
<input [(ngModel)]="name" />
<p>{{name}}</p>
```

在上面的代码中，我们将`FormsModule`添加到了`app.module.ts`中的`imports`数组中。

然后我们在`AppComponent`中添加了`name`字段。

最后，我们添加了一个带有`ngModel`指令的 input 元素和一个带有`name`变量的 p 元素。

当我们在输入中输入一些东西时，它会显示在 p 元素中。

# 结论

我们可以使用`ngClass`指令在元素上设置类。

要在元素上设置动态样式，我们可以使用`ngStyle`指令。

为了添加双向数据绑定，我们使用了在`FormsModule`中可用的`ngModel`指令。