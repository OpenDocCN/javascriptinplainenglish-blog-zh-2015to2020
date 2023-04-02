# Vue.js 事件处理简介—更多修饰符

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-event-handling-more-modifiers-84c3e6b28202?source=collection_archive---------2----------------------->

![](img/a7cb12b2246dd6cfc3b116ae7c2031ad.png)

Photo by [Jakub Sejkora](https://unsplash.com/@jakubsejkora?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何用 Vue.js 处理各种事件，并修改事件处理程序的行为。

# 关键修饰符

当监听键盘事件时，我们经常想要检查特定的键。为此，我们可以添加键名作为`v-on`指令的修饰符。

例如，我们可以如下使用它:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    name: ""
  },
  methods: {
    submit() {
      alert(this.name);
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
      <input v-model="name" v-on:keyup.enter="submit" />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

当输入被聚焦时，上面的代码将监视回车键的按下。然后，当我们按下 Enter 键并在那时释放该键时，我们将得到在警告框中显示的输入内容。

`v-on:keyup.enter=”submit”`仅检查回车键何时松开。

对于超过一个单词的键名，我们将其转换为 kebab case。

例如，如果我们想监听 page up 键的`keyup`事件，我们写:

```
<input v-on:keyup.page-up="onPageUp">
```

然后当释放向上翻页键时，调用`onPageUp`。

## 关键代码

我们也可以把键码作为`v-on`的修饰符来传递。例如，如果我们想在按下 Enter 键后释放它时监听事件，我们可以编写以下代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    name: ""
  },
  methods: {
    submit() {
      alert(this.name);
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
      <input v-model="name" v-on:keyup.13="submit" />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然而，不推荐使用`keyCode`事件，因此新的浏览器可能不支持它。

Vue 中包含常用键码的别名。它们包括:

*   `.enter`
*   `.tab`
*   `.delete`(捕捉“删除”和“退格”键)
*   `.esc`
*   `.space`
*   `.up`
*   `.down`
*   `.left`
*   `.right`

一些键，像箭头键和`.esc`在 IE9 中有不一致的值，所以如果我们的应用程序支持 IE9，这些内置别名应该是首选。

我们还可以定义自己的键码别名，如下所示:

```
Vue.config.keyCodes.f3 = 114
```

那么我们可以使用:

```
v-on:keyup.f3
```

在我们的应用程序中。

![](img/44aac2bd9204c49435148bf5466117e0.png)

Photo by [Wolfgang Lutz](https://unsplash.com/@wolfi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 系统修饰键

从 Vue 2.1.0 或更高版本开始，我们只能在按下相应的修饰键时使用修饰键来触发鼠标或键盘事件监听器:

*   `.ctrl`
*   `.alt`
*   `.shift`
*   `.meta`

meta 键是 Macintosh 键盘上的 command 键。在 windows 键盘上，元键是 Windows 键。在 Sun 工作站键盘上，它是实心菱形键。

例如，如果我们希望 Alt-S 作为快捷键来显示我们所键入内容的警告，我们可以编写以下代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    name: ""
  },
  methods: {
    submit() {
      alert(this.name);
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
      <input v-model="name" [@keyup](http://twitter.com/keyup).alt.83="submit" />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

如果我们希望在按住 Ctrl 键单击按钮时弹出警告，我们可以编写以下代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {},
  methods: {
    sayHi() {
      alert("hi");
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
      <button [@click](http://twitter.com/click).ctrl="sayHi">Say Hi</button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们按住 Ctrl 键单击 Say hi 按钮时，我们会得到一个带有单词“Hi”的警告。

在这两种情况下，当事件发出时，必须按下修饰键。所以在上面的例子中，除了它后面的内容之外，Alt 或 Ctrl 也必须按。

## 。精确修饰符

从 Vue 2.5.0 开始，我们可以使用`.exact`修改器来控制触发事件所需的系统修改器的精确组合。

例如:

```
<button @click.ctrl="onClick">A</button>
```

只有当 Ctrl 键被按下时才会运行`onClick`，如果我们想只在 Ctrl-click 且没有其他键被按下时运行一个方法，我们可以写:

```
<button @click.ctrl.exact="onCtrlClick">A</button>
```

## 鼠标按钮修饰符

从 Vue 2.2.0 开始，可以使用以下鼠标修改器事件:

*   `.left`
*   `.right`
*   `.middle`

当特定的鼠标按钮被按下时，它们就会被触发。

# 结论

我们可以听一个按键动作或带有按键修饰的按键动作组合。

我们可以通过键码或别名来引用它们。

拥有一个以上单词的键名必须转换为烤肉串大小写。

我们也可以用`.exact`听到准确的组合键，用鼠标键修饰键听到鼠标按钮的点击。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。