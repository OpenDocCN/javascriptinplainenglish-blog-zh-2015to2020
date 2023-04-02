# Vue 3 —参考和边缘案例

> 原文：<https://javascript.plainenglish.io/vue-3-refs-and-edge-cases-aec75bd0589e?source=collection_archive---------6----------------------->

![](img/40cb5de59156d013d4a6815280b55631.png)

Photo by [Andrew Hughes](https://unsplash.com/@hughesy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何在 Vue 3 中使用模板引用和边缘案例。

# 模板参考

有时我们可能需要直接访问 DOM。

为了让我们做到这一点，我们可以给我们想要访问的 DOM 元素分配一个 ref，然后我们可以在组件代码中访问它。

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
      <input ref="input" />
    </div>
    <script>
      const app = Vue.createApp({
        methods: {
          focusInput() {
            this.$refs.input.focus();
          }
        },
        mounted() {
          this.focusInput();
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

装载 Vue 实例时聚焦输入。

我们将`input` ref 分配给我们的输入。

然后我们在它上面调用`focus`来关注输入。

我们还可以添加一个对组件本身的引用，并使用它来调用`focusInput`。

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
      <custom-input ref="customInput"></custom-input>
    </div>
    <script>
      const app = Vue.createApp({
        mounted() {
          this.$refs.customInput.focusInput();
        }
      }); app.component("custom-input", {
        methods: {
          focusInput() {
            this.$refs.input.focus();
          }
        },
        template: `<input ref='input' />`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们将输入放入`custom-input`组件。

我们有在输入端调用`focus`的`focusInput`方法。

我们给`custom-input`组件分配了一个 ref，这样我们就可以调用它的`focusInput`方法。

所以我们得到了和之前一样的结果。

`$refs`仅在组件渲染后填充。

这是直接子组件操作的最后手段。

我们应该避免从模板和计算属性中访问引用。

# 边缘案例

在 Vue 3 中有一些边缘情况是通常的构造无法解决的。

# 控制更新

通常，我们依靠 Vue 的反应系统来更新我们的组件。

但有时，我们可能需要控制组件的更新方式。

Vue 提供了实现这一点的方法。

# 强制更新

我们可以通过在组件上使用`$forceUpdate`方法来强制更新。

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
      <button @click="forceUpdate">update</button>
      <p>{{new Date()}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        methods: {
          forceUpdate() {
            this.$forceUpdate();
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

为了做到这一点。

我们的模板中有一个非反应式表达式，所以我们可以调用`$forceUpdate`用新的日期更新组件。

# 用`v-once`创建静态组件

我们可以用`v-once`指令创建静态组件。

它使带有指令的元素内的内容只更新一次。

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
      <div v-once>
        hello world
      </div>
    </div>
    <script>
      const app = Vue.createApp({}); app.mount("#app");
    </script>
  </body>
</html>
```

那么一旦组件被渲染一次，Vue 就不会试图更新它。

![](img/cfd2fb525748c9d5fee4ea5092a01406.png)

Photo by [Kevin Noble](https://unsplash.com/@nobleshots?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

模板引用让我们可以直接访问组件方法和 DOM 元素。

我们还可以用`$forceUpdate`方法和`v-once`指令强制更新，以显示静态内容。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**