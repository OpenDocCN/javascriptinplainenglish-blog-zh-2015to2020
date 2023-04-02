# 角形介绍

> 原文：<https://javascript.plainenglish.io/introduction-to-angular-forms-4f5ce6da55e?source=collection_archive---------7----------------------->

![](img/d9983df7d907279f98ae3b43ff6afb16.png)

Photo by [John Duncan](https://unsplash.com/@jrduncan11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在这篇文章中，我们将看看用 Angular 创建表单的基础。

# 不同种类的形式

Angular 提供了两种不同的方法来处理表单中的用户输入。

它们要么是反应式的，要么是模板驱动的。它们都从视图中捕获事件，验证用户输入，创建表单模型和数据模型进行更新，并提供跟踪更改的方法。

反应型更强。它们更具可扩展性、可重用性和可测试性。

模板驱动的表单对于在应用程序中添加简单的表单非常有用。它们很容易添加，但是它们的伸缩性不如反应式表单。

# 共同基础

这两种形式共享以下构件:

*   `FormControl` —跟踪单个表单控件的值和验证状态
*   `FormGroup` —跟踪表单控件集合的相同值和状态
*   `FormArray` —跟踪表单控件数组的相同值和状态
*   `ControlValueAccessor` —在有角度的`FormControl`实例和本地 DOM 元素之间建立桥梁。

# 表单模型设置

我们可以通过设置输入元素的`formControl`属性来设置反应式表单的表单模型。

例如，我们可以编写以下代码:

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
import { FormControl } from "@angular/forms";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  nameControl = new FormControl("");
}
```

`app.component.html`:

```
<input type="text" [formControl]="nameControl" />
```

在上面的代码中，我们将`ReactiveFormsModule`添加到了`AppModule`中。

然后我们在`AppComponent`中创建了`nameControl`。

最后，我们将`formControl`属性设置为`nameControl`，以将输入连接到`FomControl`。

# 模板驱动表单中的设置

我们可以使用`ngModel`指令将输入值绑定到组件中的 a 字段。

例如，我们可以这样写:

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
```

在上面的代码中，我们在`app.module.ts`中导入了`FormsModule`。

然后我们在`AppComponent`中创建了`name`字段，将数据绑定到该字段。

然后，我们通过`ngModel`将输入值绑定到`name`。

反应式表单和模板驱动表单都提供了模型和输入之间的双向绑定。

![](img/02f99446cce3ee1086cf89f1ad27c787.png)

Photo by [Noah Rosenfield](https://unsplash.com/@noah2199?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 反应形式的数据流

从视图到模型，数据流遵循以下步骤:

1.  用户在输入中键入一些内容
2.  表单输入发出输入事件
3.  控件值访问器监听表单输入上的事件，并将新值发送到`FormControl`实例。
4.  `FormControl`实例通过`valueChanges`可观察对象发出新值。
5.  订阅`valueChanges`接收新值

从模型到视图，流程如下:

1.  用户调用`nameControl.setValue`方法来更新`FormControl`值
2.  `FormControl`实例通过`valueChanges`可观察对象发出新值。
3.  任何`valueChanges`可观察值的订阅者都会收到新值
4.  表单输入元素上的控件值访问器用新值更新元素

# 模板驱动表单中的数据流

模板驱动表单中从视图到模型的数据如下:

1.  用户在输入元素中输入内容。
2.  input 元素发出一个带有输入值的输入事件
3.  附加到输入的控制值访问器触发了`FormControl`实例上的`setValue`方法
4.  `FormControl`实例通过`valueChanges` observable 发出新值
5.  任何`valueChanges`可观察值的订阅者都会收到新值
6.  控制值访问器还调用发出`ngModelChange`事件的`NgModel.viewToModelUpdate`方法
7.  `ngModelChanges`触发模型值更新

从模型到视图，流程如下:

1.  该值在组件中更新
2.  变化检测开始
3.  因为输入值已更改，所以在`NgModel`指令实例上调用了`ngOnChanges`
4.  `ngOnChanges`将一个异步任务排队以设置`FormControl`实例的值
5.  变更检测完成
6.  设置`FormControl`实例值的任务在下一个节拍运行
7.  `FormControl`实例通过`valueChanges`可观察对象发出最新的变化
8.  任何`valueChanges`可观察值的订阅者都会收到新值
9.  控件值访问器更新视图中的表单输入元素

# 结论

我们可以创建角形作为反应或模板驱动的形式。

反应式表单更健壮，模板驱动的表单适合简单的表单。

他们都有相同的部分。控制值访问器获取输入值并将其设置为模型。

## 用简单英语写的 JavaScript 的注释

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[](mailto:submissions@javascriptinplainenglish.com)**给我们，我们会把你添加为作者。**

**我们还推出了三种新的出版物！请关注我们的新出版物: [**AI in Plain English**](https://medium.com/ai-in-plain-english) ，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****