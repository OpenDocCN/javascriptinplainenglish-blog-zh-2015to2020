# 用 Vue.js 实现悬停效果

> 原文：<https://javascript.plainenglish.io/implementing-hover-effects-with-vue-js-bacc560ca16c?source=collection_archive---------2----------------------->

![](img/b2566e71b91ea08f3ca60ad56d578462.png)

Photo by [christopher lemercier](https://unsplash.com/@elevantarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何在 Vue 中实现鼠标悬停或悬停效果。

# 鼠标悬停或悬停效果

有了 CSS，我们可以使用`hover`伪类来设计悬停效果。例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <style>
      .foo:hover {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1 class="foo">
      foo
    </h1>
  </body>
</html>
```

该代码将红色应用于悬停在`foo`类上的元素。

有了 Vue，事情就没那么简单了。我们必须倾听正确的事件，运用正确的风格。

为了跟踪鼠标的叶子，我们必须使用`mouseleave` 事件。`mouseenter` 事件检测鼠标何时进入一个元素。

`mouseover`事件的工作方式类似于`mouseenter`，但是`mouseover`会冒泡，所以它不会创建很多独特的事件，这使得应用程序运行得更快。

我们可以在 Vue 中创建相同的效果，如下所示:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    hovered: false
  }
});
```

`index.html`:

```
 <!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      .foo-hover {
        color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <h1
        @mouseover="hovered = true"
        @mouseleave="hovered = false"
        :class="{'foo-hover': hovered}"
      >
        foo
      </h1>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有将`hovered`变量设置为`true`的`mouseover`处理程序。此外，我们还有将`hovered`设置为`false`的`mouseleave`处理程序。

然后我们用`class`绑定将`foo-hover`绑定到`hovered`变量，这样它只在`hovered`为`true`时才被应用。

# 悬停时显示元素

我们还可以通过添加`v-if`来显示一个元素，当我们悬停在另一个元素上时，有条件地检查这个元素。

例如，我们可以写:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    hovered: false
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      .foo-hover {
        color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @mouseover="hovered = true" @mouseleave="hovered = false">
        Hover to Show Secret
      </button>
      <p v-if="hovered">Secret</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们将`mouseover`和`mouseleave`处理程序移到了一个按钮上。

然后我们添加了一个 p 元素，只有当`hovered`是`true`时才会显示。因此，当我们悬停在悬停显示秘密按钮，然后我们会看到秘密显示。

![](img/acc4db865694de6722aac13e3c3c5e51.png)

Photo by [Hawin Rojas](https://unsplash.com/@hawin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 悬停在 Vue 组件上

当我们将鼠标悬停在一个 Vue 组件上时，为了应用相同的效果，我们必须将`native`修改器添加到我们的`mouseover`和`mouseleave`事件处理程序中，以便将相同的效果应用到一个组件上。

例如，我们可以写:

`index.js`:

```
Vue.component("foo-text", {
  template: `<h1>foo</h1>`
});new Vue({
  el: "#app",
  data: {
    hovered: false
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      .foo-hover {
        color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <foo-text
        @mouseover.native="hovered = true"
        @mouseleave.native="hovered = false"
        :class="{'foo-hover': hovered}"
      >
      </foo-text>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们给`@mouseover`和`@mouseleave`指令添加了`native`修饰符，让我们的组件监听`mouseover`和`mouseleave`事件，就像它是一个普通的 HTML 元素一样。然后我们像之前一样根据`hovered`的值设置`foo-hover`类。

# 结论

我们可以通过监听`mouseleave`和`mouseover`事件来创建悬停效果。悬停效果在鼠标悬停时应用，在鼠标离开时移除。`mouseover`和`mouseleave`比`mouseenter`和`mouseout`好，因为`mouseover`和`mouseleave`会冒泡而不是创造新事件。

为了对我们的 Vue 组件应用相同的效果，我们必须将`native`修饰符添加到我们的`@mouseleave`和`@mouseover`指令中。

## 来自简明英语团队的一封信

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English****—谢谢，继续学习！**](https://medium.com/python-in-plain-english)

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的中级用户名和您感兴趣的内容，我们将会回复您！****