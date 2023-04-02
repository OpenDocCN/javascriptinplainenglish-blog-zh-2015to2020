# Vue 3 —插槽人手不足

> 原文：<https://javascript.plainenglish.io/vue-3-slot-shorthands-b673883b1ab7?source=collection_archive---------8----------------------->

![](img/ebdec861643d3285304720edbd2c8967.png)

Photo by [Carl Raw](https://unsplash.com/@carltraw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看 Vue 3 的吃角子老虎机。

# 默认插槽的速记语法

如果我们有一个槽，那么我们不需要`template`元素。

我们可以将`v-slot`指令放在开始组件标签上。

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
      <items v-slot="slotProps">
        <b>{{ slotProps.item.name }}</b>
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

我们在组件标签上有`v-slot`指令，只有在我们的组件中有一个插槽时才允许。

# 破坏插槽道具

吃角子老虎道具可以被破坏。

因为我们有一个对象作为`v-slot`的值，我们可以析构它的属性。

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
      <items v-slot="{ item: { name } }">
        <b>{{ name }}</b>
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

我们析构了`slotProps`对象的属性，所以不用写:

```
<items v-slot="slotProps">
  <b>{{ slotProps.item.name }}</b>
</items>
```

我们有:

```
<items v-slot="{ item: { name } }">
  <b>{{ name }}</b>
</items>
```

这可以使模板更干净，尤其是如果我们有很多插槽。

我们也很容易提供后援。

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
      <items v-slot="{ item: { name = 'placeholder' } }">
        <b>{{ name }}</b>
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
            items: [{ name: "apple" }, { name: "orange" }, {}]
          };
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们将`name`属性的默认值设置为`'placeholder'`，这样如果没有为`name`属性设置任何内容，我们就可以显示出来。

# 动态插槽名称

插槽名称可以是动态的。

所以我们可以写:

```
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

其中`dynamicSlotName`是带有槽名的变量。

# 命名插槽速记

命名的插槽可以用`#`符号短接。

例如，不要写:

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
        </template>
        <template v-slot:default>
          <p>lorem ipsum.</p>
        </template>
        <template v-slot:footer>
          <p>footer</p>
        </template>
      </blog-post>
    </div>
    <script>
      const app = Vue.createApp({});
      app.component("blog-post", {
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
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们可以写:

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
        <template #header>
          <h1>header</h1>
        </template>
        <template #default>
          <p>lorem ipsum.</p>
        </template>
        <template #footer>
          <p>footer</p>
        </template>
      </blog-post>
    </div>
    <script>
      const app = Vue.createApp({});
      app.component("blog-post", {
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
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

`#`是`v-slot:`的简称。

![](img/b5672f08e60ea276af607f88c444fa88.png)

Photo by [Heather Gill](https://unsplash.com/@heathergill?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

命名插槽和默认插槽都有简写。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**