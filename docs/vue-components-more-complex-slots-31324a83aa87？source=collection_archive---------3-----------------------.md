# 如何在 Vue 中创建更复杂的插槽

> 原文：<https://javascript.plainenglish.io/vue-components-more-complex-slots-31324a83aa87?source=collection_archive---------3----------------------->

![](img/ea9533d902148d45343bbd665849f37f.png)

Photo by [Matt Heaton](https://unsplash.com/@mattisrad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究命名和限定作用域的槽。

# 命名插槽

有时我们希望有多个插槽。为了区分它们，我们必须给它们命名。

我们可以用带有`name`属性的命名插槽定义一个组件，如下所示:

```
Vue.component("layout", {
  template: `
  <div>
    <header>    
      <slot name="header"></slot>
    </header>
    <main>      
      <slot></slot>
    </main>
    <footer>      
      <slot name="footer"></slot>
    </footer>
  </div>
  `
});
```

然后，我们可以按如下方式一起使用上面的组件:

`src/index.js`:

```
Vue.component("layout", {
  template: `
  <div>
    <header>    
      <slot name="header"></slot>
    </header>
    <main>      
      <slot></slot>
    </main>
    <footer>      
      <slot name="footer"></slot>
    </footer>
  </div>
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
      <layout>
        <template v-slot:header>
          <h1>Header</h1>
        </template> <p>Main</p> <template v-slot:footer>
          <p>Footer</p>
        </template>
      </layout>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

名为`header`的插槽填充有:

```
<template v-slot:header>
   <h1>Header</h1>
</template>
```

名为`footer`的插槽被填充:

```
<template v-slot:footer>
   <p>Footer</p>
</template>
```

没有名称的插槽填充有:

```
<p>Main</p>
```

如果我们想用`v-slot:default`为没有名称的槽包装内容，如下所示:

```
<template v-slot:default>
  <p>Main</p>
</template>
```

无论哪种方式，呈现的 HTML 都是一样的。

# 作用域插槽

我们可以使用作用域插槽来访问子组件中的数据。

为了使来自子组件的数据在父组件中可用，我们可以使用`v-bind`指令。

从父组件获取子组件数据的简单示例如下:

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
        <template v-slot:default="slotProps">
          {{ slotProps.user.firstName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有:

```
<p>
   <slot v-bind:user="user"></slot>
</p>
```

使`user`在根 Vue 实例中可用。

然后在根模板中，我们有:

```
<user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</user>
```

通过`slotProps` `slotProps`访问`user`的数据可以访问子组件中通过`v-bind`可用的所有数据。

![](img/948ad7c5ef02e774661ca528ca9f569a.png)

Photo by [Austin Ban](https://unsplash.com/@austinban?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 单独默认插槽的缩写语法

如果只有提供内容的默认槽，我们可以将组件上的`v-slot:default`或`v-slot`缩短如下:

```
<user v-slot:default="slotProps">
   {{ slotProps.user.firstName }}
</user>
```

或者:

```
<user v-slot="slotProps">
   {{ slotProps.user.firstName }}
</user>
```

如果有其他命名的插槽，那么上面的语法就不能使用，因为可能会有歧义。

如果我们有多个插槽，那么我们必须编写如下代码:

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
        <template v-slot:first-name="slotProps">
          {{ slotProps.user.firstName }}
        </template>
        <template v-slot:last-name="slotProps">
          {{ slotProps.user.lastName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们必须显式命名插槽，然后我们可以分别通过`v-slot:first-name=”slotProps”`和`v-slot:last-name=”slotProps”`访问子组件的数据。

此外，我们将槽内容包装在`template`中。

## 破坏插槽道具

我们可以用析构赋值操作符来析构插槽道具的属性。

例如，我们可以如下使用它:

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
        <template v-slot:first-name="{ user }">
          {{ user.firstName }}
        </template>
        <template v-slot:last-name="{ user }">
          {{ user.lastName }}
        </template>
      </user>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们没有写`slotProps`，而是改成了`{ user }`。`{ user }`与`slotProps.user`相同。

# 结论

我们可以使用命名的和限定范围的插槽来创建多个插槽，并分别从父组件的子组件中访问数据。

命名槽防止了歧义，并允许我们使用多个槽。

同样，我们可以在子组件中使用`v-bind`，然后在组件中使用`slotProps`从父组件中访问子组件的数据。

## **用简单的英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，请用你的 Medium 用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。