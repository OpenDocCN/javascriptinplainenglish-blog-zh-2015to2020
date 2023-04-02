# Vue 3 —混合剂

> 原文：<https://javascript.plainenglish.io/vue-3-mixins-3c7cb1a76ff3?source=collection_archive---------6----------------------->

![](img/facc0b19961c559d6d0b8418de39d5bb.png)

Photo by [Mike Petrucci](https://unsplash.com/@mikepetrucci?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何创建 mixins 来使我们的代码可重用。

# 混合蛋白

Mixins 是一种灵活的方式，让我们可以向 Vue 组件添加可重用的功能。

mixin 对象可以有任何组件选项。

当一个组件使用 mixin 时，mixin 将被合并到组件自己的选项中。

例如，我们可以创建一个 mixin，并通过编写以下代码来使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <p>app</p>
    </div> <script>
      const mixin = {
        created() {
          this.foo();
        },
        methods: {
          foo() {
            console.log("foo");
          }
        }
      }; const app = Vue.createApp({
        mixins: [mixin]
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们用一个具有`mixins`属性的对象调用了`Vue.createApp`。

值的就是我们的`mixin`米辛。

它只是一个对象，在我们期望的地方有组件属性。

它们会自动合并到组件中。

因此，将调用`this.foo`方法，我们将看到控制台日志运行。

# 选项合并

将使用适当的策略合并选项。

如果有任何冲突，组件的数据优先于 mixin。

同名的钩子函数被合并成一个数组，这样所有的函数都会被调用。

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
      <p>app</p>
    </div> <script>
      const mixin = {
        created() {
          console.log("mixin hook");
        }
      }; const app = Vue.createApp({
        mixins: [mixin],
        created() {
          console.log("app hook");
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

然后我们按顺序记录了`'mixin hook'`和`'app hook'`。

期望像`methods`、`components`和`directives`这样的对象值的选项将被合并到同一个对象中。

当存在冲突键时，组件的选项将优先。

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
      <p>app</p>
    </div> <script>
      const mixin = {
        methods: {
          foo() {
            console.log("foo");
          },
          conflicting() {
            console.log("mixin conflicting");
          }
        }
      }; const app = Vue.createApp({
        mixins: [mixin],
        methods: {
          baz() {
            console.log("bar");
          },
          conflicting() {
            console.log("app conflicting");
          }
        }
      }); const vm = app.mount("#app");
      vm.conflicting();
      vm.foo();
      vm.baz();
    </script>
  </body>
</html>
```

然后我们会看到:

```
app conflicting 
foo 
baz
```

记录在案。

Vue 实例的`conflicting`方法在 mixin one 上使用。

但是`foo`和`baz`被合并到 Vue 实例中，因为它们之间没有冲突。

# 全球混合

我们可以用`app.mixin`方法创建全局混合。

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
      <p>app</p>
    </div> <script>
      const app = Vue.createApp({
        foo: "bar"
      }); app.mixin({
        created() {
          const foo = this.$options.foo;
          console.log(foo);
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们将`foo`属性添加到我们的 Vue 实例中。

这可以通过 mixin 中的`this.$options`属性来访问。

所以我们将看到从我们的`created`钩子中记录的`'bar'`。

一旦我们在全球范围内应用了 mixin，那么它将会影响之后创建的每一个 Vue 实例。

![](img/cfa45617f9b2e9f12c4248aa5e608eeb.png)

Photo by [Alexey Ruban](https://unsplash.com/@intelligenciya?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Mixins 是可重用的代码片段，可以集成到我们的 Vue 组件中。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**