# Vue 3 — v 型和部件

> 原文：<https://javascript.plainenglish.io/vue-3-v-model-and-components-de2d39527f80?source=collection_archive---------3----------------------->

![](img/98a1e7db7b1c2e4b6cabc6fc225e6e1e.png)

Photo by [Drew Farwell](https://unsplash.com/@outdoor_junkiez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何让我们的组件接受`v-model`指令。

# 在组件上使用`v-model`

`v-model`只是监听`input`事件和绑定`value`道具的简称。

例如，以下内容:

```
<input v-model="message" />
```

与以下内容相同:

```
<input :value="message" @input="message = $event.target.value" />
```

我们将`message`的值作为`value`属性的值传递。

当发出`input`事件时，我们将`message`设置为`$event.target.value`的值。

当我们在一个组件上使用`v-model`时，`v-model`略有不同:

```
<custom-input
  :model-value="message"
  @update:model-value="message = $event"
></custom-input>
```

这部分不同于 Vue 2，输入元素和组件做的是同样的事情。

现在我们只需使用`model-value`属性，并以`model-value`作为参数发出`opdate`事件。

为此，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <custom-input v-model="message"></custom-input>
      <p>{{message}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            message: ""
          };
        }
      }); app.component("custom-input", {
        props: ["modelValue"],
        template: `
          <input
            :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)"
          >
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了一个使用`modelValue`道具的`custom-input`组件。

模板中的输入使用`modelValue`作为其值。

当从输入中发出`input`事件时，会发出带有`$event.target.value`属性的`update:modelValue`事件。

`$event.target.value`有我们输入的输入值。

然后在父 Vue 实例中，我们有:

```
<custom-input v-model="message"></custom-input>
```

在模板中。

我们将`message`绑定到`custom-input`上，这样当我们输入`custom-input`时，就会看到`message`的值显示出来。

# 利用槽的内容分发

我们可以用插槽在组件中分发内容。

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
      <message-box>
        hello world
      </message-box>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("message-box", {
        template: `
          <div>
            <strong>Message: </strong>
            <slot></slot>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建内部带有`slot`元素的`message-box`组件。

这样，我们可以像在父 Vue 实例的模板中一样，通过在标签之间传递项目来显示我们想要的项目。

# 动态组件

有了 Vue 3，我们可以动态渲染组件。

这对于选项卡式界面等功能非常有用。

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
      <button @click='currentTabComponent = "tab-foo"'>foo</button>
      <button @click='currentTabComponent = "tab-bar"'>bar</button>
      <button @click='currentTabComponent = "tab-baz"'>baz</button>
      <component :is="currentTabComponent"></component>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            currentTabComponent: "tab-foo"
          };
        }
      }); app.component("tab-foo", {
        template: `
          <div>
            foo
          </div>
        `
      }); app.component("tab-bar", {
        template: `
          <div>
            bar
          </div>
        `
      }); app.component("tab-baz", {
        template: `
          <div>
            baz
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了 3 个组件，分别叫做`tab-foo`、`tab-bar`和`tab-baz`，我们通过创建 3 个按钮来设置变量`currentTabComponent`的值，从而动态地设置它们。

然后，我们将该值传递到`component`组件的`is`属性中，让我们用`currentTabComponent`指示的名称显示我们想要的组件。

`component`可以和普通的 HTML 元素一起使用，但是它们会被当作组件对待。

所以所有属性都将被绑定为 DOM 属性。

![](img/1eeaeaf216f216fa6491fef74744f68d.png)

Photo by [Michael Olsen](https://unsplash.com/@mganeolsen?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`v-model`来创建定制的表单控件组件。

组件可以有插槽，让我们把它们的内容放在我们想要的位置。

并且可以动态呈现组件。

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**