# Vue 组件—动态插槽名称和快捷键

> 原文：<https://javascript.plainenglish.io/vue-components-dynamic-slot-names-and-shorthands-efbace3990f1?source=collection_archive---------3----------------------->

![](img/3cf7a9185c58929e8b0b89da8bcb4e18.png)

Photo by [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究添加动态插槽名称。

# 动态插槽名称

从 Vue.js 2.6.0 开始，我们可以用`v-slot`传入动态槽名。例如，我们可以如下使用它:

`src/index.js`:

```
Vue.component("user", {
  data() {
    return {
      user: {
        firstName: "Joe",
        lastName: "Smith"
      }
    };
  },
  template: `<p>
    <slot v-bind:user="user" name='first-name'></slot>
    <slot v-bind:user="user" name='last-name'></slot>
  </p>`
});new Vue({
  el: "#app",
  data: {
    firstname: "first-name",
    lastname: "last-name"
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
      <user>
        <template v-slot:[firstname]="{ user }">
          {{ user.firstName }}
        </template>
        <template v-slot:[lastname]="{ user }">
          {{ user.lastName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，`data`中的`firstname`和`lastname`被设置为我们的根模板中的槽名。

# 命名插槽速记

`v-slot`有简写。我们可以把`v-slot:`缩写成符号`#`。例如，我们可以将示例重写如下:

`src/index.js`:

```
Vue.component("user", {
  data() {
    return {
      user: {
        firstName: "Joe",
        lastName: "Smith"
      }
    };
  },
  template: `<p>
    <slot v-bind:user="user" name='first-name'></slot>
    <slot v-bind:user="user" name='last-name'></slot>
  </p>`
});new Vue({
  el: "#app"
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
      <user>
        <template #first-name="{ user }">
          {{ user.firstName }}
        </template>
        <template #last-name="{ user }">
          {{ user.lastName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

为了引用默认插槽，我们必须编写`#default`。例如，我们写道:

`src/index.js`:

```
Vue.component("user", {
  data() {
    return {
      user: {
        firstName: "Joe",
        lastName: "Smith"
      }
    };
  },
  template: `<p>
    <slot v-bind:user="user"></slot>    
  </p>`
});new Vue({
  el: "#app"
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
      <user>
        <template #default="{ user }">
          {{ user.firstName }} {{ user.lastName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

![](img/61f5e9cca14e30dec1870e3bf25c8de0.png)

Photo by [David Marcu](https://unsplash.com/@davidmarcu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 与 v-for 一起使用

我们可以向列表中提供的后备内容添加一个槽，并允许我们更改父列表中的内容。例如，我们可以这样写:

`src/index.js`:

```
Vue.component("people", {
  data() {
    return {
      persons: [
        { name: "Joe", isAdult: true },
        { name: "Jane", isAdult: false }
      ]
    };
  },
  template: `
    <ul>  
      <li v-for='person of persons'>
        <slot v-bind:person="person"></slot>    
      </li>
    </ul>
  `
});new Vue({
  el: "#app"
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
      <people v-bind:persons="persons">
        <template #default="{ person }">
          {{person.name}}
          <span v-if="person.isAdult"> - Adult</span>
          <span v-else> - Child</span>
        </template>
      </people>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有带有`persons`数据的`people`组件。我们在`people`组件中使用`v-for`渲染`persons`，并通过`v-bind`使条目可用:

```
<slot v-bind:person="person"></slot>
```

然后在根模板中，我们有:

```
<people v-bind:persons="persons">
  <template #default="{ person }">
    {{person.name}}
    <span v-if="person.isAdult"> - Adult</span>
    <span v-else> - Child</span>
  </template>
</people>
```

从`people`中获取`person`，然后提供一个`template`来显示我们想要的条目，而无需修改`people`组件。

# 结论

从 Vue 2.6.0 开始，我们可以使用动态插槽名称，而不是硬编码的名称。

此外，命名槽的简写是`#`。对于默认插槽，我们总是要写`#default`。

最后，我们可以将它与`v-for`一起使用，让我们定义一个可以与每个条目一起使用的`template`。我们在数组的条目上使用`v-bind`，而不是整个数组，然后我们可以访问父组件中的数据，并添加一个`templarte`，通过在父组件中定义它来以我们喜欢的方式显示数据。