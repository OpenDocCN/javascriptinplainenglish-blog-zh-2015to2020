# 定义和使用角度属性指令

> 原文：<https://javascript.plainenglish.io/defining-and-using-angular-attribute-directives-b4a033f651af?source=collection_archive---------5----------------------->

![](img/d780a5384b4024765948187c2f0dd6f2.png)

Photo by [Boris Smokrovic](https://unsplash.com/@borisworkshop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Angular 是 Google 制作的一个流行的前端框架。像其他流行的前端框架一样，它使用基于组件的架构来构建应用程序。

在本文中，我们将看看如何用 Angular 创建和使用属性指令。

# 构建一个简单的属性指令

我们可以使用 Angular CLI 创建一个简单的指令。

为了创建一个，我们运行:

```
ng generate directive highlight
```

为我们的指令创建框架代码。

一旦我们这样做了，我们就可以为我们的指令写一些代码。我们编写以下代码:

`highlight.directive.ts`:

```
import { Directive, ElementRef } from '@angular/core';@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'pink';
  }
}
```

上面的代码接受指令所应用到的元素的一个`ElementRef`。

然后我们将它的`backgroundColor`设置为`'pink'`。

括号使我们的指令成为属性指令。

我们应该给我们的指令名加上类似`app`的前缀，这样它们就不会和 HTML 属性名冲突。

`app.module.ts`:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HighlightDirective } from './highlight.directive';@NgModule({
  declarations: [
    AppComponent,
    HighlightDirective
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

我们在我们的`AppModule`中添加了`HighlightDirective`，这样我们就可以使用它了。

那么我们可以在`app.component.html`中如下使用它:

```
<div appHighlight>foo</div>
```

然后我们应该看到一个粉红色的 div，里面有“foo”。

# 响应用户发起的事件

我们还可以使用指令来检测和处理用户发起的事件。

例如，我们可以将上面的`HighlightDirective`修改如下:

```
import { Directive, ElementRef, HostListener } from '@angular/core';@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {    } @HostListener('mouseenter') onMouseEnter() {
    this.highlight('pink');
  } @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  } private highlight(color: string){
    this.el.nativeElement.style.color = color;
  }
}
```

我们添加了`HostListener`decorator 来监听我们的`highlight`指令所应用到的元素的`mouseenter`和`mouseleave`事件。

因此，当我们悬停在文本上时，我们会看到它变成粉红色。否则，它会恢复正常颜色。

![](img/f2198ec694e38d5d77fc0b683dd48141.png)

Photo by [Andrew Alexander](https://unsplash.com/@aa_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用@Input 数据绑定将值传递到指令中

我们可以像处理组件一样使用`@Input`装饰器将数据传递给指令。

例如，我们可以这样写:

`highlight.directive.ts`:

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() highlightColor: string;
  constructor(private el: ElementRef) { } ngOnInit() {
    this.el.nativeElement.style.color = this.highlightColor;
  }
}
```

`app.component.html`:

```
<div appHighlight highlightColor='pink'>foo</div>
```

在上面的代码中，我们添加了`highlightColor`输入，然后在`app.component.html`中传递颜色字符串来设置颜色。

因为我们将`highlightColor`设置为粉色，所以我们将得到粉色文本。

# 绑定到@Input 别名

如果我们把别名`@Input`改成与指令名相同的名字，我们可以使上面的代码更短。

例如，我们可以写:

`highlight.directive.ts`:

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input('appHighlight') highlightColor: string;
  constructor(private el: ElementRef) { } ngOnInit() {
    this.el.nativeElement.style.color = this.highlightColor;
  }
}
```

那么我们可以在`app.component.html`中如下使用；

```
<div appHighlight='pink'>foo</div>
```

正如我们所看到的，我们不再需要单独指定`highlightColor`。

# 绑定到第二个属性

我们可以将多个`@Input`绑定到多个属性。

例如，我们可以这样写:

`highlight.directive.ts`:

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input('appHighlight') highlightColor: string;
  @Input() defaultColor: string;
  constructor(private el: ElementRef) { } ngOnInit() {
    this.el.nativeElement.style.color = this.highlightColor || this.defaultColor;
  }
}
```

在上面的代码中，我们有两个`@Inputs`，而不是一个。

现在我们可以在`app.component.html`中如下使用它:

```
<div appHighlight='pink' defaultColor='yellow'>foo</div>
<div appHighlight defaultColor='green'>bar</div>
```

在上面的代码中，我们在第二个 div 中没有设置任何颜色字符串的情况下应用了`appHighlight`指令。

因此，由于`defaultColor`被设置为`'green'`，它将按照我们在`highlight.directive.ts`的`ngOnInit`方法中指定的方式被应用。

# 结论

我们通过运行`ng g directive`来定义属性指令，并用名字外面的括号给它们命名。

然后我们可以用`@Input` s 传入属性，用`@HostListener`监听事件。

此外，我们获取指令所应用的元素，然后对其应用我们想要的任何样式。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。