# Vue 3 —渲染功能基础

> 原文：<https://javascript.plainenglish.io/vue-3-render-functions-basics-a55b44a339f2?source=collection_archive---------3----------------------->

![](img/04c7e8c827a3ec137302854b7da56cbf.png)

Photo by [Marsumilae](https://unsplash.com/@marsumilae?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何用 Vue 3 创建渲染函数。

# 渲染函数

大多数时候应该使用模板来构建 Vue 组件。

然而，在有些情况下，我们可能需要更多的灵活性。

例如，我们可能想要添加带有动态标签的元素。

而不是写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <variable-heading :level="1">foo</variable-heading>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("variable-heading", {
        template: `
          <h1 v-if="level === 1">
            <slot></slot>
          </h1>
          <h2 v-else-if="level === 2">
            <slot></slot>
          </h2>
          <h3 v-else-if="level === 3">
            <slot></slot>
          </h3>
          <h4 v-else-if="level === 4">
            <slot></slot>
          </h4>
          <h5 v-else-if="level === 5">
            <slot></slot>
          </h5>
          <h6 v-else-if="level === 6">
            <slot></slot>
          </h6>
        `,
        props: {
          level: {
            type: Number,
            required: true
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们可以用渲染函数使标题标签动态化。

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
      <variable-heading :level="1">foo</variable-heading>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("variable-heading", {
        render() {
          const { h } = Vue; return h(`h${this.level}`, {}, this.$slots.default());
        },
        props: {
          level: {
            type: Number,
            required: true
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们用一个`render`方法创建了`variable-heading`组件来呈现我们的 HTML。

`h`方法让我们渲染我们的项目。

第一个参数是标记名。

第二个是我们想要传递给它的任何道具或属性。

第三个参数是一个子数组。

`this.$slots.default()`是让我们填充内容的默认槽，

组件的其余部分与其他部分相同。

DOM 中的每个元素都是一个节点，`h`方法让我们像在树中一样通过嵌套来呈现节点。

# 虚拟 DOM 树

Vue 保留了一个虚拟 DOM 树来跟踪需要对真实 DOM 进行的更改。

`h`方法返回一个对象，该对象包含 Vue 更新读取的 DOM 所需的信息。

# `h()`论据

`h`方法有 3 个参数。

标记名是第一个参数。它可以是字符串、对象、函数或`null`。

如果它是一个对象，那么它必须是一个组件或异步组件。

第二个参数是一个带有可选属性的对象。

第三个参数是子节点的字符串、数组或对象。

也是可选的。

# 虚拟节点必须是唯一的

在我们的渲染函数中不能有重复的 VNodes。

所以我们不能有:

```
render() {
  const myParagraphVNode = Vue.h('p', 'hi')
  return Vue.h('div', [
    paragraph, paragraph
  ])
}
```

我们不能在数组中有 2 个`paragraph`。

![](img/6bc144914c5fe0ccb29e452e6e2d01ae.png)

Photo by [Uriel Soberanes](https://unsplash.com/@soberanes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用渲染函数告诉 Vue 如何用 JavaScript 渲染组件。

这是模板的替代方案。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**