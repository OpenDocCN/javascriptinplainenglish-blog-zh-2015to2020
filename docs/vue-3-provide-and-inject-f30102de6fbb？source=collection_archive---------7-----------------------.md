# Vue 3 —提供和注入

> 原文：<https://javascript.plainenglish.io/vue-3-provide-and-inject-f30102de6fbb?source=collection_archive---------7----------------------->

![](img/1273a0741b41a79155842b5fd7a3c6ce.png)

Photo by [Elaine Casap](https://unsplash.com/@ecasap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究用 Vue 3 提供和注入数据。

# 提供和注射

为了使在深度嵌套的组件之间传递数据更容易，我们可以绕过 props，使用`provide`属性来公开一些数据。

然后我们可以使用`inject`属性来获取数据。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post></blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        data() {
          return {
            post: { title: "hello world" }
          };
        },
        provide: {
          author: "james"
        },
        template: `
          <div>
            {{ post.title }}
            <blog-post-statistics></blog-post-statistics>
          </div>
        `
      }); app.component("blog-post-statistics", {
        inject: ["author"],
        template: `
          <p>{{author}}</p>
        `,
        created() {
          console.log(this.author);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`blog-post`和`blog-post-statistics`组件。

在`blog-post`组件中，我们有带有`author`属性的`provide`属性。

这是我们提供给子组件的数据。

因此，在`blog-post-statistics`组件中，我们有一个`inject`属性，其中包含了我们想要获取的`provide`中的属性名数组。

所以`blog-post-statistics`中的`author`在模板中是`author`或者在组件方法中是`this.author`。

但是，这不适用于 Vue 实例属性。

所以如果我们有:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post></blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        data() {
          return {
            post: { title: "hello world", author: "james" }
          };
        },
        provide: {
          post: this.post
        },
        template: `
          <div>
            {{ post.title }}
            <blog-post-statistics></blog-post-statistics>
          </div>
        `
      }); app.component("blog-post-statistics", {
        inject: ["post"],
        template: `
          <p>{{post.author}}</p>
        `,
        created() {
          console.log(this.post.author);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

则不会显示任何内容。

要解决这个问题，我们必须将值传递给`provide`的属性，并将`provide`更改为一个函数:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post></blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        data() {
          return {
            post: { title: "hello world", author: "james" }
          };
        },
        provide() {
          return {
            author: this.post.author
          };
        },
        template: `
          <div>
            {{ post.title }}
            <blog-post-statistics></blog-post-statistics>
          </div>
        `
      }); app.component("blog-post-statistics", {
        inject: ["author"],
        template: `
          <p>{{author}}</p>
        `,
        created() {
          console.log(this.author);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

现在`provide`是`blog-post`组件的方法，而不是属性。

我们把我们想提供的东西返回给子组件。

# 使用反应性

当我们更新子组件时，父组件中具有反应属性的更改不会反映在子组件中。

这个问题可以通过将反应属性设置为计算属性来解决。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <counter></counter>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("counter", {
        data() {
          return {
            count: 0
          };
        },
        provide() {
          return { count: Vue.computed(() => this.count) };
        },
        template: `
          <div>
            <button @click='count++'>increment</button>
            <count-display></count-display>
          </div>
        `
      }); app.component("count-display", {
        inject: ["count"],
        template: `
          <p>{{count.value}}</p>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有一个带有`provide`方法的`counter`组件，它返回一个带有`count`属性的对象。

它的值是一个计算属性，回调函数中返回了`this.count`。

这样，最新的值将被传播到子组件。

然后在`count-display`组件中，我们可以用`value`属性获取值。

![](img/f29bfbace167772f98142fca1b92e7ea.png)

Photo by [Kate Remmer](https://unsplash.com/@studioktr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Provide 和 inject gives 是一种不使用 props 将数据传递给深层嵌套组件的方法。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)