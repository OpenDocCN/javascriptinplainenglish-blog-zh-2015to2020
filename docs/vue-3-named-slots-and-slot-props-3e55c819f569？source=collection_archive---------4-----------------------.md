# Vue 3 —命名插槽和插槽道具

> 原文：<https://javascript.plainenglish.io/vue-3-named-slots-and-slot-props-3e55c819f569?source=collection_archive---------4----------------------->

![](img/52690d2fb5084559e6e67af390ec1060.png)

Photo by [Mohammed Kabir](https://unsplash.com/@evoknow?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何使用插槽来填充 Vue 3 的内容。

# 命名插槽

我们可以命名我们的插槽，这样我们可以在一个组件中有多个插槽。

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
      <blog-post>
        <template v-slot:header>
          <h1>header</h1>
        </template> <template v-slot:default>
          <p>lorem ipsum.</p>
        </template> <template v-slot:footer>
          <p>footer</p>
        </template>
      </blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        template: `
          <header>
            <slot name="header"></slot>
          </header>
          <main>
            <slot></slot>
          </main>
          <footer>
            <slot name="footer"></slot>
          </footer>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

`blog-post`组件有 header、main 和 footer 元素。

在每个里面，他们有自己的插槽。

我们用名字`header`来命名顶部`slot`。

我们将底部的`slot`命名为`footer`。

主标签有没有名字的`slot`。

然后我们可以用`template`元素填充它们。

`v-slot`指令让我们通过传入插槽名称作为参数来填充插槽。

如果一个槽没有名字，那么我们可以用`default`来指代它。

因此，我们将 h1 显示在顶部。

“lorem ipsum”显示在中间。

“页脚”显示在底部。

# 作用域插槽

作用域插槽让我们可以将子元素的状态提供给父元素。

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
      <items>
        <template v-slot:default="slotProps">
          <b>{{ slotProps.item.name }}</b>
        </template>
      </items>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("items", {
        template: `
          <ul>
            <li v-for="( item, index ) in items">
              <slot v-bind:item="item"></slot>
            </li>
          </ul>
        `,
        data() {
          return {
            items: [{ name: "apple" }, { name: "orange" }, { name: "grape" }]
          };
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有了想要呈现的带有`items`的`items`组件。

我们没有直接呈现它们，而是用`v-bind`指令将它们传递到`slot`中，这样我们就可以改变它们在父节点中的呈现方式。

`v-bind`的参数将是父模板中`slotProps`变量的属性名。

该值将是`slotProps.item`属性的属性值。

因此，我们可以在`items`标签之间添加`template`标签。

`template`标签包含带有`slotProps`值的`v-slot:default`指令。

我们可以用它来定制从`items`中的`v-bind`传入的属性的呈现。

如果只有一个插槽，那么我们可以简化访问唯一缺省插槽的语法。

例如，我们可以写:

```
v-slot="slotProps"
```

而不是:

```
v-slot:default="slotProps"
```

所以我们可以:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <items>
        <template v-slot="slotProps">
          <b>{{ slotProps.item.name }}</b>
        </template>
      </items>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("items", {
        template: `
          <ul>
            <li v-for="( item, index ) in items">
              <slot v-bind:item="item"></slot>
            </li>
          </ul>
        `,
        data() {
          return {
            items: [{ name: "apple" }, { name: "orange" }, { name: "grape" }]
          };
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

稍微短一点。

![](img/a6c1af2d8ae66ff339f49394616024f9.png)

Photo by [DEAR](https://unsplash.com/@riverse?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将命名的插槽添加到组件中，并使用插槽属性从父组件的子组件中访问数据。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**