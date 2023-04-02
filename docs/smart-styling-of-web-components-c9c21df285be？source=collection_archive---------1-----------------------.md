# Web 组件的智能样式

> 原文：<https://javascript.plainenglish.io/smart-styling-of-web-components-c9c21df285be?source=collection_archive---------1----------------------->

## 如何聪明地利用基于状态的样式

![](img/33c8cd9dda95d55227bcf716932966eb.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Web 组件的杀手锏之一是通过影子 DOM 真正封装样式。

这意味着 Web 组件的内部 CSS 可以与周围的文档完全隔离，而不需要特定的命名约定。

组件内部定义的所有样式都只适用于组件，不会从组件中泄漏出来，除了[继承的 CSS 属性](https://www.w3.org/TR/CSS21/propidx.html)之外，组件外部定义的样式不适用于组件。

实际上，这意味着只有那些适用于文本的属性才会被继承，比如`font`、`color`、`line-height`、`text-align`(以及`visibility`)。所有其他样式都不会穿透影子 DOM 边界，而是在 Web 组件内部定义的。

这样做的一个很大的好处是，CSS 可以大大简化，因为每个样式表只适用于单个组件，这样就不需要大量的类名或 BEM 之类的命名约定。

就好像你只需要对迷你文档进行样式化，不需要担心影响其他文档。

根据你的 Web 组件的大小，你通常只需要少量的 HTML 元素，这样你的 CSS 就会保持清晰和简洁。

# 基于状态的样式

在 React 和 StencilJS 组件中使用 JSX 时，我注意到当组件的内部状态改变时，情况通常会变得更复杂，这需要反映在它的 UI 中。

例如，当你有一个按钮的`disabled`属性被设置为`true`时，你会想要向用户显示它被禁用了，可能是通过将`opacity`设置为一个较低的值并将`cursor`设置为`not-allowed`。

通常这是通过应用条件类来实现的:

```
render() {
  return (
    <div id="container" className={this.disabled ? 'disabled' : ''}>
      <button>Save</button>
    </div>
  )
}
```

这是一个简单的例子，但是您可以想象当更多的状态需要在 UI 中反映时，事情会变得非常棘手:

```
render() {
  return (
    <div id="container"
      className={
       `${this.disabled && 'disabled'}
        ${this.processing && 'processing'
        ${this.error && 'error'}
        ...
      `}>
      <button>Save</button>
    </div>
  )
}
```

幸运的是，有可用的[库](https://github.com/lukeed/clsx)来使应用条件类变得更容易，但是通常这仍然会很快变得混乱。

# 将属性反映到属性

Web 组件通过*将属性反映到属性*，为基于状态的样式提供了一个优秀的解决方案。

这意味着 Web 组件的每个或仅某些属性具有相应的属性，每当属性值改变时，该属性就会更新。

这已经是本地 HTML 元素的常见做法，例如`<button>`和`<input>`。

当您在页面上选择一个`<button>`并将它的`disabled`属性设置为`true`时，您会注意到它也获得了一个`disabled`属性:

```
// before
<button>Edit</button>const button = document.querySelector('button');
button.disabled = true;// after
<button **disabled**>Edit</button>
```

我们可以利用这种机制，结合`:host()`选择器和 CSS 属性选择器，根据 Web 组件的状态轻松地设计它们的样式:

```
:host([disabled]) button {
  opacity: 0.5;
  cursor: not-allowed;
}
```

`:host()`选择器可以接受一个任意的选择器，在这种情况下，我们使用属性选择器来定义按钮拥有`disabled`属性时的样式。

因为我们将属性反映到属性，所以我们通常会使用属性选择器，但是当然我们也可以使用其他选择器，例如`:host(.disabled)`。

如果有必要定义仅在多个属性可用时才需要应用的样式，那么可以简单地一次指定这些属性:

```
:host([disabled][active]) {
  ...
}
```

只有当`disabled`和`active`属性都存在时，才会应用上述规则。

我们还可以定义属性有特定值时的样式。例如，我们可能想在按钮被点击时在按钮内部显示一个微调器，并且一个动作正在进行中，比如提交一个表单。

```
#spinner {
  display: none;
}:host([state="processing"]) #spinner {
  display: block;
}// inside the component:
<div id="container">
  <button> 
    Saving...
    <svg id="spinner"></svg>
  </button>
</div>
```

这里，默认情况下微调按钮`svg`是隐藏的，当按钮的`state`属性得到值`processing`时，微调按钮将会显示。

这样，我们可以通过简单地更改 Web 组件的某些属性，轻松地显示和隐藏元素，或者对 UI 进行其他更改。

这是一种简单明了的方法，有很多好处。

*   **没有更多的类与属性保持同步**
    对于类，我们需要为每个属性创建一个类，并确保它们保持同步。通过将属性反射到属性，属性被直接绑定到布局。
*   **更简洁的 HTML**
    基于状态的样式完全由 CSS 完成，因此无需更多代码来切换条件类，从而使 HTML 更简洁。
*   **更高效的 CSS**
    因为所有样式都是使用`:host()`选择器完成的，所以所有 CSS 规则都是从主机元素开始自顶向下编写的。这鼓励 CSS 规则有效地利用 CSS 级联。
    当使用类时，这通常会导致非常具体的 CSS 规则和覆盖。
    我最近使用`:host()`选择器重构了一个包含 400 多行 CSS 的大型 StencilJS 组件，使其包含的 CSS 少于 300 行。CSS 现在不仅更短，而且更容易阅读和理解。
*   **从外部设计样式的能力**
    由于组件的状态现在通过相应的属性从外部“可见”,这也可以用于从外部设计组件。
    例如，当一个`<awesome-button>`组件通过它的`disabled`属性被禁用并且也获得了这个属性时，样式也可以在它被禁用时从外部应用:

```
awesome-button[disabled] {
  pointer-events: none; ...
}<awesome-button disabled></awesome-button>
```

# 如何将属性反映到属性

通过为属性定义一个 setter，可以很容易地将属性反映到属性。每当属性值改变时，我们也在 setter 中改变属性:

```
set disabled(isDisabled) {
  if(isDisabled) {
    this.setAttribute('disabled', '');
  }
  else {
    this.removeAttribute('disabled');
  }
}
```

在这种情况下，`disabled`是一个空属性，所以我们只是根据值来设置或删除属性。

例如，如果我们有一个可以接受特定值的`state`属性，该属性也会接受这个值:

```
set state(value) {
  this.setAttribute('state', value);
}
```

如果我们还希望能够通过属性来更改属性，那么我们可能会尝试将它添加到 Web 组件的`observedProperties`并实现`attributeChangedCallback()`，但这是一个错误，因为这将导致无限循环:

```
class AwesomeButton extends HTMLElement { static get observedProperties() {
    return ['disabled'];
  } constructor() {
    super(); ...
  }  set disabled(isDisabled) {
    if(isDisabled) {
      this.setAttribute('disabled', '');
    }
    else {
      this.removeAttribute('disabled');
    }
  } **// do NOT try to set the property inside the callback 
  // this will invoke the setter which in turn will invoke 
  // attributeChangedCallback which will invoke the setter again 
  // causing an infinite loop**
  attributeChangedCallback(attr, oldVal, newVal) {
    if(attr === 'disabled') {
      this.disabled = this.hasAttribute('disabled');
    }
  }
}
```

相反，我们提供了一个 getter，它将从属性中读取值，因此它们总是同步的:

```
get disabled() {
  return this.hasAttribute('disabled');
}
```

# 基于状态的样式做得很好

当您开始编写通过属性到属性的反射来利用基于状态的样式的 Web 组件时，您会发现组件的 HTML 和 CSS 都将变得更加干净、简洁和易于理解。

您将在一个更高的层次上定义 CSS 规则，从主机开始向下，这将导致更干净的 CSS，具有更少的规则和覆盖。

您将从定义适用于某些属性的一般规则开始:

```
// default styles
:host() {
  ...
}// disabled = true
:host([disabled]) {
  ...
}:host([disabled]) button {
  ...
}// state = 'processing'
:host([state="processing"]) {
  ...
}:host([state="processing"]) .spinner {
  ...
}
```

需要时，您可以应用特定的附加样式或覆盖:

```
// style applied when both state = "processing" and active = true
:host([state="processing][active]) {
  ...
}
```

将某些属性反映到属性中并不总是有意义的，所以当情况不是这样时，最好找到另一种解决方案。此外，请记住，只有接受原始值(如字符串、数字和布尔值)的属性才应该被反射。

例如，反射一个以对象数组作为值的属性是没有意义的，因为这个值需要被序列化，并且没有 CSS 规则可以定位它。

在这种情况下，最好反射到另一个具有原始值的属性。

例如，如果您有一个`<data-table>`组件，它的`data`属性接受一个对象数组，并且您需要在设置它时进行样式化，那么您可以反射到一个空属性`populated`,以指示该组件已经被赋予了数据:

```
// value is an array of objects
set data(value) {
  if(value.length > 0) {
    this.setAttribute('populated', '');
  }
  else {
    this.removeAttribute('populated');
  }
}
```

看看我的[材质按钮](https://github.com/DannyMoerkerke/material-webcomponents/blob/master/src/material-button.js) Web 组件，它大量使用属性到属性的反射来设计灵感。

*你可以* [*在 Twitter 上关注我*](https://twitter.com/dannymoerkerke) *我经常在那里写关于 PWAs、web 组件和现代 Web 功能的文章。*