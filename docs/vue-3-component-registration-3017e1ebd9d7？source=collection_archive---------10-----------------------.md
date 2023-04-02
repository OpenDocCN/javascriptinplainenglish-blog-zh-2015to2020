# Vue 3 —组件注册

> 原文：<https://javascript.plainenglish.io/vue-3-component-registration-3017e1ebd9d7?source=collection_archive---------10----------------------->

![](img/3461644dcf7468e543d95fa07506da16.png)

Photo by [Elizeu Dias](https://unsplash.com/@elishavision?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何向 Vue 3 注册我们的组件。

# 组件名称

当我们注册一个组件时，我们必须给它一个名字。

例如，我们可以用`app.component`方法全局注册一个组件。

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
      <foo></foo>
    </div>
    <script>
      const app = Vue.createApp({});
      app.component("foo", {
        template: `
          <div>
            foo
          </div>
        `
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

创建一个可以在我们的 Vue 实例中使用的`foo`组件。

名称是小写的，多个单词可以用连字符连接。

这个约定将避免与当前和未来的 HTML 元素冲突。

名称的大小写可以是 kebab case，用连字符连接名称。

我们也可以使用 PascalCase 作为名称。

然而，如果我们将组件直接添加到 DOM 中，那么只有 kebab-case 是有效的。

`app.component`全局注册一个组件。这意味着任何组件都可以使用代码中的任何其他组件。

# 本地注册

全球注册并不理想。

我们不希望组件之间有名称冲突，也不希望有我们不需要的额外依赖。

要在本地注册一个组件，我们可以向组件添加一个`components`属性。

我们的组件可以被定义为 JavaScript 对象。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="[https://unpkg.com/vue@next](https://unpkg.com/vue@next)"></script>
  </head>
  <body>
    <div id="app">
      <component-a></component-a>
      <component-b></component-b>
    </div>
    <script>
      const ComponentA = {
        template: `<p>component a</p>`
      };
      const ComponentB = {
        template: `<p>component b</p>`
      }; const app = Vue.createApp({
        components: {
          "component-a": ComponentA,
          "component-b": ComponentB
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建两个组件，分别叫做`component-a`和`component-b`。

我们在`ComponentA`和`ComponentB`对象中定义了我们的组件。

然后，我们通过将它们放在`components`属性中，在根 Vue 实例中本地注册它们。

键是组件名，值是组件本身。

这样，我们只能在根 Vue 实例中使用`component-a`和`component-b`。

本地注册的组件在子组件中不可用。

如果我们想让他们可用，我们必须注册他们。

# 模块系统中的本地注册

如果我们想在一个模块系统中本地注册一个组件，那么我们可以这样写:

```
import ComponentA from './ComponentA'
import ComponentB from './ComponentB'export default {
  components: {
    ComponentA,
    ComponentB
  }
  // ...
}
```

我们用简写添加组件的键和值，因为我们想把组件名命名为与变量名相同。

![](img/7e6104cbfe47dbae2afeaf044c65f744.png)

Photo by [Dion Tavenier](https://unsplash.com/@diotav?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`app.component`方法注册组件，以便全局创建和注册一个组件。

为了在本地注册它们，我们将组件创建为对象，然后通过将它们放入 Vue 实例的`components`属性中来注册它们。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**