# 如何在 Vue 中呈现数据数组

> 原文：<https://javascript.plainenglish.io/list-rendering-with-vue-js-v-for-directive-91a0a7756a68?source=collection_archive---------0----------------------->

## 使用 vue . js-v-for 指令进行列表呈现

![](img/da64b1c59153186e00b240029d8ffe28.png)

Photo by [Félix Lam](https://unsplash.com/@feliixlam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何使用`v-for`指令来呈现列表。

# 使用 v-for 将数组映射到元素

我们可以使用`v-for`指令来呈现数组中的条目列表。

例如，我们可以如下使用它:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: [{ name: "Joe" }, { name: "Jane" }, { name: "Mary" }]
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
      <div v-for="person in persons">
        {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有了`v-for`指令，然后我们编写`person in persons`来遍历`data.persons`数组。

然后我们从每个条目中获取`name`属性并显示它。

`person`是被迭代的数组元素的别名。

然后我们得到:

```
JoeJaneMary
```

数组条目的所有属性在迭代期间都是可用的。

我们还可以添加可选的第二个参数来访问数组的索引。例如，我们可以这样写:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: [{ name: "Joe" }, { name: "Jane" }, { name: "Mary" }]
  }
});
```

`index.html`；

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-for="(person, index) in persons">
        {{index}} - {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
0 - Joe1 - Jane2 - Mary
```

我们也可以用`of`代替`in`来匹配 JavaScript 中的`for...of`循环语法:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-for="person of persons">
        {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

![](img/b8cf6539a64df3fcf65d52f82e42260e.png)

Photo by [Jens Kreuter](https://unsplash.com/@jenskreuter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# v-用一个物体表示

我们也可以使用`v-for`来遍历一个对象的值。

所以如果我们写下:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    book: {
      title: "JavaScript Book",
      author: "John Smith",
      publishedYear: 2019
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
      <div v-for="value in book">
        {{value}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们得到了:

```
JavaScript BookJohn Smith2019
```

我们可以通过向表达式提供第二个参数来遍历对象的键，如下所示:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    book: {
      title: "JavaScript Book",
      author: "John Smith",
      publishedYear: 2019
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
      <div v-for="(value, key) in book">
        {{key}} - {{value}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
title - JavaScript Bookauthor - John SmithpublishedYear - 2019
```

此外，我们可以为对象条目的`index`提供第三个参数。

为此，我们可以写:

`src/index.js:`

```
new Vue({
  el: "#app",
  data: {
    book: {
      title: "JavaScript Book",
      author: "John Smith",
      publishedYear: 2019
    }
  }
});
```

`index.html`:

```
`<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head> <body>
    <div id="app">
      <div v-for="(value, key, index) in book">
        {{index}}: {{key}} - {{value}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
0: title - JavaScript Book1: author - John Smith2: publishedYear - 2019
```

对象的迭代顺序基于`Object.keys()`的枚举顺序，这并不保证在 JavaScript 引擎实现中是一致的。

# 保持状态

`v-for`默认情况下，就地更新项目，而不是移动 DOM 元素来匹配项目的顺序，以确保它反映了应该在特定索引处呈现的内容。

因此，只有当列表不依赖于子组件状态或临时 DOM 状态(如输入值)时，它才是合适的。

为了确保 Vue 知道哪些元素是唯一的，我们应该通过`v-bind:key`指令提供一个`key`属性。

例如，我们可以写:

`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    persons: [{ name: "Joe" }, { name: "Jane" }, { name: "Mary" }]
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
      <div v-for="person in persons" v-bind:key="person.name">
        {{person.name}}
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们必须使用原始值作为键。

`key`属性是 Vue 识别节点的通用机制。

# 结论

我们可以使用`v-for`来遍历一个数组的条目或一个对象的条目，并呈现这些条目。

要使用它来呈现数组，我们可以使用`in`或`of`。同样，我们可以用`v-for`访问数组条目的索引。

要遍历对象，我们可以做同样的事情，只是第一个参数是属性值，第二个参数是键，第三个参数是索引。

每种情况下都只需要第一个参数。

可以设置`key`属性，这样 Vue 就可以识别唯一的节点。我们可以用`v-bind:key`将其设置为原始值。