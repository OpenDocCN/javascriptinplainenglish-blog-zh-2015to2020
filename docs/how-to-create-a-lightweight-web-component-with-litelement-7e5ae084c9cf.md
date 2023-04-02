# 如何用 LitElement 创建轻量级 web 组件

> 原文：<https://javascript.plainenglish.io/how-to-create-a-lightweight-web-component-with-litelement-7e5ae084c9cf?source=collection_archive---------5----------------------->

## 快速构建并试用您的 web 组件

![](img/9a85596cbcb71e796cb999e3747e84f9.png)

Photo of Earth globe with countries by Porapak Apichodilok

在之前的一篇文章中，[我写了一篇关于如何只使用普通 JavaScript 编写 web 组件的文章](https://medium.com/javascript-in-plain-english/create-your-first-web-component-using-vanilla-javascript-82a50cc742b0)。在这篇新帖中，我们将看到如何快速完成，避免样板代码。

我们可以只用普通的 JS 创建一个 web 组件，但是我们很快就会意识到我们重复了很多代码来做同样的事情。LitElement 有助于定义 web 组件，而不必学习定制模板语言和遵循 web 组件标准，因此我们的组件将与每个框架或普通 js 一起工作。

LitElement 使用 [lit-html](https://github.com/Polymer/lit-html) 来定义和呈现 html 模板。Lit-Html 让我们用 JavaScript 编写 Html 模板，然后渲染和*重新渲染*这些模板和数据，以创建和更新 DOM。lit-html 只刷新模板中发生变化的部分。它不会重新渲染整个视图。

事不宜迟，让我们开始使用 LitElement 构建一个简单的 Web 组件。

我们将逐步构建我们的定制组件<my-component>。</my-component>

注意:*为了简单起见，我将使用一个 online 和* [*实时 Html edito*](https://jsbin.com/?html,output) *r 来构建我们的 Web 组件。但是可以随意使用，例如，*[*NodeJs*](https://nodejs.org/en/)*与*[*NPM*](https://docs.npmjs.com/about-npm/)*包管理器。*

要遵循本指南，建议您具备以下基本知识:

*   HTML、CSS 和 Javascript。
*   Javascript 模块
*   箭头功能

目前，您的浏览器不理解<my-component>标签。当浏览器发现像<my-component>这样的未知 HTML 标签时，它会将其呈现为一个内联元素，并继续表示下一个元素。</my-component></my-component>

```
<!DOCTYPE html>
<html lang="en">

<head>
  <title>lit-element code sample</title> 
</head><body>
 **<my-component></my-component>** 
</body></html>
```

首先，我们需要导入 LitElement 基类和 html helper 函数。注意，在 web 上，您可以通过在脚本标签中将 type 属性设置为“module”来告诉浏览器将

```
<!DOCTYPE html>
<html lang="en">

<head>
  <title>lit-element code example</title> 
</head><body>
 **<my-component></my-component>**<script **type="module"**>
    **import {LitElement, html} 
      from 'https://unpkg.com/lit-element?module';**  
  </script></body></html>
```

创建扩展 LitElement 基类的组件类:

```
class MyComponent extends  LitElement {  
   constructor() {
        super();
   }
  ...
}
```

这里，我们将实现“render”方法来为我们的元素设置一个模板。render 方法必须返回“lit-html TemplateResult”要创建“TemplateResult”，我们必须使用“html”辅助函数标记 JavaScript 模板文字:

```
class MyComponent extends  LitElement { 
   **render() {**
    return **html`
**      <!-- template content -->
      <p>Hello! I'm a Web component, 
        Which simulates a counter...
      </p>
      <p>Count: 0</p>`
   }
}
```

现在，我们将在构造函数类中初始化一个计数器属性。

属性是通过一个静态 getter 来定义的。LitElement 将该组件上设置的任何 html 属性解码为可以在 javascript 中使用的属性。对静态属性 ***中定义的属性的任何更改都会触发组件的*** *重新渲染。支持的类型有数字、字符串、布尔和数组对象。*

```
class MyComponent extends  LitElement { 
   constructor() {
        super();
        **this.counter = 0;**
   }

 **static get properties() {
         return {
          counter: {type: Number}
         }
   }** render() {
    return html`
      <p>Hello! I'm a Web component, 
        Which simulates a counter...
      </p>
      <p>Count: </p>**`
  ** }}
```

在下一步中，在类中添加一个方法来增加计数器属性:

```
count(e) {
       this.counter ++;
       console.log( this.counter);
}
```

我们在 html 模板中放置属性计数器和增加它的按钮。

*模板的动态部分通过模板字符串表达式设置。我们可以使用任何有效的 javascript 表达式，让 lit-html 处理 dom 的更新。*

在 litElement 中，我们使用语法:@{onclick，change，…}来注册内联事件处理程序:

```
render() {
...
<button **@click**="${(e) =>  
    this.count(e)}">
    Press me to count!!</button>
 `
 ;
}
```

向我们的组件添加一些样式非常容易:

*样式应该作为静态 getter 添加。它们被评估一次，然后被添加到元素的影子 dom 中。*[*Shadow Dom*](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)*负责确定元素的 CSS 范围，只影响元素的模板，而不影响组件之外的其他方面。*

导入“css”帮助器方法:

```
import {LitElement, html, **css**}
       from 'https://unpkg.com/lit-element?module';
```

并添加静态 styles() getter 方法:

```
class MyComponent extends  LitElement {
 ...
 **static get styles()** {
    return css`
      :host {
        display: block;
      }
     .counter{
        color: red;
      } 
    `;
 ...
}
```

将 css 类添加到 html 标签中:

```
render() {
  ...
  <p **class="counter"**>
    Count:${this.counter}
  </p>
  ...
}
```

最后，我们需要向浏览器注册我们的组件:

```
customElements.define('my-component', MyComponent);
```

我们现在可以在一个在线 html 编辑器中尝试一下。

```
<!DOCTYPE html>
<html lang="en">

<head>
  <title>lit-element code sample</title> 
</head><body>**<my-component></my-component>**<script type="module">

import {LitElement, html, css}
       from 'https://unpkg.com/lit-element?module';class MyComponent extends LitElement { static get properties() {
          return {
            counter: {type: Number}
         }
  } constructor() {
         super();
         this.counter = 0;
       } static get styles() {
       return css`
         :host {
            display: block;
          }
         .counter{
            color: red;
         }        
      `;
    } render() {
     return html`
         <p>Hello! I'm a Web  
            component, 
            Which simulates a                       
            counter...
         </p> <p class='counter'>
          Count:${this.counter}</p> <button [@click](http://twitter.com/click)="${(e) =>  
          this.count(e)}">Press me to 
         counting!!</button>
        `
       ;
 }//end render method count(e) {
       this.counter ++;
       console.log( this.counter);
 }}//end of class

//Register componet 
customElements.define('my-component',   
   MyComponent);</script>

</body>
</html>
```

## 结论

在本文中，我们看到了一个如何构建 Web 组件的简单示例。但是使用 LitElement/lit-html，可以创建任何组件类型。LitElement 非常强大，可以帮助我们更快、更清晰、更少出错地开发代码。

非常感谢你阅读这篇小文章，我希望它对你有用！

## 参考

文学元素