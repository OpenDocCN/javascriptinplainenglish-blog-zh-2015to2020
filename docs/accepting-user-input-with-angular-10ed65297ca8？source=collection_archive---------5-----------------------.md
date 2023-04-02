# 用角度接受用户输入

> 原文：<https://javascript.plainenglish.io/accepting-user-input-with-angular-10ed65297ca8?source=collection_archive---------5----------------------->

![](img/7f7a16981ee6ac42e26832b7c4f34344.png)

Photo by [Andrew Alexander](https://unsplash.com/@aa_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何用 Angular 接受用户输入。

# 用户输入

我们可以使用角度事件绑定来响应任何 DOM 事件。

许多 DOM 事件是由用户输入触发的。绑定让我们提供了一种从用户那里获取输入的方式。

例如，我们可以听到如下的按钮点击声:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  onClickMe() {
    alert("clicked");
  }
}
```

`app.component.html`:

```
<button (click)="onClickMe()">Click me!</button>
```

在上面的代码中，我们使用了`onClickMe`方法来显示警告。

然后在`app.component.html`中，我们添加了一个绑定到点击的按钮，当它被点击时调用`onClickMe`。

# 从$event 对象获取用户输入

DOM 事件携带了对组件有用的信息负载。我们可以通过引用`$event`对象来访问这些信息。

例如，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  keysPressed: string[] = [];
  onKeyUp(event) {
    this.keysPressed.push(event.key);
  }
}
```

`app.component.html`:

```
<input (keyup)="onKeyUp($event)" />
<p>{{keysPressed.join(',')}}</p>
```

在上面的代码中，我们有调用输入的`keyup`事件的`AppComponent`的`onKeyUp`方法。

在`onKeyUp`中，我们将按下的键推入`this.keyPressed`数组。

然后在模板中，我们调用`join`来组合按在一起的按键的字符串。

# 键入$event

对于键盘事件，我们可以使用`KeyboardEvent`类型来键入`$event`。

我们可以用`HTMLInputElement`类型输入元素。

例如，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  values: string[] = [];
  onKeyUp(event: KeyboardEvent) {
    this.values.push((event.target as HTMLInputElement).value);
  }
}
```

`app.component.html`:

```
<input (keyup)="onKeyUp($event)" />
<p>{{values.join(',')}}</p>
```

在上面的`AppComponent`中，我们将`event`设置为`KeyboardEvent`类型，并将`event.target`转换为`HTMLInputElement`类型。

这样，我们可以自动完成，所以我们不太可能犯错误。

然而，将整个`$event`对象传递给组件暴露了太多关于事件的细节，因此在模板和组件之间产生了紧密耦合。

# 从模板引用变量获取用户输入

我们可以使用模板引用变量来获取输入值。

例如，我们可以这样做:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {}
```

`app.component.html`:

```
<input #box (keyup)="null" />
<p>{{box.value}}</p>
```

在上面的代码中，我们将`keyup`处理程序设置为`null`，并将`#box`引用变量添加到输入框中。

然后我们通过引用`box.value`来呈现`#box`输入的值。

当我们在输入框中输入内容时，我们会看到 p 元素中显示的值。

这比使用`$event`对象获取值要好，因为我们不需要访问`$event`对象来获取值。

![](img/774c80a6204a88575cbbf8fadb0d5458.png)

Photo by [Rod Long](https://unsplash.com/@rodlong?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 关键事件过滤

我们可以通过用`key`属性指定键来监听元素中的特定按键。

例如，如果我们想监听 Enter 键的按键，我们可以编写以下代码:

`app.component.html`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  onEnter(value) {
    alert(value);
  }
}
```

`app.component.html`:

```
<input #box (keyup.enter)="onEnter(box.value)" />
```

在上面的代码中，`AppComponent`中的`onEnter`方法将`value`输入到输入框中。

然后在`app.component.html`中，我们将`#box`模板变量添加到输入中，得到:

```
(keyup.enter)="onEnter(box.value)"
```

监听回车键的按键并传递输入到输入中的值，通过调用带有传入值的`onEnter`来显示警报。

# 模糊论

我们可以通过向元素传递事件监听器来监听元素的`blur`事件。

例如，我们可以写:

`app.component.ts`:

```
import { Component } from "@angular/core";@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  value: string;
  update(value) {
    this.value = value;
  }
}
```

`app.component.html`:

```
<input #box (blur)="update(box.value)" />
<p>{{value}}</p>
```

在上面的代码中，我们将`#box`模板变量添加到输入框中，并通过将`(blur)`设置为`AppComponent`中的`update`方法添加了一个`blur`事件监听器。

`update`方法获取一个输入值并将其设置为`this.value`，这样我们就可以在模板中显示它。

然后，当我们在输入中键入一些内容，然后将光标从输入处移开时，我们将看到 p 元素中显示的值。

# 结论

我们可以通过监听各种事件监听器来处理用户输入。

要获取事件发出的事件对象，我们可以引用`$event`对象。

为了更容易地获取输入值，我们可以向模板中的元素添加一个模板引用变量，然后从中获取我们想要的属性。