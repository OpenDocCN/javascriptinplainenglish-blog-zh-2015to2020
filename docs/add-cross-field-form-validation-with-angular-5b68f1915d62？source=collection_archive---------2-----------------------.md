# 添加带角度的跨字段表单验证

> 原文：<https://javascript.plainenglish.io/add-cross-field-form-validation-with-angular-5b68f1915d62?source=collection_archive---------2----------------------->

![](img/27e3754689625b773264d9b7c752db47.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何添加跨字段验证，以验证依赖于具有角度模板驱动和反应式表单的其他表单字段的表单字段。

# 添加到反应形式

我们可以通过创建一个获取多个表单控件的验证器函数，然后将它们放入我们的表单组中，从而为反应式表单添加跨字段验证。

我们可以编写以下代码来添加跨字段验证器，以验证密码和确认密码字段的值是否匹配:

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

`password-validator.ts`:

```
import { ValidationErrors, FormGroup, ValidatorFn } from "@angular/forms";export const passwordValidator: ValidatorFn = (
  control: FormGroup
): ValidationErrors | null => {
  const password = control.get("password").value;
  const confirmPassword = control.get("confirmPassword").value;
  return password && confirmPassword && password === confirmPassword
    ? null
    : { passwordsNotEqual: true };
};
```

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormControl, FormGroup } from "@angular/forms";
import { passwordValidator } from "./password-validator";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  user = { password: "", confirmPassword: "" };
  userForm; ngOnInit() {
    this.userForm = new FormGroup(
      {
        password: new FormControl(),
        confirmPassword: new FormControl()
      },
      { validators: passwordValidator }
    );
  }
}
```

`app.component.html`:

```
<form [formGroup]="userForm">
  <input formControlName="password" required placeholder="Password" />
  <br />
  <input
    formControlName="confirmPassword"
    required
    placeholder="Confirm Password"
  /> <div
    *ngIf="userForm.errors?.passwordsNotEqual && (userForm.touched || userForm.dirty)"
  >
    Passwords don't match.
  </div>
</form>
```

在上面的代码中，我们有我们的`passwordValidator`函数:

```
export const passwordValidator: ValidatorFn = (
  control: FormGroup
): ValidationErrors | null => {
  const password = control.get("password").value;
  const confirmPassword = control.get("confirmPassword").value;
  return password && confirmPassword && password === confirmPassword
    ? null
    : { passwordsNotEqual: true };
};
```

它获取输入的值，然后检查它们是否相同。如果不是，那么我们返回一个错误对象。否则，我们返回`null`。

然后，我们在`AppComponent`中创建表单，如下所示:

```
this.userForm = new FormGroup(
      {
        password: new FormControl(),
        confirmPassword: new FormControl()
      },
      { validators: passwordValidator }
    );
```

注意，交叉字段`passwordValidator`被添加到第二个参数的对象中。

然后，我们在模板中显示验证消息，如下所示:

```
<div
    *ngIf="userForm.errors?.passwordsNotEqual && (userForm.touched || userForm.dirty)"
  >
  Passwords don't match.
</div>
```

我们检查整个表单而不是单个控件的错误。

![](img/4974e890a2669b1d429164094115332d.png)

Photo by [Berkay Gumustekin](https://unsplash.com/@berkaygumustekin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 向模板驱动的表单添加跨字段验证

要向模板驱动的表单添加跨字段验证，我们必须向表单添加一个指令，该指令运行我们之前创建的跨表单验证器。

例如，我们可以编写以下代码来检查密码和确认密码字段是否输入了相同的值:

`app.module.ts`:

```
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { FormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";
import { PasswordValidatorDirective } from "./password.directive";@NgModule({
  declarations: [AppComponent, PasswordValidatorDirective],
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
  user = { password: "", confirmPassword: "" };ngOnInit() {}
}
```

`password-validator.ts`:

```
import { ValidationErrors, FormGroup, ValidatorFn } from "@angular/forms";export const passwordValidator: ValidatorFn = (
    control: FormGroup
): ValidationErrors | null => {
    const password = control.value['password'];
    const confirmPassword = control.value['confirmPassword'];
    return password && confirmPassword && password === confirmPassword
        ? null
        : { passwordsNotEqual: true };
};
```

`password.directive.ts`:

```
import { Directive } from "@angular/core";
import {
  AbstractControl,
  Validator,
  NG_VALIDATORS,
  ValidationErrors
} from "@angular/forms";
import { passwordValidator } from "./password-validator";@Directive({
  selector: "[appPassword]",
  providers: [
    {
      provide: NG_VALIDATORS,
      useExisting: PasswordValidatorDirective,
      multi: true
    }
  ]
})
export class PasswordValidatorDirective implements Validator {
  validate(control: AbstractControl): ValidationErrors {
    return passwordValidator(control);
  }
}
```

`app.component.html`:

```
<form appPassword #userForm="ngForm">
  <input
    name="password"
    #password="ngModel"
    [(ngModel)]="user.password"
    required
    placeholder="Password"
  />
  <br />
  <input
    name="confirmPassword"
    #confirmPassword="ngModel"
    [(ngModel)]="user.confirmPassword"
    required
    placeholder="Confirm Password"
  /> <div
    *ngIf="userForm.errors?.passwordsNotEqual && (userForm.touched || userForm.dirty)"
  >
    Passwords don't match.
  </div>
</form>
```

在上面的代码中，我们有以下验证器:

```
export const passwordValidator: ValidatorFn = (
    control: FormGroup
): ValidationErrors | null => {
    const password = control.value['password'];
    const confirmPassword = control.value['confirmPassword'];
    return password && confirmPassword && password === confirmPassword
        ? null
        : { passwordsNotEqual: true };
};
```

我们使用`control.value`访问要输入的值。然后，代码的其余部分保持与上一个示例相同。

此外，我们添加了一个指令，以便在表单中使用验证器:

```
@Directive({
  selector: "[appPassword]",
  providers: [
    {
      provide: NG_VALIDATORS,
      useExisting: PasswordValidatorDirective,
      multi: true
    }
  ]
})
export class PasswordValidatorDirective implements Validator {
  validate(control: AbstractControl): ValidationErrors {
    return passwordValidator(control);
  }
}
```

它只是运行验证器函数。我们将该指令添加到了`AppModule`中，这样我们就可以在模板中使用它了。

在模板中，我们将`appPassword`添加到表单中，并将`ngModel`指令和`name`属性添加到输入中，以实现数据绑定和验证。

此外，我们还补充道:

```
#userForm="ngForm"
```

为表单创建一个模板变量，以便我们可以检查输入下方的表单验证状态，并在输入值不匹配时显示消息。

我们通过书写来检查表格中的错误:

```
userForm.errors?.passwordsNotEqual && (userForm.touched || userForm.dirty)
```

只有当我们输入了由`userForm.touched`和`userForm.dirty`条件表示的内容时，我们才会检查。

# 结论

我们可以使用跨域表单验证器将跨域表单验证添加到表单中。

对于反应式表单，我们不需要指令。然而，对于模板驱动的表单，我们需要一个运行验证器的指令，这样我们就可以将验证器指令添加到模板中并运行验证器。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****