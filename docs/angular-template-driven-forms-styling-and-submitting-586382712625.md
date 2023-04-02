# 角度模板驱动的表单—样式化和提交

> 原文：<https://javascript.plainenglish.io/angular-template-driven-forms-styling-and-submitting-586382712625?source=collection_archive---------5----------------------->

![](img/c9552a79e062dd5fc9620a9130799f2e.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究如何设计模板驱动表单的样式和处理提交事件。

# 为视觉反馈添加自定义 CSS

我们可以设计模板驱动的表单，让用户知道他们必须做些什么来使输入有效。

例如，我们可以编写以下代码来设计表单的样式:

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
```

`app.component.css`:

```
.ng-valid[required],
.ng-valid.required {
  border-left: 3px solid green;
}.ng-invalid:not(form) {
  border-left: 3px solid red;
}
```

在上面的代码中，我们通过向每个输入添加`required`属性并使用`ngModel`绑定到`model`中的每个字段，创建了一个在`AppComponent`中进行验证的表单。

此外，我们添加了`name`属性，这样我们就可以对我们的字段进行适当的验证。

我们用`ngForm`指令定义了`personForm`模板变量。

然后在`app.component.css`中，我们为`ng-valid[required]`设置样式，当我们的输入有效时，Angular 会应用这些样式。

我们还为`ng-invalid`类添加了样式，并从`ng-invalid`中排除了`form`，这样我们只将边框应用于输入，而不是表单本身。

# 添加验证消息

我们还可以通过使用`ngModel`指令定义模板变量来添加验证消息，然后使用它来获取每个字段的状态。

为此，我们将`app.component.html`改为:

```
<form #personForm="ngForm">
  <input
    name="name"
    placeholder="Name"
    type="text"
    [(ngModel)]="model.name"
    required
    #name="ngModel"
  />
  <div [hidden]="name.valid || name.pristine">
    Name is required
  </div>
  <br />
  <input
    name="address"
    placeholder="Address"
    type="text"
    [(ngModel)]="model.address"
    required
    #address="ngModel"
  />
  <div [hidden]="address.valid || address.pristine">
    Address is required
  </div>
</form>
```

在上面的代码中，我们添加了:

```
#name="ngModel"
```

并且:

```
#address="ngModel"
```

分别输入到姓名和地址。

这些是模板变量，让我们获得每个字段的验证状态，并在字段无效时显示如下内容:

```
<div [hidden]="name.valid || name.pristine">
  Name is required
</div>
```

并且:

```
<div [hidden]="address.valid || address.pristine">
  Address is required
</div>
```

如果字段是原始的，这意味着我们没有输入它，那么我们隐藏消息。如果它们是有效的，我们也隐藏消息。

否则，我们给他们看。

![](img/11bbf2aed524874ffaa63961925bf047.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 提交带有 ngSubmit 的表单

如果一切都有效，那么用户可以提交表单。为了让他们这样做，我们添加了`ngSubmit`事件处理程序来处理表单提交。

我们可以这样做:

`app.component.ts`:

```
import { Component } from "[@angular/core](http://twitter.com/angular/core)";[@Component](http://twitter.com/Component)({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  model = {
    name: "",
    address: ""
  }; onSubmit() {
    alert("form submitted");
  }
}
```

`app.component.html`:

```
<form #personForm="ngForm" (ngSubmit)="onSubmit()">
  <input
    name="name"
    placeholder="Name"
    type="text"
    [(ngModel)]="model.name"
    required
    #name="ngModel"
  />
  <div [hidden]="name.valid || name.pristine">
    Name is required
  </div>
  <br />
  <input
    name="address"
    placeholder="Address"
    type="text"
    [(ngModel)]="model.address"
    required
    #address="ngModel"
  />
  <div [hidden]="address.valid || address.pristine">
    Address is required
  </div>
  <br />
  <button type="submit" [disabled]="!personForm.form.valid">
    Submit
  </button>
</form>
```

在上面的代码中，我们添加了一个`onSubmit`方法来处理表单提交。

然后我们补充道:

```
(ngSubmit)="onSubmit()"
```

来监听提交事件。

然后我们添加一个提交按钮，如下所示:

```
<button type="submit" [disabled]="!personForm.form.valid">
  Submit
</button>
```

以便用户可以提交他们的表单。

如果`personForm`无效，我们需要`[disabled]=”!personForm.form.valid”`来禁用提交按钮。

然后，当我们填写所有内容并单击提交时，我们会得到一个警告框，显示“表单已提交”。

否则，什么都不会发生，我们会得到一个错误显示和红色左边框。

# 结论

我们使用`ngSubmit`让用户提交数据。

为了验证每个字段，我们使用`ngModel`指令为每个输入定义模板变量，然后我们可以得到它们的验证状态。

为了根据输入的有效性来样式化输入，我们可以分别根据有效和无效输入的`ng-valid`和`ng-invalid`来样式化它们。

## 进一步阅读

[](/create-a-multi-page-job-application-form-using-angular-f0b1640f4195) [## 使用 Angular 创建多页工作申请表

### 一步一步的教程，以建立一个多页的工作申请表使用 Angular 和 SurveyJS，一个免费的，开源的…

javascript.plainenglish.io](/create-a-multi-page-job-application-form-using-angular-f0b1640f4195) 

*更多内容看* [***说白了就是 io***](https://plainenglish.io/) *。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)，[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*，*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) ***。****

****对缩放您的软件启动感兴趣*** *？检查出* [***电路***](https://circuit.ooo/?utm=publication-post-cta) *。**