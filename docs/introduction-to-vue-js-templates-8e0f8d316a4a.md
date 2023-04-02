# Vue.js 模板简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-templates-8e0f8d316a4a?source=collection_archive---------2----------------------->

![](img/9cbcaaafeec6c2fbacb504806b9cdf83.png)

Photo by [Tamara Bellis](https://unsplash.com/@tamarabellis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何创建和使用模板来显示数据和处理事件。

# 它是如何工作的

Vue 模板让我们将数据绑定到 DOM。

Vue 将模板编译成虚拟 DOM 渲染函数。它可以计算出要重新呈现的组件的最小数量，并在数据更改时最小化 DOM 操作。

# 插值

数据绑定最基本的形式是带双花括号的文本插值。

例如，我们可以写:

```
<p>{{foo}}</p>
```

显示来自 Vue 实例或组件的`foo`的值。

上面的代码会在`foo`更新的时候更新。

我们可以通过使用如下的`v-once`指令来防止它在第一次渲染后更新:

```
<p v-once>{{foo}}</p>
```

# 原始 HTML

我们也可以用`v-html`指令将原始 HTML 放入元素中。

这让我们可以将 HTML 元素和格式添加到元素中。

例如，如果我们在`src/index.js`中有以下内容:

```
new Vue({
  el: "#app",
  data: {
    rawHtml: "<b>bar</b>"
  }
});
```

以及`index.html`中的以下内容:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <p v-html="rawHtml"></p>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

然后我们将`bar`加粗显示，因为`p`元素的内部 HTML 是`<b>bar</b>`。

当我们使用它时，我们必须净化 HTML，使它不包含任何代码。这是为了防止跨站点脚本攻击，因为当我们向`v-html`指令传入一个值时，如果有代码的话，代码就会运行。

# 属性

我们可以用`v-bind`动态设置 HTML 元素属性的值，如下所示。

在`src/index.js`中，我们写道:

```
new Vue({
  el: "#app",
  data: {
    isButtonDisabled: true
  }
});
```

那么当我们在`index.html`中写下以下内容时:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button v-bind:disabled="isButtonDisabled">Button</button>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

然后，由于我们将`isButtonDisabled`属性设置为`true`，该按钮将被禁用。这也适用于任何其他真值。

另一方面，如果我们将其设置为`false`，我们会看到该按钮被禁用。这也适用于其他 falsy 值，如`null`、`undefined`、0 和空字符串。

# JavaScript 表达式

我们可以在花括号之间添加任何其他 JavaScript 表达式。

例如，我们可以写:

```
{{ number + 10 }}
{{ yes ? 'YES' : 'NO' }}
{{ str.split('').reverse('').join('') }}
```

同样，我们可以将表达式写成`v-bind`指令的值，如下所示:

```
<div v-bind:id="`div-${id}`"></div>
```

语句不会在花括号内运行。例如:

```
{{ let a = 1 }}
```

会给我们一个错误。我们将在控制台中得到错误“避免使用 JavaScript 关键字作为属性名:”let“。

我们也不能在花括号中运行条件语句:

```
{{ if (id) { return id } }}
```

我们将得到错误“避免使用 JavaScript 关键字作为属性名:“if”。

![](img/1a0fe45a6fd03a159af28511d9128e43.png)

Photo by [Pietro De Grandi](https://unsplash.com/@peter_mc_greats?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 指令

指令是带有前缀`v-`的特殊属性。我们可以传入一个 JavaScript 表达式作为值。

`v-for`可以接受多个由`;`分隔的表达式。

例如，我们可以使用`v-if`有条件地显示内容:

```
<p v-if="show">Hello</p>
```

如果`show`是真的，那我们就等着看`Hello`吧。否则不会显示。

# 争论

一些指令带有一个参数，在指令名后用冒号表示。

例如，我们可以通过编写以下代码来设置`a`元素的`href`值:

```
<a v-bind:href="'https://medium.com'">Medium </a>
```

`v-on`指令也有一个参数。例如，我们可以通过编写以下代码来处理元素的 click 事件:

```
<a v-on:click="onClick"> Click Me </a>
```

然后，当我们单击`a`元素时，我们的 Vue 实例或组件中的`onClick`方法将会运行。

# 动态参数

从 Vue.js 2.6.0 开始，我们可以使用 JavaScript 表达式作为指令的参数。

例如，我们可以如下使用它。在`src/index.js`中，我们写道:

```
new Vue({
  el: "#app",
  data: {
    click: `${"cl"}${"ick"}`
  },
  methods: {
    onClick() {
      alert("clicked");
    }
  }
});
```

那么在`index.html`中，我们可以使用如下的动态参数:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <a v-on:[click]="onClick"> Click Me </a>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

这将引用 Vue 实例中`data`属性的`click`属性的值，因此我们实际上有了`v-on:click`。

因此，当我们单击`a`标签时，我们会得到一个显示`clicked`文本的警告框。

动态参数表达式必须计算为字符串或`null`。

括号内的空格和引号也无效。

# 结论

我们可以用各种方式创建模板。最基本的是使用双花括号语法的插值。我们可以将任何 JavaScript 表达式放在括号之间。

为了动态地设置属性值，我们可以使用`v-bind`指令。该值可以是任何 JavaScript 表达式。

一些指令带有一个参数，该参数是指令名称中冒号后面的部分。同样，在这种情况下，指令的值可以是任何 JavaScript 表达式。

从 Vue 2.6.0 开始可以使用动态参数。我们可以通过将表达式放在方括号中来使用它。