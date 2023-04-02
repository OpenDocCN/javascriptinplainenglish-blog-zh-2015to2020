# 复杂角部件相互作用示例

> 原文：<https://javascript.plainenglish.io/complex-angular-component-interaction-examples-b1f9488a784c?source=collection_archive---------2----------------------->

![](img/fac28728f6142116589fa7786e6b0ebc.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看一些复杂的角分量相互作用的例子。

# 父节点通过局部变量与子节点交互

我们可以为子元素创建模板引用变量，然后在模板中引用该变量来访问父元素中子组件的变量。

例如，我们可以编写以下代码:

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
  person = {};
}
```

`app.component.html`:

```
<app-person #appPerson></app-person>
<button (click)='appPerson.setPerson()'>Set Person</button>
<p>{{appPerson.person.name}}</p>
```

`person.component.ts`:

```
import { Component, Output, EventEmitter } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  person = {}; setPerson() {
    this.person = { name: 'Jane' };
  }
}
```

我们把`person.component.html`留为空白。

在上面的代码中，我们有`PersonComponent`，它是`AppComponent`的子节点。

在`PeronComponent`中，我们有`person`变量和`setPerson`方法。

然后在`app.component.html`中，我们添加了一个`#appPerson`模板变量，这样我们就可以访问它的变量。

然后我们用它在 Set Person 按钮上调用`PersonComponent`的`setPerson`方法。

我们还在 p 元素中显示了来自`PersonComponent`的`appPerson.person.name`。

当我们点击 Set Person 按钮时，p 标签自动更新以显示由`setPerson`设置的`appPerson.person.name`的值。

# Parent 调用@ViewChild()

在前面的例子中，我们访问子组件的局部变量。简单易行，但因为亲子连线都是在父体内完成的，所以受限。

父组件不能访问子组件。

为了在父组件的代码中访问子组件，我们可以使用一个`ViewChild`。

为了使用`ViewChild`，我们可以编写以下代码:

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
import { Component, ViewChild } from '@angular/core';
import { PersonComponent } from './person/person.component';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  @ViewChild(PersonComponent, {static: false})
  private personComponent: PersonComponent;setPerson(){
    this.personComponent.setPerson();
  }
}
```

`app.component.html`:

```
<button (click)='setPerson()'>Set Person</button>
<p>{{personComponent && personComponent.person.name}}</p>
<app-person></app-person>
```

`person.component.ts`:

```
import { Component } from '@angular/core';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  person = {}; setPerson() {
    this.person = { name: 'Jane' };
  }
}
```

我们又把`person.component.html`留空了。

在上面的代码中，我们在`PersonComponent`中有相同的`person`和`setPerson`成员。

然后在`AppComponent`中，我们添加了如下`ViewChild`:

```
@ViewChild(PersonComponent, {static: false})
private personComponent: PersonComponent;
```

从`AppComponent`内直接访问`PersonComponent`。我们需要`PersonComponent`模板中的`app-person`组件，尽管我们没有在其中显示任何东西，以便`AppComponent`可以访问`PersonComponent`。

然后我们就直接调用`AppComponent`的`setPerson`方法中的方法。

此外，我们通过以下方式访问模板中的`app-person`组件:

```
<p>{{personComponent && personComponent.person.name}}</p>
```

显示来自`PersonComponent`的`person.name`属性。

![](img/f3883cd5939328636a87fef20d42341f.png)

Photo by [Regine Tholen](https://unsplash.com/@designbytholen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 父母和孩子通过服务进行交流

父组件和子组件也可以通过共享服务进行通信。

我们可以创建一个服务，然后将数据从父组件监听的子组件发送到服务中的一个可观察对象，这样它就可以获得最新的数据。

为此，我们可以编写以下代码:

`app.module.ts`:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { PersonComponent } from './person/person.component';
import { PersonService } from './person.service';@NgModule({
  declarations: [
    AppComponent,
    PersonComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [PersonService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

`person.service.ts`:

```
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';@Injectable({
  providedIn: 'root'
})
export class PersonService {
  private personSource = new Subject();
  personSource$ = this.personSource.asObservable(); announcePerson(person) {
    this.personSource.next(person);
  }}
```

`app.component.ts`:

```
import { Component } from '@angular/core';
import { PersonService } from './person.service';@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  person = {}; constructor(private personService: PersonService) {
    personService.personSource$.subscribe(
      person => {
        this.person = person;
      });
  }
}
```

`app.component.html`:

```
<app-person></app-person>
<p>{{person.name}}</p>
```

`person.component.ts`:

```
import { Component } from '@angular/core';
import { PersonService } from '../person.service';@Component({
  selector: 'app-person',
  templateUrl: './person.component.html',
  styleUrls: ['./person.component.css']
})
export class PersonComponent {
  constructor(private personService: PersonService) { } setPerson() {
    this.personService.announcePerson({ name: 'Jane' });
  }
}
```

`person.component.html`:

```
<button (click)='setPerson()'>Set Person</button>
```

在上面的代码中，我们有了`PersonService`，在这里我们创建了一个新的`Subject`，并将其转换为可观察值。

在`annoucePerson`方法中，我们接受一个`person`对象，并调用`personSource`上的`next`，通过`personSource$`可观察对象广播`person`对象。

然后在`AppComponent`中，我们订阅了`personSource$`可观察到的写法:

```
personService.personSource$.subscribe(
      person => {
        this.person = person;
      });
```

从可观测值中获取`person`的最新值。

在`person.component.html`中，我们添加了如下按钮:

```
<button (click)='setPerson()'>Set Person</button>
```

当我们点击它时，我们调用`setPerson`方法，该方法从上面的`PersonService`中调用`annoucePerson`方法，该方法带有我们想要传入的对象。

由于`AppComponent`订阅了`personSource$`可观测值，我们应该得到`person.name`的最新值。

因此，当我们单击 Set Person 时，我们应该在屏幕上显示“Jane”。

# 结论

我们可以用许多方式在父组件和子组件之间进行通信。

我们可以在父模板的子组件上添加一个模板变量，然后在父模板上访问它的公共成员。

要访问父组件中子组件的成员，我们可以通过引用父组件中子组件的`ViewChild`来完成。

最后，我们可以创建一个在父组件和子组件之间共享的服务，然后调用`Subject`的`next`方法通过可观察对象广播消息。

然后父订阅可观察值，并从那里获得最新的广播值。