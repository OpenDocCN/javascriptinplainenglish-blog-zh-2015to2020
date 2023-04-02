# 用角度创建动态表单

> 原文：<https://javascript.plainenglish.io/creating-dynamic-forms-with-angular-b126d3968695?source=collection_archive---------7----------------------->

![](img/1fea1c79264ebaf89f1293fb2c988f8b.png)

Photo by [Vincent van Zalinge](https://unsplash.com/@vincentvanzalinge?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何用 Angular 构建动态表单。

# 动态表单

我们可以使用表单组轻松构建动态表单。

动态表单从源获取数据，然后我们使用角度表单组将它们转换成表单。

一个简单的例子如下:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { ReactiveFormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, ReactiveFormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormControl, FormGroup, Validators } from "@angular/forms";[@Component](http://twitter.com/Component)({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  questions = [
    {
      key: "name",
      question: "What's your name?",
      required: true,
      controlType: "text"
    },
    {
      key: "age",
      question: "What's your age?",
      required: false,
      controlType: "number"
    }
  ];
  questionsForm; constructor() {
    this.questionsForm = this.toFormGroup(this.questions);
  } toFormGroup(questions) {
    let group: any = {}; questions.forEach(question => {
      group[question.key] = question.required
        ? new FormControl(question.value || "", Validators.required)
        : new FormControl(question.value || "");
    });
    return new FormGroup(group);
  }
}
```

`app.component.html`:

```
<div [formGroup]="questionsForm">
  <div *ngFor="let question of questions">
    <label [attr.for]="question.key">{{question.label}}</label> <div>
      <input
        [formControlName]="question.key"
        [id]="question.key"
        [type]="question.controlType"
        [placeholder]="question.question"
      />
    </div> <div *ngIf="questionsForm.controls[question.key]?.errors?.required">
      {{question.question}} is required
    </div>
  </div>
</div>
```

在上面的代码中，我们导入了`AppModuke`中的`ReactiveFormsModule`。

然后在`AppComponent`中，我们有一个`questions`数组，里面有问题类型，不管它是否是必需的，以及我们用于表单控件名称的键。

我们有一个`toFormGroup`对象来遍历`questions`数组，然后用从`questions`条目的字段创建的`FormControl`来创建`FormGroup`。

如果一个`questions`条目的`required`被设置为`true`，那么我们将`Validators.required`验证器添加到我们的`FormGroup`中。

然后在问题被添加为`FormControl` s 后，我们返回`FormGroup`。

然后在我们的模板中，我们用`*ngFor`遍历`questions`数组，然后根据每个`questions`条目中的值设置输入元素中的字段。

控件类型、占位符、ID、`formControlName`均由`question`条目中的值设置。

我们还有:

```
[formGroup]="questionsForm"
```

将我们在`AppComponent`中创建的`questionsForm`与模板中的表单绑定。

为了显示每个字段的验证消息，我们通过编写以下代码来动态获取控件:

```
questionsForm.controls[question.key]
```

其具有`errors`属性。这样，我们就可以得到它们返回的错误。

最后，我们看到一个表单，它有两个字段，分别是姓名和年龄。姓名是文本输入，年龄是由`questions`条目的`controlType`设置的数字输入。

当我们有一个空的名称字段时，我们会得到一个错误消息，直到我们键入一个名称。

![](img/045a28865dc3e9ade9bf770bf02aecaf.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过将来自数据源的值映射到`FormControls`来轻松创建动态表单，我们用它来填充表单组。

在我们的模板中，我们可以用数据源中的内容填充所有字段。

为了获得我们创建的表单组的控件，我们可以在任何地方使用表单组的`controls`来获得我们的控件。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****