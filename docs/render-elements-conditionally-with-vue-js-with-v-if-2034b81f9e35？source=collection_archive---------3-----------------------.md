# 使用 Vue.js 和 v-if 有条件地呈现元素

> 原文：<https://javascript.plainenglish.io/render-elements-conditionally-with-vue-js-with-v-if-2034b81f9e35?source=collection_archive---------3----------------------->

![](img/d5770563cb938789f931dd14e9d68f12.png)

Photo by [Emre Gencer](https://unsplash.com/@reo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将通过使用`v-if`、`v-else-if`和`v-else`指令，来看看用 Vue.js 在屏幕上有条件地呈现项目的各种方法。

# 虚拟中频

`v-if`指令用于有条件地渲染一个块。只有当指令的表达式返回一个真值时，才会呈现带有`v-if`属性的块。

例如，在`src/index.js`中，我们输入:

```
new Vue({
  el: "#app",
  data: {
    show: true
  }
});
```

然后在`index.html`中，我们放入:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-if="show">
        Hi
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后将显示`Hi`，因为`show`被设置为`true`。

如果我们想在表达式返回`false`时显示一些东西，除了`v-if`之外，我们还可以使用`v-else`。

例如，如果我们将`src/index.js`中的`true`改为`false`:

```
new Vue({
  el: "#app",
  data: {
    show: false
  }
});
```

然后在`index.html`中，如果我们有:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-if="show">Hi</div>
      <div v-else>Bye</div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们看到`Bye`,因为`show`被设置为`false`,我们将`v-else`指令添加到一个元素中，该元素是紧跟在带有`v-if`指令的元素之后的元素的兄弟元素。

# 模板元素上的 v-if 切换元素组

我们可以将`v-if`添加到一组元素中，方法是将它们包装在一个`template`元素周围。

例如，我们可以写:

```
<template v-if="show">
  <p>foo</p>
  <p>bar</p>
</template>
```

如果`show`设置为`true`，则会显示`foo`和`bar`。

# `v-else`

当紧跟在带有`v-if`的元素之后的兄弟元素中的`v-if`指令的表达式返回`false`时，我们可以使用`v-else`指令来显示一些内容。

例如，我们可以随机显示消息，如下所示。在`index.html`中，我们把:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-if="Math.random() > 0.5">
        Hi
      </div>
      <div v-else>
        Bye
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在`src/index.js`中，我们输入:

```
new Vue({
  el: "#app",
  data: {}
});
```

![](img/044116489e53606ebc62bbcc6bbbd2f7.png)

Photo by [Dane Deaner](https://unsplash.com/@danedeaner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 如果，否则

`v-if`后面还可以跟一个带有`v-else-if`指令的元素，这样我们就可以有条件地显示多个元素。

例如，我们可以把`index.html`写成如下:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-if="Math.random() > 0.8">
        A
      </div>
      <div v-else-if="Math.random() > 0.5 && Math.random() <= 0.8">
        B
      </div>
      <div v-else>
        C
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们将根据`Math.random()`返回的内容显示 A、B 和 C。

这是从 Vue 2.1.0 开始提供的。

# 用键控制可重用元素

Vue 在有条件渲染项目时，尽量重用已有的元素。

这意味着有时我们会得到意想不到的东西。

例如，如果我们在`src/index.js`中有以下内容:

```
new Vue({
  el: "#app",
  data: {
    loginType: "username"
  },
  methods: {
    toggleType() {
      this.loginType = this.loginType === "username" ? "email" : "username";
    }
  }
});
```

以及`index.html`中的以下内容:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="toggleType">Change Login Type</button>
      <template v-if="loginType === 'username'">
        <label>Username</label>
        <input placeholder="Enter your username" />
      </template>
      <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address" />
      </template>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后，当我们在输入内容后单击 Change Login Type 按钮时，我们会在更改模板后看到相同的输入值。

为了让输入从头开始呈现，我们可以向属性`key`添加一个属性`key`，为每个输入添加不同的值。

因此，我们可以写下，而不是上面的`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="toggleType">Change Login Type</button>
      <template v-if="loginType === 'username'">
        <label>Username</label>
        <input placeholder="Enter your username" key="username" />
      </template>
      <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address" key="email" />
      </template>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

那么输入将被清除。

因为我们没有添加`key`属性，所以其他的东西仍然被有效地重用。

# 结论

我们可以用`v-if`有条件地渲染事物。当我们设置为值的表达式返回`true`时，将显示添加了`v-if`的元素。

我们可以将它与`v-else-if`和`v-else`结合使用，其中我们可以提供当`v-if`中的条件返回`false`时显示的元素。

当条件返回真值且`v-if`中的元素返回假值时，显示带有`v-else-if`的元素。它必须直接添加到带有`v-if`的元素下面。

如果没有带`v-else-if`的元素，带`v-else`的元素可以添加到带`v-if`的元素下面。如果有一个或多个带有`v-else-if`的元素，那么`v-else`元素必须添加到它的下面。

`v-else`可用于显示所有其他条件为假的情况。