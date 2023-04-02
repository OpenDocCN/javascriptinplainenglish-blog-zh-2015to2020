# Vue 3 中类绑定的工作方式

> 原文：<https://javascript.plainenglish.io/vue-3-class-bindings-67d5e39c7342?source=collection_archive---------10----------------------->

![](img/bca2a0e14b1f80fdbaf7ab02939a500d.png)

GFPhoto by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究类绑定。

# 类绑定

为了让我们能够动态地设计我们的应用程序，我们必须能够用它来设置动态的类和样式。

设置类的一种方法是使用 th 对象语法:

```
<div :class="{ active: isActive }"></div>
```

`active`是类别的名称，`isActive`是`active`打开时的条件。

只有`isActive`是真实的。

我们可以在一个对象中有多个类。

例如，我们可以写:

```
<div
  class="static"
  :class="{ active: isActive, 'has-error': hasError }"
></div>
```

我们有静态的`static`类，它总是被添加。

我们在`:class`绑定中有一个类，它只在条件为真时才打开。

绑定对象不必是内联的，所以我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <div class="static" :class="classObj"></div>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            classObj: {
              active: true,
              "has-error": false
            }
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

将`classObj`作为`:class`绑定的值传入。

# 数组语法

我们也可以在数组中包含类名。

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
      <div :class="[activeClass, errorClass]"></div>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            activeClass: "active",
            errorClass: "has-error"
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们将`activeClass`和`errorClass`放在一个数组中。

我们可以用 JavaScript 代码打开和关闭它们。

如果我们想有条件地切换一个类，我们可以为它们编写三元表达式。

例如，我们可以写:

```
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```

`activeClass`是只加是`isActive`是真。

否则，表达式返回空字符串。

# 带有组件的类

我们可以用组件添加类。

例如，如果我们有:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <hello-world class="baz qux"></hello-world>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("hello-world", {
        template: `<p class="foo bar">hello world</p>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们用 p 元素上的`foo`和`bar`类创建了`hello-world`组件。

此外，我们在`hello-world`组件本身上有`baz`和`qux`类。

来自两个地方的类将被合并，因为 p 元素是组件的根元素。

所以`baz`和`qux`类将被添加到 p 元素中。

类绑定也可以。

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
      <hello-world :class="{ active: isActive }"></hello-world>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            isActive: true
          };
        }
      }); app.component("hello-world", {
        template: `<p class="foo bar">hello world</p>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

因为`isActive`是`true`，所以我们在 p 元素上有了与`foo`和`bar`结合的`active`类。

![](img/1c55a94002efd8c986069c2426ea9528.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一个对象或数组动态地添加 HTML 类。

它们可以被添加到 Vue 组件和 HTML 元素中。

如果它们被添加到组件中，那么它们将被添加到组件的根元素中。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**