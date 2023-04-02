# 如何在 Vue 中有条件地渲染元素

> 原文：<https://javascript.plainenglish.io/render-elements-conditionally-with-vue-js-v-show-and-performance-f6a69a338412?source=collection_archive---------4----------------------->

## 虚拟展示和表演

![](img/ee15d5b9191f1c0f42bb5ebfd614266e.png)

Photo by [Cristina Gottardi](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将通过使用`v-show`指令来查看使用 Vue.js 在屏幕上有条件地呈现项目的各种方法。

此外，我们将通过各种方式来看看组合`v-if`和`v-for`的性能含义。

# 虚拟展示

当我们传递给它的表达式是真的时，指令用来显示一些东西。

例如，我们可以如下使用它:

```
<h1 v-show="show">Hi</h1>
```

`v-show`和`v-if`的区别在于`v-show`元素保留在 DOM 中，即使被隐藏了。

另一方面，`v-if`元素通过从 DOM 中移除来隐藏。

此外，`v-if`删除事件监听器，并且当`v-if`的表达式为真时，如果条件块中的子组件被隐藏并重新创建，则它们会被销毁。

`v-if`元素的不同之处还在于，如果在条件渲染期间条件为 falsy，它将不会做任何事情。直到表达式变得真实，它才会被渲染。

`v-if`比`v-show`具有更高的切换性，因为在切换时必须进行 DOM 操作和附加事件监听器。

所以，如果事情需要经常切换，`v-show`效率更高。

# v-if 带 v-for

如果一起使用，`v-for`比`v-if`优先级高。

然而，我们不应该一起使用它们，因为`v-for`的优先级高于`v-if`。

如果我们想在呈现数组中的项目时过滤掉一些元素，我们应该使用计算属性。

如果我们只呈现一小部分数组元素，它必须遍历整个列表，然后检查我们设置的表达式是否正确。

例如，下面的 JavaScript 和 HTML 代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Jane", "Mary"]
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
  </head>  <body>
    <div id="app">
      <div v-for="person in persons" v-if='person !== "Joe"'>
        {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

不是很有效，因为每次循环被渲染时，Vue 必须遍历每个元素，然后检查`person !== “Joe”`是否是`true`。

相反，我们应该使用如下计算属性:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Jane", "Mary"]
  },
  computed: {
    personsWithoutJoe() {
      return this.persons.filter(p => p !== "Joe");
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
  </head>  <body>
    <div id="app">
      <div v-for="person in personsWithoutJoe">
        {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码效率更高，因为只有在`persons`改变时才会重新计算`personsWithoutJoe`。

此外，在渲染过程中只迭代`personsWithoutJoe`中的项目，所以`v-for`不必遍历所有项目。

模板中的逻辑也更少，这使它保持了整洁。维护就容易多了。

我们还可以通过将`v-if`移动到具有`v-for`的父元素来获得性能优势，因此只有在满足条件时才会呈现元素内部的内容。

例如，如果我们有:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Jane", "Mary"],
    show: false
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
  </head>  <body>
    <div id="app">
      <div v-if="show">
        <div v-for="person in persons">
          {{person}}
        </div>
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

由于`show`是`false`，所以不渲染任何内容。Vue 不需要工作就能看到里面的东西。

这比:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>  <body>
    <div id="app">
      <div v-for="person in persons" v-if="show">
        {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，Vue 必须遍历每个条目，并检查`v-if`中的条件是否返回真值。

正如我们所见，尽管代码只是略有不同，但性能影响是很大的。

![](img/e933e240b0892440e4f66dd404104cfd.png)

Photo by [Jérémie Crémer](https://unsplash.com/@jcremer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`v-show`与`v-if`相似，除了带有`v-show`的项目无论是否显示都会保留在 DOM 中。

还有，组合`v-for`和`v-if`的时候要小心。

首先，如果应用于相同的元素，`v-for`优先于`v-if`。

此外，我们不应该在同一个元素上使用它们，因为 Vue 必须在每次迭代中检查每个元素的`v-if`条件。

如果我们想要呈现一个过滤列表，我们应该使用一个计算属性。

如果我们想检查是否应该渲染整个数组，我们应该将`v-if`移动到元素`v-for`的父元素，这样如果`v-if`的条件是 falsy，那么 Vue 就不会渲染那里的项目。