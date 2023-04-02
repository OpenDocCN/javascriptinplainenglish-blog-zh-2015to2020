# Vue 3 —动态和异步组件

> 原文：<https://javascript.plainenglish.io/vue-3-dynamic-and-async-components-9d6389b012a?source=collection_archive---------4----------------------->

![](img/9bf569b3ea2cbc61a92b40517f3df5f8.png)

Photo by [Yogesh Mankame](https://unsplash.com/@yogi198?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何用 Vue 3 添加动态和异步组件。

# 带`keep-alive`的动态组件

`keep-alive`组件让我们保持组件的状态，即使我们退出了它们。

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
      <button @click='currentComponent = "tab-home"'>home</button>
      <button @click='currentComponent = "tab-post"'>post</button>
      <button @click='currentComponent = "tab-archive"'>archive</button>
      <keep-alive>
        <component :is="currentComponent"></component>
      </keep-alive>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            currentComponent: "tab-home"
          };
        }
      }); app.component("tab-home", {
        template: `
          <div>
            home
          </div>
        `
      }); app.component("tab-post", {
        data() {
          return {
            on: false
          };
        },
        template: `
          <div>
            <button @click='on = !on'>toggle</button>
            <p v-if='on'>post</p>
          </div>
        `
      }); app.component("tab-archive", {
        data() {
          return {
            on: false
          };
        },
        template: `
          <div>
            <button @click='on = !on'>toggle</button>
            <p v-if='on'>archive</p>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`keep-alive`组件来缓存动态组件的状态，这样当我们离开它后再次显示它时，我们保持和以前一样的状态。

因此，如果`on`状态是`tab-post`或`tab-archive`中的`true`，那么`on`将保持`true`，即使我们在任何组件之间来回切换。

没有它，`on`状态将被重置为初始状态。

# 异步组件

在较大的应用程序中，我们可能需要将应用程序分成更小的块，只在需要的时候从服务器加载组件。

我们可以用`Vue.defineAsyncComponent`方法来实现。

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
      <async-comp></async-comp>
    </div>
    <script>
      const app = Vue.createApp({}); const AsyncComp = Vue.defineAsyncComponent(
        () =>
          new Promise((resolve, reject) => {
            resolve({
              template: "<div>async component</div>"
            });
          })
      ); app.component("async-comp", AsyncComp); app.mount("#app");
    </script>
  </body>
</html>
```

定义异步组件并使用它。

我们用一个回调函数调用了`Vue.defineAsyncComponent`，该回调函数返回一个解析为组件对象的承诺。

我们可以将返回给`app.component`方法的异步组件以名称作为第一个参数，异步组件对象作为第二个参数。

然后我们可以在父 Vue 实例的模板中使用它。

我们也可以从其他地方导入组件。

例如，我们可以写:

```
import { defineAsyncComponent } from 'vue'const AsyncComp = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)app.component('async-component', AsyncComp)
```

`import`返回一个解析到 Vue 组件的承诺，因此我们获得了相同的结果。

我们还可以通过将我们的异步组件放在`components`属性中来本地注册组件:

```
import { createApp, defineAsyncComponent } from 'vue'createApp({
  // ...
  components: {
    AsyncComponent: defineAsyncComponent(() =>
      import('./components/AsyncComponent.vue')
    )
  }
})
```

# 带悬念地使用

默认情况下，异步组件是可挂起的。

这意味着我们可以将它传递给`Suspenbse`组件并使用它。

要禁用暂停，我们可以在异步组件中将`suspensible`设置为`false`。

![](img/41ddba4da16abc87a28b83b5d5db468a.png)

Photo by [Shyam](https://unsplash.com/@thezenoeffect?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以定义异步组件来异步加载它们。

`keep-alive`组件让我们在它们之间切换时保持组件的状态。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)