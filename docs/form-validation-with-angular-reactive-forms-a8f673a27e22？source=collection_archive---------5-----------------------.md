# 角反应形式的形式验证

> 原文：<https://javascript.plainenglish.io/form-validation-with-angular-reactive-forms-a8f673a27e22?source=collection_archive---------5----------------------->

![](img/c44da0128d50733c503b851d9f635929.png)

Photo by [Erin Wilson](https://unsplash.com/@erinw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究如何向反应式表单添加表单验证。

# 反应式表单验证

为了验证反应式表单，我们可以在表单控件对象中添加验证器。

表单控件有两种验证器。同步验证器同步运行代码来检查输入的有效性。它们返回验证错误对象或`null`。

异步验证器返回一个承诺或可观察对象，稍后发出一组验证器或`null`。

# 内置验证器

我们可以使用内置的验证器函数来验证输入。

例如，我们可以创建一个表单组，一个表单控件，然后向其添加验证，如下所示:

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
import { Validators, FormControl, FormGroup } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  person = { name: "" };
  personForm; ngOnInit() {
    this.personForm = new FormGroup({
      name: new FormControl(this.person.name, [
        Validators.required,
        Validators.minLength(4)
      ])
    });
  } get name() {
    return this.personForm.get("name");
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input formControlName="name" required /> <div *ngIf="name.invalid && (name.dirty || name.touched)">
    <div *ngIf="name.errors.required">
      Name is required.
    </div>
    <div *ngIf="name.errors.minlength">
      Name must be at least 4 characters long.
    </div>
  </div>
</form>
```

在上面的代码中，我们如下创建`personForm`:

```
this.personForm = new FormGroup({
      name: new FormControl(this.person.name, [
        Validators.required,
        Validators.minLength(4)
      ])
    });
```

我们在表单组中有一个名为`name`的表单控件，它有必需的和`minLength`验证器。

然后，我们用以下方式将表单绑定到表单组:

```
[formGroup]="personForm"
```

在我们的模板中。

我们对输入做同样的事情，写下:

```
formControlName="name"
```

然后，我们可以使用`name`表单控件来检查验证错误，并相应地将它们显示在输入下方。

# 自定义验证程序

我们可以在表单控件中添加自定义验证器来验证内置验证器中没有的内容。

我们可以这样做:

`banned-name-validator.ts`:

```
import { ValidatorFn, AbstractControl } from "@angular/forms";export function bannedNameValidator(nameRe: RegExp): ValidatorFn {
  return (control: AbstractControl): { [key: string]: any } | null => {
    const banned = nameRe.test(control.value);
    return banned ? { bannedName: { value: control.value } } : null;
  };
}
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import { Validators, FormControl, FormGroup } from "@angular/forms";
import { bannedNameValidator } from "./banned-name-validator";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  person = { name: "" };
  personForm; ngOnInit() {
    this.personForm = new FormGroup({
      name: new FormControl(this.person.name, [
        Validators.required,
        Validators.minLength(4),
        bannedNameValidator(/foo/i)
      ])
    });
  } get name() {
    return this.personForm.get("name");
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input formControlName="name" required /> <div *ngIf="name.invalid && (name.dirty || name.touched)">
    <div *ngIf="name.errors.required">
      Name is required.
    </div>
    <div *ngIf="name.errors.minlength">
      Name must be at least 4 characters long.
    </div>
    <div *ngIf="name.errors.bannedName">
      Name cannot be foo.
    </div>
  </div>
</form>
```

在上面的代码中，我们添加了我们的`bannedNameValidator`函数，该函数返回一个验证器，如果值无效则返回一个错误对象，如果值有效则返回`null`。

我们的 validator 函数接受一个正则表达式，并验证我们没有输入作为正则表达式传入的禁用名称。

然后在`AppComponent`中，我们添加了:

```
bannedNameValidator(/foo/i)
```

这样我们就可以使用我们的验证器了。

在模板中，我们添加了:

```
<div *ngIf="name.errors.bannedName">
  Name cannot be foo.
</div>
```

显示我们的自定义验证消息。

如果我们输入‘foo’或不符合其他表单验证要求的内容，我们会看到显示的验证错误消息。

![](img/1bf0fbd183d3b7a9625608c11222d659.png)

Photo by [Zdeněk Macháček](https://unsplash.com/@zmachacek?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 实现自定义异步验证器

自定义异步验证器返回承诺或可观察值，而不是错误对象或`null`。

如果它返回一个可观测值，它必须在某个点完成。

例如，我们可以如下实现和使用异步验证器函数:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import {  ReactiveFormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, ReactiveFormsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

`banned-name-validator.ts`:

```
import {
  AbstractControl,
  ValidationErrors,
  AsyncValidatorFn
} from "@angular/forms";export function banndNameValidator(bannedName: RegExp): AsyncValidatorFn {
  return (control: AbstractControl): Promise<ValidationErrors | null> => {
    const result = bannedName.test(control.value)
      ? { bannedName: { value: control.value } }
      : null;
    return Promise.resolve(result);
  };
}
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import { Validators, FormControl, FormGroup } from "@angular/forms";
import { banndNameValidator } from "./banned-name-validator";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  person = { name: "" };
  personForm; ngOnInit() {
    this.personForm = new FormGroup({
      name: new FormControl(
        this.person.name,
        [Validators.required, Validators.minLength(4)],
        [banndNameValidator(/foo/i)]
      )
    });
  }get name() {
    return this.personForm.get("name");
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input formControlName="name" required /> <div *ngIf="name.invalid && (name.dirty || name.touched)">
    <div *ngIf="name.errors.required">
      Name is required.
    </div>
    <div *ngIf="name.errors.minlength">
      Name must be at least 4 characters long.
    </div>
    <div *ngIf="name.errors.bannedName">
      Name cannot be foo.
    </div>
  </div>
</form>
```

在上面的代码中，我们有`[banndNameValidator(/foo/i)]`，它被添加为`FormControl`构造函数的第三个参数。

第三个参数包含异步验证器列表。

验证器的功能如下:

```
export function banndNameValidator(bannedName: RegExp): AsyncValidatorFn {
  return (control: AbstractControl): Promise<ValidationErrors | null> => {
    const result = bannedName.test(control.value)
      ? { bannedName: { value: control.value } }
      : null;
    return Promise.resolve(result);
  };
}
```

它返回一个函数，该函数返回一个 promise，该 promise 根据我们传递给 validator 函数的正则表达式检查我们的输入。

如果有输入值的`control.value`无效，我们用一个验证错误对象来解决。否则，我们决心`null`。

然后在我们的模板中，我们有:

```
<div *ngIf="name.errors.bannedName">
  Name cannot be foo.
</div>
```

它显示了我们键入“foo”后的消息。

# 结论

Angular 为表单控件内置了验证器。

我们可以在表单控件中添加验证函数来验证我们的输入。我们可以创建一个同步或者异步验证函数。

如果有错误，同步验证函数返回一个验证错误对象，如果没有错误，返回`null`。

如果有错误，异步验证器返回一个带有验证器错误的 promise 或 observable。否则，我们决定用`null`来表示承诺，或者用`null`来表示可观察到的事情。

## 用简单英语写的 JavaScript 的注释

我们已经推出了三种新的出版物！ [**AI 说白了**](https://medium.com/ai-in-plain-english) ， [**UX 说白了**](https://medium.com/ux-in-plain-english) ，[Python 说白了](https://medium.com/python-in-plain-english) **。跟随你感兴趣的人继续你的成长——谢谢，继续学习！**

我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。**