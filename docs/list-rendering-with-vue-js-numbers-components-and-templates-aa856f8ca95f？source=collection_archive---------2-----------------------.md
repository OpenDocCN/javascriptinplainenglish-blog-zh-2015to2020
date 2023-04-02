# 使用 Vue.js 呈现列表—数字、组件和模板

> 原文：<https://javascript.plainenglish.io/list-rendering-with-vue-js-numbers-components-and-templates-aa856f8ca95f?source=collection_archive---------2----------------------->

![](img/03f5ee1348a48d3ec10c0310d871685d.png)

Photo by [Cristina Gottardi](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在这篇文章中，我们将看看渲染一系列的数字，渲染组件和模板，等等。

# v-表示有范围

`v-for`可以取一个整数来呈现一个数字范围。例如，如果我们有以下内容:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {}
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
  </head><body>
    <div id="app">
      <span v-for="num in 10">
        {{num}}
      </span>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们看到:

```
1 2 3 4 5 6 7 8 9 10
```

# 模板上的 v-for

我们可以使用`v-for`来遍历`template`元素。例如，我们可以写:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {}
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
      <template v-for="num in 10">
        <span>
          {{num}}
        </span>
        <span>|</span>
      </template>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
```

显示在屏幕上。

# v-for 与 v-if

如果一起使用,`v-for`比`v-if`优先级高。

然而，我们不应该一起使用它们，因为`v-for`的优先级高于`v-if`。

如果我们想在呈现数组中的项目时过滤掉一些元素，我们应该使用计算属性。

如果我们只呈现一小部分数组元素，它必须遍历整个列表，然后检查我们设置的表达式是否正确。

例如，下面的 JavaScript 和 HTML 代码:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    numbers: [1, 2, 3, 4, 5]
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
      <span v-for="num in numbers" v-if="num !== 3">
        {{num}}
      </span>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

不是很有效，因为每次渲染循环时，Vue 必须遍历每个元素，然后检查`num !== 3`是否为`true`。

相反，我们应该使用如下计算属性:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    numbers: [1, 2, 3, 4, 5]
  },
  computed: {
    not3() {
      return this.numbers.filter(p => p !== 3);
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
      <span v-for="num in not3">
        {{num}}
      </span>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码效率更高，因为只有在`numbers`改变时才会重新计算`not3`。

此外，在渲染过程中只迭代`not3`中的项目，因此`v-for`不必遍历所有项目。

模板中的逻辑也更少，这使它保持了整洁。维护就容易多了。

我们还可以通过将`v-if`移动到拥有`v-for`的父元素来获得性能优势，因此只有当条件满足时，元素内部的内容才会被呈现。

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
  </head>    <body>
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
  </head>    <body>
    <div id="app">
      <div v-for="person in persons" v-if="show">
        {{person}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，Vue 必须遍历每个条目，检查`v-if`中的条件是否返回真值。

正如我们所见，尽管代码只是略有不同，但性能影响是很大的。

![](img/ac9c58c86a2575829522a88ff5235a86.png)

Photo by [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# v-与组件一起形成

我们可以在定制组件上使用`v-for`,就像使用任何其他元素一样。

例如，我们可以这样写:

`src/index.js`:

```
Vue.component("num", {
  props: ["num"],
  template: "<span>{{num}}</span>"
});new Vue({
  el: "#app",
  data: {
    numbers: [1, 2, 3, 4, 5]
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
      <num v-for="num in numbers" :num="num" :key="num" />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
12345
```

显示在屏幕上。

注意，我们提供了`:key`属性。从 Vue 2.2.0 开始，这是必需的。

此外，我们通过编写以下代码将`num`传递到`num`组件的`num`属性中:

```
:num="num"
```

这让我们可以向组件注入我们想要的东西，减少它们之间的耦合。

# 结论

我们可以通过给`v-for`提供一个数字而不是一个数组或对象来呈现一系列数字。

`v-for`和`v-if`不应该一起使用，这样`v-if`就不必在每次迭代中运行，我们也可以减少迭代的项数。

此外，我们可以像其他元素一样使用`v-for`遍历模板和组件。