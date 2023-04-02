# Vue 3 —自定义事件

> 原文：<https://javascript.plainenglish.io/vue-3-custom-events-d2f310fe34c9?source=collection_archive---------5----------------------->

![](img/ecc31cbf3809c4eb86639ac2195d770d.png)

Photo by [Jakob Dalbjörn](https://unsplash.com/@jakobdalbjorn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何用 Vue 3 发出和处理定制事件。

# 自定义事件

我们可以用`this.$emit`方法发出定制事件。

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
      <component-a @increment-count="count++"></component-a>
      <p>{{count}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            count: 0
          };
        }
      }); app.component("component-a", {
        template: `
          <div>
            <button @click='this.$emit("increment-count")'>click      me</button>
          </div>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们从`component-a`发出`increment-count`事件。

然后，我们在父 Vue 实例的模板中监听相同的事件:

```
<component-a @increment-count="count++"></component-a>
```

事件名称的大小写必须相同才能正常工作。

因此，当我们点击“点击我”按钮时，我们可以将`count`增加 1。

# 定义自定义事件

我们可以用组件的`emits`选项定义自定义事件。

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
      <component-a @increment-count="count++"></component-a>
      <p>{{count}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            count: 0
          };
        }
      }); app.component("component-a", {
        emits: ["increment-count"],
        template: `
          <div>
            <button @click='$emit("increment-count")'>click me</button>
          </div>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`emits`选项，它有一个`component-a`可以发出的事件名称字符串数组。

这为我们的组件提供了更好的文档，因为我们可以限制可以发出的事件种类。

# 验证发出的事件

我们可以添加代码来验证发出的事件。

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
      <component-a @increment-count="count += $event"></component-a>
      <p>{{count}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            count: 0
          };
        }
      }); app.component("component-a", {
        emits: {
          ["increment-count"](value) {
            return value === 2;
          }
        },
        template: `
          <div>
            <button @click='$emit("increment-count", 2)'>click me</button>
          </div>`
      });app.mount("#app");
    </script>
  </body>
</html>
```

以确保我们总是将 2 传递给`$emit`方法。

我们在使用`increment-count`方法的`component-a`对象中有`emits`属性。

它采用了`value`参数，这是我们作为第二个参数传入的值。

在方法中，我们将验证结果作为布尔值返回。

如果有效，我们返回`true`。

否则，我们返回`false`。

# `v-model`论据

`v-model`也可以验证参数。

我们可以用`props`属性检查属性值。

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
      <component-a v-model:foo="bar"></component-a>
      <p>{{bar}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            bar: ""
          };
        }
      }); app.component("component-a", {
        props: {
          foo: String
        },
        template: `
          <input 
            type="text"
            :value="foo"
            @input="$emit('update:foo', $event.target.value)">
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有`props`属性，它有键`foo`。

值是`String`，所以`foo`必须是一个字符串。

我们的输入发出了`update:foo`事件，所以我们将输入的值发送回父组件。

然后我们可以将父属性与`v-model:foo`指令绑定，这样我们就可以将`foo`的值与`bar`状态的值同步。

因此，当我们在输入框中输入时，我们会看到我们输入的值显示出来。

![](img/e6c4eead06da0149ea6a6476d6da393b.png)

Photo by [Product School](https://unsplash.com/@productschool?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过调用带有事件名和可选的第二个参数值的`this.$emit`来发出定制事件。

然后我们可以用`@`指令速记来听。

发出的值可以通过`$event`对象访问。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**