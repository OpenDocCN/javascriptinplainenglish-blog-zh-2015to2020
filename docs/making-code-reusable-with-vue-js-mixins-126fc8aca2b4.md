# 使用 Vue.js Mixins 使代码可重用

> 原文：<https://javascript.plainenglish.io/making-code-reusable-with-vue-js-mixins-126fc8aca2b4?source=collection_archive---------6----------------------->

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何定义和使用 mixins 来使代码可重用。

# 基础

我们可以通过编写一些简单的代码来定义一个 mixin，这些代码由一个具有组件的一些属性的对象组成，如下所示:

`src/index.js`:

```
const appMixin = {
  created() {
    this.hello();
  },
  methods: {
    hello() {
      this.message = "hi";
    }
  }
};new Vue({
  el: "#app",
  mixins: [appMixin],
  data: {
    message: ""
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
  </head>
  <body>
    <div id="app">
      {{message}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们通过将 mixin `appMixin`分配给一个结构与 Vue 实例相同的对象来定义它。

因此，当创建 Vue 实例时，`hello`被调用，因为我们在`appMixin`中有了`created`钩子。

然后在模板中，我们显示`message`，所以我们在屏幕上得到`hi`。

我们可以放置的其他属性包括`methods`、`components`和`directives`。它们都将被合并到我们传递给`new Vue`的对象或组件选项对象中。

如果有任何相同之处，组件的选项将优先于 mixin 中的项目。

例如，如果我们有以下内容:

`src/index.js`:

```
const appMixin = {
  created() {
    this.hello();
  },
  methods: {
    hello() {
      this.message = "hi";
    }
  }
};new Vue({
  el: "#app",
  mixins: [appMixin],
  data: {
    message: ""
  },
  created() {
    this.hello();
  },
  methods: {
    hello() {
      this.message = "foo";
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
  </head>
  <body>
    <div id="app">
      {{message}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后显示`foo`,因为 Vue 实例中的方法优先于 mixin 中的方法。

我们也可以使用`Vue.extend`方法创建一个新组件，如下所示:

`src/index.js`:

```
const appMixin = {
  created() {
    this.hello();
  },
  methods: {
    hello() {
      this.message = "hi";
    }
  }
};const Component = Vue.extend({
  mixins: [appMixin],
  el: "#app"
});new Component();
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>
  <body>
    <div id="app">
      {{message}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

`Vue.extend`的工作方式与上面的`new Vue`相同。

与`Vue.extend()`使用相同的合并机制。

![](img/f20d0ac9664171a13d63c2d46ab5024f.png)

Photo by [Paul Gilmore](https://unsplash.com/@paulgilmore_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 选项合并

我们可以通过选择合适的合并策略来调整项目的合并方式。

例如，数据对象被递归合并，在冲突的情况下，组件的数据优先。

`src/index.js`:

```
const mixin = {
  data() {
    return {
      message: "hi",
      foo: "abc"
    };
  }
};new Vue({
  el: "#app",
  mixins: [mixin],
  data() {
    return {
      message: "bye",
      bar: "def"
    };
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
  </head>
  <body>
    <div id="app">
      {{message}} {{foo}} {{bar}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们看到显示了`bye abc def`,因为`message`同时存在于组件和 mixin 中，并且组件优先。

`foo`和`bar`分别只存在于 mixin 和 component 中，所以它们都合并到同一个`data`对象中。

同名的钩子被合并到一个数组中，这样它们都将被调用。Mixin 钩子将被称为组件自己的钩子。

所以如果我们有:

`src/index.js`:

```
const mixin = {
  mounted() {
    console.log("mixin hook called");
  }
};new Vue({
  el: "#app",
  mixins: [mixin],
  mounted() {
    console.log("component hook called");
  }
});
```

我们看到:

```
mixin hook called
component hook called
```

在`console.log`输出中。

期望像`methods`、`components`和`directives`这样的对象值的选项被合并到同一个对象中。当对象中存在冲突时，组件的选项将优先考虑。

例如，如果我们有:

`src/index.js`:

```
const mixin = {
  methods: {
    foo() {
      console.log("foo");
    }
  }
};const vm = new Vue({
  el: "#app",
  mixins: [mixin],
  methods: {
    foo() {
      console.log("bar");
    }
  }
});
vm.foo();
```

我们将看到`bar`被记录，因为组件的`foo`方法优先于`mixin`的`foo`方法。

在`Vue.extend()`中使用了相同的合并策略。

# 结论

Mixins 允许我们创建可以包含在多个组件中的代码。

在这些属性中，通过使组件的`methods`、`components`和`directives`优先于 mixin 的项目来合并它们。

数据也合并在一起，如果两个地方存在相同的键，则组件的数据优先。

钩子被合并到一个数组中，如果它们有相同的名字，mixin 的钩子在组件的钩子之前被调用。

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。