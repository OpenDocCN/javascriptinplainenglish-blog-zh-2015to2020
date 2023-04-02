# Vue 3 —插槽

> 原文：<https://javascript.plainenglish.io/vue-3-slots-4f360eb98d6d?source=collection_archive---------7----------------------->

![](img/b992b50fd4349fcd717ecf469174a70b.png)

Photo by [Benoit Dare](https://unsplash.com/@_themoi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何使用插槽来填充 Vue 3 的内容。

# 时间

我们可以添加插槽，让我们将内容分发到我们想要的位置。

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
      <click-button>click me</click-button>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("click-button", {
        template: `
          <button>
            <slot></slot>
          </button>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了内部带有`slot`的`click-button`组件，这样我们可以在标签之间添加我们自己的内容。

它会将“点击我”作为内容文本呈现到按钮上。

我们也可以在标签之间添加 HTML。

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
      <click-button><b>click me</b></click-button>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("click-button", {
        template: `
          <button>
            <slot></slot>
          </button>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有粗体的“点击我”文本，因为我们有`b`标签。

标签之间还可以有其他组件。

所以我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <click-button><hello-world></hello-world></click-button>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("click-button", {
        template: `
          <button>
            <slot></slot>
          </button>`
      }); app.component("hello-world", {
        template: `
          <b>click me</b>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

在`click-button`标签之间我们有了`hello-world`组件，我们会看到和以前一样的结果。

如果组件的模板不包含`slot`元素，那么标签之间的任何内容都将被丢弃。

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
      <click-button>click me</click-button>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("click-button", {
        template: `
          <button></button>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

那么“点击我”将不会显示，我们有一个空按钮。

# 渲染范围

如果我们有一个插槽，我们可以从父组件访问子组件中的项目。

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
      <click-button>
        delete {{item.name}}
      </click-button>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            item: {
              name: "this"
            }
          };
        }
      }); app.component("click-button", {
        template: `
          <button>
            <slot></slot>
          </button>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

然后我们将`item.name`字符串作为`click-button`组件中插槽内容的一部分传入。

这只是为了展示。`item`仍在父范围内。

# 后备内容

回退内容可以添加到槽中。

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
      <click-button> </click-button>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            item: {
              name: "this"
            }
          };
        }
      }); app.component("click-button", {
        template: `
          <button>
            <slot>submit</slot>
          </button>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

在槽之间添加默认内容。

由于在`click-button`标签之间没有任何内容，我们将看到显示“提交”。

![](img/db373cb2fb152745dae47205b8cd5742.png)

Photo by [Aysha Begum](https://unsplash.com/@aysha_be?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

插槽有助于我们在组件中分发内容。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**