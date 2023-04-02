# Vue 3 — HTML 和指令

> 原文：<https://javascript.plainenglish.io/vue-3-html-and-directives-f18759e6e736?source=collection_archive---------8----------------------->

![](img/ebd6628c87d677a3a2ca3658018346db.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Vue 3 处于测试阶段，可能会发生变化。**

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的流行和易用性之上。

在本文中，我们将了解如何使用 Vue 3 创建模板。

# 原始 HTML

我们可以使用`v-html`指令渲染原始的超文本标记语言。

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
      <p>{{ rawHtml }}</p>
      <p><span v-html="rawHtml"></span></p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            rawHtml: `<b>foo</b>`
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们用`data`方法将`rawHtml`属性与返回的对象一起返回。

如果我们把原始的超文本标记语言传到小胡子里，它就会被消毒。这意味着将显示原始代码。

如果我们将其传递到`v-html`指令中，它将显示为 HTML，因此它将是粗体的。

因此，如果我们使用`v-html`，我们应该小心跨站点脚本漏洞。

我们应该只显示`v-html`可信的内容。

# 属性

如果我们想动态地设置属性值，我们必须使用`v-bind`指令。

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
      <div v-bind:id="dynamicId"></div>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            dynamicId: `foo`
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

使用`v-bind`将 div 的`id`属性设置为`foo`。

如果我们有布尔属性，它的存在意味着我们将值设置为`true`。

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
      <button v-bind:disabled="disabled">Button</button>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            disabled: true
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们通过将`disabled`值传到`v-bind:disabled`使按钮失效，即`true`。

# JavaScript 表达式

我们可以把 JavaScript 表达式放在双花括号之间。

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
      {{message.split('').reverse().join('')}}
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            message: "hello world"
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

要反转模板中的`'hello world'`。

我们只能用单个表达式而不能用语句，所以我们不能写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      {{ var a = 1 }}}
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            message: "hello world"
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

Vue 模板编译器会给我们一个错误。

# 指令

指令是以`v-`开头的特殊属性。

他们期望的值是单个的 JavaScript 表达式。

`v-for`、`v-on`不在此限。

当 DOM 表达式的值发生变化时，我们会被动地将副作用应用于 DOM。

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
      <p v-if="show">hello world</p>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            show: true
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

用`v-if`显示“你好世界”信息，因为`show`是`true`。

`v-if`是一个指令，当某物的价值是真实的时，它会显示出它的价值。

![](img/c74c84a1f8a1949b0f34dcd7428375f6.png)

Photo by [Russ Ward](https://unsplash.com/@rssemfam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

模板可以有原始的 HTML、属性和指令。

指令是接受值的特殊属性。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**