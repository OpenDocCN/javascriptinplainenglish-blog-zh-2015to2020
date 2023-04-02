# 角度模板驱动的表单简介

> 原文：<https://javascript.plainenglish.io/introduction-to-angular-template-driven-forms-4b6a44427665?source=collection_archive---------15----------------------->

![](img/a0d27f41bdd57edeb18b156bb6783fbc.png)

Photo by [Andy Chilton](https://unsplash.com/@andyc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何构建基本的角度模板驱动的表单。

# 模板驱动的表单

有了模板驱动的表单，我们可以用模板语法构建表单，而不是使用组件中的方法。

它归结为使用`ngModel`创建一个双向绑定来读写表单控件的值，并验证模板而不是组件中的输入。

错误显示在模板中。

例如，我们可以创建一个简单的模板驱动表单，如下所示:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";@NgModule({
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
<input type="text" [(ngModel)]="name" />
<p>{{name}}</p>
```

在上面的代码中，我们在`app.module.ts`中导入了`FormsModule`。

然后我们在`AppComponent`中创建`name`字段，这是我们绑定到双向绑定的模型。当我们在输入中输入一些东西时，它会被自动设置为`name`的值。

当我们以编程方式更改`name`的值时，它也会反映在输入中。

最后，在`app.component.html`中，我们有了带有`ngModel`指令的输入框。

方括号和圆括号的组合表示双向绑定。

我们还有一个带有`name`字段的 p 元素，用于显示我们在输入框中输入的值。

然后，当我们键入名称时，我们会看到它显示在 p 元素中。

# 对 ngModel 使用*ngFor

我们可以用`ngFor`和`ngModel`一起添加多个动态输入。

我们可以一起创建一个带有动态选项的选择元素。

我们可以编写以下代码来制作一个:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  names: string[] = ["Mary", "Jane", "Joe"];
  name: string;
}
```

`app.component.html`:

```
<select required [(ngModel)]="name">
  <option *ngFor="let name of names" [value]="name">{{name}}</option>
</select>
<p>{{name}}</p>
```

在上面的代码中，我们在`AppComponent`中有一个`names`数组，它是我们想要显示的项目。

然后在模板中，我们有了由`ngFor`生成的带有选项的 select 元素。

我们有了`[value]`，这样我们就可以将`name`作为`value`属性的值传递给它。我们也有`{{name}}`来显示给用户选择。

p 元素保持不变，将显示我们从下拉列表中选择的选项。

我们在 select 上也有`required`，这样我们可以在提交之前检查它是否被选中。

最后，我们有一个下拉列表，其中包含姓名条目作为选项。

# NgForm 指令

我们使用`ngForm`指令为表单定义一个模板变量。有了它，我们可以检查模板中的表单状态，还可以将它传递给组件来检查那里的状态。

例如，我们可以在我们的组件中添加一个具有各种字段的模型，然后添加一个找到这些字段并使它们成为必需字段的表单，如下所示:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  model = {
    name: '',
    address: '',
  }
}
```

`app.component.html`:

```
<form #personForm="ngForm">
  <input
    name="name"
    placeholder="Name"
    type="text"
    [(ngModel)]="model.name"
    required
  />
  <br />
  <input
    name="address"
    placeholder="Address"
    type="text"
    [(ngModel)]="model.address"
    required
  />
</form>
<p>{{model | json}}</p>
<p>{{personForm.valid}}</p>
```

在上面的代码中，我们将`AppComponent`中的`model`与`name`和`address`字段放在一起。

然后在我们的模板中，我们有了用`ngForm`指令定义的`personForm`变量。

那么我们有 2 个输入分别用于`name`和`address`。它们都是必需的，因为我们有`required`属性。注意，我们有`name`属性。如果我们想让表单验证工作，这是必需的。

第一个 p 标签显示`model`的值，最下面一个显示`personForm`的验证状态。

现在，当我们留下任何输入空白时，我们将在底部的 p 标签中得到`false`,如果我们填写了所有内容，将得到`true`。

![](img/36fb8b61ba55e81faae4e6cedc518ca2.png)

Photo by [Kolleen Gladden](https://unsplash.com/@rockthechaos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 用 ngModel 跟踪控制状态和有效性

`ngModel`不仅仅是双向数据绑定。它还告诉我们用户是否触摸了控件，值是否改变，或者值是否变得无效。

如果已经访问了一个控件，`ngModel`将在输入上设置`ng-touched`类。否则，它将被设置为`ng-untouched`。

如果控制值已经改变，`ngModel`将在输入上设置`ng-dirty`类。否则，它将被设置为`ng-pristine`。

如果控制值有效，`ngModel`将在输入上设置`ng-valid`类。否则，它将被设置为`ng-invalid`。

# 结论

我们可以使用`ngForm`和`ngModel`指令来构建模板驱动的表单。

`ngForm`为我们的表单定义一个模板变量。一旦我们用`ngForm`为表单定义了一个模板变量，我们就可以获得表单状态。

`ngModel`为我们提供了输入和模型之间的双向绑定。它还根据状态设置表单的类。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****