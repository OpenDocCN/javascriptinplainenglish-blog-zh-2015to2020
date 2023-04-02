# 角度反应形式—修补值和建筑形式

> 原文：<https://javascript.plainenglish.io/angular-reactive-forms-patching-values-and-building-forms-e35e33ab4666?source=collection_archive---------1----------------------->

![](img/047650eaa66d22cf2e54a2e7ec74ac0e.png)

Photo by [Steve Harvey](https://unsplash.com/@trommelkopf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何更新表单组中的值并使用`FormBuilder`。

# 修补模型值

有两种方法可以更新反应形式的模型值。

我们可以调用单个控件上的`setValue`来更新值，或者调用`FormGroup`上的`patchValue`来替换表单模型中的值。

我们可以这样称呼`setValue`:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormControl, FormGroup } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  personForm = new FormGroup({
    name: new FormControl(""),
    address: new FormGroup({
      street: new FormControl("")
    })
  }); updateName() {
    this.personForm.setValue({ ...this.personForm.value, name: "Jane" });
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input type="text" formControlName="name" placeholder="Name" />
  <div formGroupName="address">
    <input type="text" formControlName="street" placeholder="Street" />
  </div>
</form><button (click)="updateName()">Update Name</button>
```

`setValue`严格遵循表单组的结构，所以我们必须像在`updateName`方法中一样包含所有内容。

如果我们只想更新一些值，我们可以使用如下的`patchValue`方法:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormControl, FormGroup } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  personForm = new FormGroup({
    name: new FormControl(""),
    address: new FormGroup({
      street: new FormControl("")
    })
  }); updateName() {
    this.personForm.patchValue({ name: "Jane" });
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input type="text" formControlName="name" placeholder="Name" />
  <div formGroupName="address">
    <input type="text" formControlName="street" placeholder="Street" />
  </div>
</form><button (click)="updateName()">Update Name</button>
```

在上面的代码中，`updateName`方法更改为:

```
this.personForm.patchValue({ name: "Jane" });
```

正如我们所看到的，我们不必包括所有的字段。我们只包含了我们想要更新的内容。

# 用 FormBuilder 生成表单控件

我们可以使用`FormBuilder`类使创建反应式表单变得更容易。

要用`FormBuilder`创建一个表单，我们可以编写如下代码:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormBuilder } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private fb: FormBuilder) {} personForm = this.fb.group({
    name: [""],
    address: this.fb.group({
      street: [""]
    })
  });
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input type="text" formControlName="name" placeholder="Name" />
  <div formGroupName="address">
    <input type="text" formControlName="street" placeholder="Street" />
  </div>
</form>
```

上面的代码具有与其他示例相同的功能。

我们有一个`personForm`，它的`name`字段在根表单组中，而`street`字段嵌套在`address`表单组中。

# 简单表单验证

为了确保输入的内容完整和正确，我们必须在表单中添加表单验证。

使用反应式表单，我们可以在表单控件中添加一个验证器，并显示整个表单的状态。

我们可以添加如下验证:

`app.component.ts`:

```
import { Component } from "@angular/core";
import { FormBuilder, Validators } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private fb: FormBuilder) {} personForm = this.fb.group({
    name: ["", Validators.required],
    address: this.fb.group({
      street: ["", Validators.required]
    })
  });
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <input type="text" formControlName="name" placeholder="Name" required />
  <div formGroupName="address">
    <input type="text" formControlName="street" placeholder="Street" required />
  </div>
</form>
```

在上面的代码中，我们通过导入`Validator`并在每个字段中使用`required`属性来制作所需的`name`和`street`字段。

此外，我们必须在输入中添加`required`属性，以防止在检查模板后表达式发生变化时出现错误。

# 显示表单状态

我们可以引用表单的`status`属性来获取表单的验证器状态。

为此，我们只需将`app.component.html`改为如下:

```
<form [formGroup]="personForm">
  <p>Form Status: {{ personForm.status }}</p>
  <input type="text" formControlName="name" placeholder="Name" required />
  <div formGroupName="address">
    <input type="text" formControlName="street" placeholder="Street" required />
  </div>
</form>
```

现在表单应该显示它的验证状态。`personForm.status`如果无效则返回`'INVALID'`，如果有效则返回`'VALID'`。

因此，我们应该在表单的第一行看到。

![](img/eb360287bdf44e63d3da7e7d15385b85.png)

Photo by [Heather Lo](https://unsplash.com/@llhheather?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用窗体数组的动态控件

要添加动态控件，我们可以使用`FormArray`类。

我们可以如下使用`FormArray`:

`app.component.ts`:

```
import { Component } from "[@angular/core](http://twitter.com/angular/core)";
import { FormBuilder, FormArray } from "[@angular/forms](http://twitter.com/angular/forms)";[@Component](http://twitter.com/Component)({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  constructor(private fb: FormBuilder) {}
  personForm = this.fb.group({
    names: this.fb.array([this.fb.control("")])
  }); get names() {
    return this.personForm.get("names") as FormArray;
  } addName() {
    this.names.push(this.fb.control(""));
  }
}
```

`app.component.html`:

```
<form [formGroup]="personForm">
  <div formArrayName="names">
    <h3>Names</h3>
    <button (click)="addName()">Add Name</button>
    <div *ngFor="let n of names.controls; let i=index">
      <label>
        Name:
        <input type="text" [formControlName]="i" />
      </label>
    </div>
  </div>
</form>
```

在上面的代码中，我们在`personForm`中创建了`names`表单数组。

然后在模板中，我们用表单中的`*ngFor`遍历控件。

`formControlName`设置为表单控件的索引。

我们还添加了一个 Add Name 按钮来添加一个新的表单控件。

# 结论

当我们想要构建表单时，可以使用`FormBuilder`类来节省时间。

它对于构建动态表单也很有用，因为它需要`FormArrays`。

我们可以用表单组的`status`属性来获取表单的状态。

要更新表单的值，我们可以调用表单组的`setValue`或`patchValue`方法。

**用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[](mailto:submissions@javascriptinplainenglish.com)**给我们，我们会把你添加为作者。**

**我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****