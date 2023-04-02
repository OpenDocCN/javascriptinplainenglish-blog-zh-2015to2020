# 使用角度创建 Web 组件

> 原文：<https://javascript.plainenglish.io/creating-web-components-with-angular-c353413e42da?source=collection_archive---------3----------------------->

![](img/67743de3b6149b5273acd40f2283f8d0.png)

Photo by [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将研究如何创建 Angular 元素，这些元素被打包成 Web 组件。

# 角度元素

大多数浏览器都支持 Web 组件，如 Chrome、Firefox、Opera 或 Safari。

我们可以将 Angular 组件转换成 Web 组件，使所有的 Angular 基础设施对浏览器可用。

像数据绑定和其他角度功能这样的特性被映射到它们的 HTML 等价物。

# 创建和使用自定义元素

我们可以通过创建一个角度组件来创建一个 Web 组件，然后将其构建到 Web 组件中。

要用 Angular 创建一个 Web 组件，我们必须做一些事情。

首先，我们创建一个组件来构建 Web 组件。然后，我们必须将我们创建的组件设置为入口点。

然后我们可以将它添加到 DOM 中。

我们将制作一个自定义组件来获取笑话。为此，我们首先运行:

```
ng g component customJoke
ng g service joke
```

创建我们的组件和服务来获取我们的笑话并显示它。

然后我们运行:

```
ng add @angular/element
```

添加角度元素文件来创建我们的 Web 组件。

然后在`joke.service.ts`中，我们添加:

```
import { Injectable } from '@angular/core';@Injectable({
  providedIn: 'root'
})
export class JokeService { constructor() { } async getJokeById(id: number) {
    const response = await fetch(`http://api.icndb.com/jokes/${id}`)
    const joke = await response.json();
    return joke;
  }
}
```

上面的代码通过 ID 从 Chuck Norris API 获取了一个笑话。

接下来，我们编写组件代码如下:

`app.module.ts`:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule, Injector } from '@angular/core';
import { AppComponent } from './app.component';
import { CustomJokeComponent } from './custom-joke/custom-joke.component';
import { JokeService } from './joke.service';@NgModule({
  declarations: [
    CustomJokeComponent,
    AppComponent
  ],
  imports: [
    BrowserModule,
  ],
  providers: [JokeService],
  bootstrap: [AppComponent],
  entryComponents: [CustomJokeComponent]
})
export class AppModule {}
```

在`AppModule`中，我们将`CustomJokeComponent`添加到`entryComponents`，这样它将成为入口点组件，而不是`AppComponent`。

它将在`custom-joke`元素被创建时加载。

`app.component.ts`:

```
import { Component, Injector } from '@angular/core';
import { createCustomElement, WithProperties, NgElement } from '@angular/elements';
import { CustomJokeComponent } from './custom-joke/custom-joke.component';@Component({
  selector: 'app-root',
  template: ''
})
export class AppComponent {
  constructor(injector: Injector) {
    const JokeElement = createCustomElement(CustomJokeComponent, { injector });
    customElements.define('custom-joke', JokeElement);
    this.showAsElement(20);
  } showAsElement(id: number) {
    const jokeEl: WithProperties<CustomJokeComponent> = document.createElement('custom-joke') as any;
    jokeEl.id = id;
    document.body.appendChild(jokeEl as any);
  }
}
```

`constructor`中的代码创建定制组件，并用我们的`showAsElement`方法将其附加到 DOM。

`createCustomElement`来自我们的`@angular/element`代码。

`showAsElement`方法加载我们之前定义的`custom-joke` Web 组件。

`custom-joke.component.ts`:

```
import { Component, OnInit, ViewEncapsulation, Input } from '@angular/core';
import { JokeService } from '../joke.service';@Component({
  selector: 'custom-joke',
  template: `<p>{{joke?.value?.joke}}</p>`,
  styles: [`p { font-size: 20px }`],
  encapsulation: ViewEncapsulation.Native
})
export class CustomJokeComponent implements OnInit {
  @Input() id: number = 1;
  joke: any = {}; constructor(private jokeService: JokeService) { } async ngOnInit() {
    this.joke = await this.jokeService.getJokeById(this.id)
  }}
```

我们把所有东西都放在一个文件中，这样它们都可以包含在我们的`custom-joke` Web 组件中。

`@Input`将被转换成一个属性，我们可以将一个数字传递给它，并通过它的 ID 获取笑话。

我们将`custom-joke.component.html`和`app.component.html`留空。

![](img/4640974075fc7778494ad620dd305bd6.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们使用`@angular/element`包来创建一个我们可以使用的 Web 组件。

不同之处在于我们内嵌了模板和样式。

此外，我们必须注册组件并将其附加到 DOM。

【JavaScript 用简单的英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)