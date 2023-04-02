# Vue.js 中事件处理的介绍

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-event-handling-89632fcc777e?source=collection_archive---------3----------------------->

![](img/f3bc2d8bd7e0e7c8fc49868799c295c8.png)

Photo by [Tomasz Rynkiewicz](https://unsplash.com/@thmsr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何用 Vue.js 处理各种事件，并修改事件处理程序的行为。

# 收听事件

我们可以使用`v-on`指令来监听 DOM 事件，并在它们被触发时运行一些 JavaScript 代码。

例如，我们可以如下使用它:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    count: 0
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button v-on:click="count += 1">Add 1</button>
      <p>Count: {{count}}</p>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后当我们点击加 1 时，`count`增加 1。

# 方法事件处理程序

我们还可以在事件被触发时运行方法。这给了我们很大的帮助，因为当事件发生时，我们可能需要运行大量的代码，所以我们不能把它们都放在模板的表达式中。

例如，我们可以设置一个方法作为`v-on:click`的值如下:

`src/index.js`:

```
new Vue({
  el: "#app",
  methods: {
    onClick() {
      alert("clicked");
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button v-on:click="onClick">Click Me</button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当单击“单击我”时，会显示一个“已单击”警告框。

注意，我们只需传入函数引用，不必在`v-on:click`指令中调用它。

# 内嵌处理程序中的方法

除了传入方法引用，我们还可以用`v-on:click`指令直接调用方法。

例如，我们可以这样写:

`src/index.js`:

```
new Vue({
  el: "#app",
  methods: {
    onClick(message) {
      alert(message);
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button v-on:click="onClick('hi')">Say Hi</button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

当我们点击“说你好”时，会出现一个警告框，显示“你好”。

有时我们必须访问触发事件的原始 DOM 元素。我们可以使用`$event`对象来做到这一点。

`$event`对象拥有触发事件的数据，包括触发事件的 DOM 元素。

例如，我们可以获取被单击的按钮的 ID，如下所示:

`src/index.js`:

```
new Vue({
  el: "#app",
  methods: {
    onClick(event, message) {
      alert(`${event.target.id} says ${message}`);
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button id="hi-button" v-on:click="onClick($event, 'hi')">Say Hi</button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码将`$event`对象传递给`onClick`处理程序，该处理程序接受设置为`$event`对象的`event`参数，并通过`event.target.id`获取按钮的 ID。字符串`message`也被传入。

然后用字符串``${event.target.id} says ${message}``调用`alert`，所以当我们点击“说你好”时，我们得到一个警告框，显示`hi-button says hi`，因为按钮的 ID 是`hi-button`。

![](img/442f95f9651cc9f4aa10f048bb9f2209.png)

Photo by [Sam Shin](https://unsplash.com/@withbigsmile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 事件修改器

我们可以使用事件修饰符来改变事件处理程序的行为。

例如，`v-on`指令的`.prevent`修饰符将自动对设置为值的事件处理函数运行`event.preventDefault`。

`.prevent`修改器可以如下使用:

```
<form v-on:submit.prevent="onSubmit"> ... </form>
```

然后，当`onSubmit`处理程序在剩余的`onSubmit`代码运行之前运行时，`event.preventDefault()`将会运行。

其他事件修饰符包括:

*   `.stop`
*   `.capture`
*   `.self`
*   `.once`
*   `.passive`

`.stop`在其余事件处理程序代码运行之前运行`event.stopPropagation()`。

`.capture`让我们捕捉事件。也就是说，当我们在内部元素中运行事件处理程序时，相同的事件处理程序也会在外部元素中运行。

例如，如果我们在`src/index.js`中有以下内容:

```
new Vue({
  el: "#app",
  data: {},
  methods: {
    onClick() {
      alert("clicked");
    }
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
  </head><body>
    <div id="app">
      <div>
        <a v-on:click.capture="onClick"> Click Me </a>
      </div>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

然后当我们单击 Click Me 时，我们会看到“clicked”警告框，因为在父元素`div`上调用了`onClick`处理程序。

只有当`event.target`是元素本身时`.self`才会触发事件处理程序。

`.once`最多会触发一次事件处理程序。

例如，如果我们在`src/index.js`中有以下内容:

```
new Vue({
  el: "#app",
  data: {},
  methods: {
    onClick() {
      alert("clicked");
    }
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
      <div>
        <a v-on:click.once="onClick"> Click Me </a>
      </div>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

那么即使我们多次点击“点击我”,我们也只会看到一次“点击”警告框。

`.passive`会将`addEventListener`的`passive`选项设置为`true`。`passive`设置为`true`意味着`preventDefault()`永远不会被调用。

它用于提高移动设备的性能。

# 结论

我们可以用`v-on`指令为 DOM 元素事件设置事件处理程序。

要使用它，我们可以传入一个表达式、事件处理程序方法引用或对事件处理程序的调用作为它的值。

我们还可以获得由`$event`对象触发的事件的数据，在这里我们可以获得被触发事件的目标元素。

最后，我们可以添加事件修饰符来改变事件处理程序的行为。