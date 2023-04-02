# 角分量相互作用综述

> 原文：<https://javascript.plainenglish.io/quick-overview-of-angular-component-interaction-b4ae09f21f36?source=collection_archive---------7----------------------->

![](img/6422d92446347a71005925878d877e53.png)

Photo by [Jason Hafso](https://unsplash.com/@jasonhafso?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看一些角度分量相互作用的例子。

# 使用输入绑定将数据从父节点传递到子节点

我们可以通过在子组件中使用`@Input`操作符将数据从父组件传递到子组件。

例如，我们可以编写以下代码来实现这一点:

`app.module.ts`:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { PersonComponent } from './person/person.component';@NgModule({
  declarations: [
    AppComponent,
    PersonComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

`app.component.ts`:

```
import { Component } from '@angular/core';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  person = { name: 'Jane' }
}
```

`app.component.html`:

```
<app-person [person]='person'></app-person>
```

`person.component.ts`:

```
import { Component, Input } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  @Input() person;
}
```

`person.component.html`:

```
<p>{{person.name}}</p>
```

在上面的代码中，我们有`AppComponent`，它是`PersonComponent`的父节点。

在`AppComponent`中，我们有`person`字段，它被传递到`app-person`的`person`属性中。

那么在`PersonComponent`中，我们有:

```
@Input() person;
```

获取传入的`person`输入值。然后我们用以下内容显示`name`属性:

```
<p>{{person.name}}</p>
```

在模板中。

然后我们应该会在屏幕上看到‘简’。

# 用 Setter 拦截输入属性的变化

我们可以向`@Input`装饰器添加一个 setter 来截取设置的值，并将其更改为其他值。

例如，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from '@angular/core';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  person = { name: 'Jane' }
}
```

`app.component.html`:

```
<app-person [person]='person'></app-person>
<app-person [person]='undefined'></app-person>
```

`person.component.ts`:

```
import { Component, Input } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  private _person = {};
  @Input()
  set person(person) {
    this._person = person || { name: 'Foo' };
  } get person() { return this._person; }
}
```

`person.component.html`:

```
<p>{{person.name}}</p>
```

在上面的代码中，我们为`PersonComponent`中的`person`属性添加了 getters 和 setters。

在 setter 中，我们根据是否定义了`person`来设置`person`。如果不是，那么我们将`this._person`设置为默认对象。否则，我们将`this._person`设置为从父节点传入的任何值。

在 getter 中，我们得到由 return `this._person`设置的`person`值。

第一行应该显示“Jane ”,因为我们将`person`传递给了`app-person`。

第二行应该显示‘Foo ’,因为我们将`undefined`作为`person`的值传递。

![](img/47bc0e5292fcf2a3d1d1b38d9fd3f929.png)

Photo by [Anders Nord](https://unsplash.com/@annoand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 用 ngOnChanges()截取输入属性更改

我们可以使用`ngOnChanges`截取更改，然后通过将值更改为我们想要的值来设置传递到`@Input`字段的值。

例如，我们可以编写以下代码:

`app.component.ts`:

```
import { Component } from '@angular/core';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  person = { name: 'Jane' }
}
```

`app.component.html`:

```
<app-person [person]='person'></app-person>
<app-person [person]='undefined'></app-person>
```

`person.component.ts`:

```
import { Component, Input, SimpleChange } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  @Input() person; ngOnChanges(change: { [propKey: string]: SimpleChange }) {
    this.person = change.person.currentValue || { name: 'Foo' };
  }
}
```

`person.component.html`:

```
<p>{{person.name}}</p>
```

在上面的代码中，我们在`PersonComponent`中使用了`ngOnChanges`钩子。

它采用一个具有变化值的`change`参数。在钩子中，我们检查`change.person.currentValue`是否被定义。

如果不是，那么我们将`this.person`设置为`{ name: ‘Foo’ }`。否则，我们将`this.person`设置为`change.person.currentValue`。

那么我们应该看到和第一个例子一样的东西。

# 父监听子事件

我们可以在子组件中调用 EventEmitter 上的`emit`,然后在父组件中监听发出的事件。

例如，我们可以这样做:

`app.component.ts`:

```
import { Component } from '@angular/core';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  person = {};
}
```

`app.component.html`:

```
<app-person (emittedPerson)='person = $event'></app-person>
<p>{{person.name}}</p>
```

`person.component.ts`:

```
import { Component, Output, EventEmitter } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  @Output() emittedPerson = new EventEmitter(); emitPerson() {
    this.emittedPerson.emit({ name: 'Jane' });
  }
}
```

`person.component.html`:

```
<button (click)='emitPerson()'>Get Person</button>
```

在上面的代码中，`PersonComponent`中有`emitPerson`方法，它调用`emittedPerson`发射器上的`emit`，在那里我们传递有效载荷以发送给父节点。

然后在`app.component.html`中，我们有:

```
<app-person (emittedPerson)='person = $event'></app-person>
```

监听来自`app-person`的`emittedPerson`事件，然后通过调用`emit`将`person`字段设置为从子节点传入的`$event`变量。

然后我们有:

```
<p>{{person.name}}</p>
```

显示发射对象的`name`属性值。

因此，当我们单击 Get Person 按钮时，我们应该看到显示“Jane”。

# 结论

我们可以通过在子节点中使用`@Input`装饰器将数据从父节点传递给子节点。

为了将数据从子节点传递到父节点，我们可以从子节点发出一个事件。然后，父母可以听它，然后运行代码来做一些事情。

`$event`拥有通过调用`emit`传入的对象。

## **用简单英语写的 JavaScript 的一个注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)