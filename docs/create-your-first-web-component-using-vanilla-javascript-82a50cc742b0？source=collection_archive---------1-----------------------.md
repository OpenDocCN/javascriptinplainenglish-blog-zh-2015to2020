# 使用普通 JavaScript 创建您的第一个 web 组件

> 原文：<https://javascript.plainenglish.io/create-your-first-web-component-using-vanilla-javascript-82a50cc742b0?source=collection_archive---------1----------------------->

## 仅使用基本的 Web 组件 API

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Image with two monitors with no visible code

Web 组件是低级浏览器特征的集合，它允许我们编写封装的、模块化的、可重用的 HTML 元素。web 组件可以在任何支持基本 HTML 和 JavaScript 的 Web 标准环境中工作，我们可以在用不同技术构建的应用程序中重用。

为了遵循这个指南，你可以使用在线代码编辑器，比如 [jsbin](https://jsbin.com) ，并且最好了解以下基本知识:

*   HTML、CSS 和 Javascript。
*   Javascript 模块
*   箭头功能

Web 组件的主要功能有:

*   自定义元素
*   模板
*   阴影 DOM

我们将逐步构建我们的定制组件<my-component>。</my-component>

```
<my-component>
    <h1>Hello world!</h1>
</my-component>
```

目前，你的浏览器不理解<my-component>标签。当浏览器发现类似<my-component>的未知 HTML 标签时，它会将其呈现为一个内联元素，并继续呈现下一个元素。使用自定义元素 API，我们可以告诉浏览器如何处理新的 HTML 标签。</my-component></my-component>

让我们在元素的底部添加一个脚本标签:

```
<!DOCTYPE html>
<html>
    <body>
        <h1>Hello world!</h1>
     <script>
      /*the code of our 
       component go here.
      */
     </script>
    </body>
</html>
```

为了创建自定义元素，我们将创建一个扩展 HTMLElement 类的类。这个类是支持所有其他 HTML 原生元素的基础，比如元素。

```
class MyComponent extends HTMLElement{
  connectedCallback() {
     console.log('my component is connected!');
  }
}
```

在构建了我们的类之后，我们可以通过在定制元素注册中心定义它来用一个标记名连接它。

```
customElements.define('my-component', MyComponent);
```

当浏览器实例化我们的定制标签时，它触发一些生命周期回调。为简单起见，我们将看到的唯一生命周期方法是<connectedcallback>和<disconnectedcallback>，如下所示:</disconnectedcallback></connectedcallback>

```
class MyComponent extends HTMLElement{
    constructor() {
      super();
      /*called when the class is 
      instantiated
      */
    }**connectedCallback**() {
     /*called when the element is 
      connected to the page.
      This can be called multiple 
      times during the element's
      lifecycle.
      for example when using drag&drop
      to move elements around
     */
   }**disconnectedCallback**() {
   /*called when the element
     is disconnected from the page
   */
}
```

因为我们的类是从 HTMLElement 类扩展而来的，所以当它被实例化时，类实例就是一个实际的动态 DOM 元素。注意，常规 DOM 元素的所有方法和属性也存在于此。

```
class MyComponent extends HTMLElement{
    connectedCallback() {
        this.style.color = 'red';
    }
}
```

为了响应用户输入，我们可以向我们的元素或它的一个子元素添加一个事件监听器。让我们添加一个在单击时改变项目颜色的按钮:

```
class MyComponent extends HTMLElement{
  constructor() {
   super();
   this.addEventListener('click',      
   () => {
         this.style.color === 'red'
         ? this.style.color = 'blue':
          this.style.color = 'red';
   });
  }  
  connectedCallback() {
      this.style.color = 'blue';
  }
}
```

结果代码:

```
<!DOCTYPE html><html>
<body><my-component>
    <h1>Hello Rick!</h1>
</my-component><script>
 class MyComponent extends HTMLElement  
 {
    constructor() {
      super();
      this.addEventListener('click',
      () => {
         this.style.color === 'red'
         ? this.style.color = 'blue':
          this.style.color = 'red';
       });
    } connectedCallback() {
      /*called when the element is 
        connected to the page
      */
      this.style.color = 'blue';
    }
 }customElements.define('my-component', MyComponent);</script>
</body>
</html>
```

对于 Web 组件，我们通常想做的不仅仅是设置一些样式。当用户与组件交互时，我们通常需要呈现组件的更多部分，并更新组件的一些部分。

浏览器为我们提供了一个<template></template>

让我们在 body 标签内创建一个<template></template>

```
<template>
    <h1>Hello Rick!</h1>
</template>
```

正如我们所看到的，“你好，里克！”不再呈现在页面上，因为它内部没有表示或执行任何内容。

我们现在可以恢复并使用它作为模板。我们通过导入来克隆它的内容，并将克隆的元素添加到组件的子组件中。

结果代码:

```
<!DOCTYPE html>
<html>
<body><template>
    <h1>Hello Rick!</h1>
</template>**<my-component></my-component>**<script>
 class MyComponent extends HTMLElement {

  constructor() {
    super();
    this.addEventListener('click',
    () => {
         this.style.color === 'red'
         ? this.style.color = 'blue':
          this.style.color = 'red';
    });
  }connectedCallback() {
   /*called when the element is 
        connected to the page
   */this.style.color = 'blue';const template = 
    document.
      querySelector('template');

    **const clone = 
    *document*.
      importNode(template.content,    
      true);

    this.appendChild(clone);**
  }
 }customElements.define('my-component', MyComponent);</script>
</body>
</html>
```

Web 组件给了我们使用" **ShadowDOM** 的能力，这个特性现在已经内置到浏览器中，所以如果我们向组件的 ShadowDOM 添加子元素，它们将不会是我们元素的子元素。然而，相反，它们将被包裹在一个阴影根中。

这个影子根是一种特殊类型的 DOM 节点，它封装了其中的元素。在这个影子根中定义的样式、脚本和其他元素不会泄漏出去，反之亦然，因此我们实现了封装。

感谢 shadow root，我们可以保证我们的组件总是以相同的方式工作，不管环境如何，因此，我们可以创建可重用的组件。

要在我们的组件中使用 shadow root，请用 this . attach shadow({ mode:' open ' })和 this . shadow root . append child(clone)替换以下行:

最终结果代码:

```
<!DOCTYPE html>
<html>
<body><template>
    <h1>Hello Rick!</h1>
</template>**<my-component></my-component>**<script>class MyComponent extends HTMLElement {constructor() {
    super();
    this.addEventListener('click',
    () => {
         this.style.color === 'red'
         ? this.style.color = 'blue':
          this.style.color = 'red';
    });
   }connectedCallback() {
  /*called when the element is 
    connected to the page
  */this.style.color = 'blue';

  const template = 
   document.querySelector('template');

  const clone =    
  document.
  importNode(template.content, true);

  //this.appendChild(clone);**this.attachShadow({ mode: 'open' });         
  this.shadowRoot.appendChild(clone);** 
   }
 }customElements.define('my-component', MyComponent);</script>
</body>
</html>
```

## 结论

这里我们已经看到了如何使用普通的 JavaScript 构建一个基本的 Web 组件。您可以只使用基本的 web 组件 API 来编写 Web 组件。如果您希望保持低依赖性，使用基本 API 可能是一个不错的选择。否则，建议使用轻量级库来帮助提高开发人员的体验并减少样板文件。

【JavaScript 用简单英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)

如果你喜欢这篇文章，可以考虑通过我的[个人资料](https://kesk.medium.com/membership)订阅 Medium。谢谢大家！