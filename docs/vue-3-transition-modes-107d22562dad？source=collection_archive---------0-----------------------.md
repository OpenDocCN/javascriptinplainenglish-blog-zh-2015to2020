# Vue 3 —过渡模式

> 原文：<https://javascript.plainenglish.io/vue-3-transition-modes-107d22562dad?source=collection_archive---------0----------------------->

![](img/3ce4021abc59dbed4e47433265de4a6e.png)

Photo by [Maksym Potapenko](https://unsplash.com/@potapenko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看如何创建各种过渡效果。

# 初始渲染时的过渡

我们可以在初始渲染带有`appear`属性的节点时应用一个过渡。

例如，我们可以这样使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .bounce-enter-active {
        animation: bounce-in 1.3s;
      }
      .bounce-leave-active {
        animation: bounce-in 1.3s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.8);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <transition name="bounce" appear>
        <p>hello</p>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({});
      app.mount("#app");
    </script>
  </body>
</html>
```

我们只需将`appear`属性添加到我们的`transition`组件中，以便在加载元素时出现转换。

# 元素之间的过渡

我们可以在符合`v-if`和`v-else`指令的元素之间添加一个转换。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .bounce-enter-active {
        animation: bounce-in 1.3s;
      }
      .bounce-leave-active {
        animation: bounce-in 1.3s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.8);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="show = !show">toggle</button>
      <transition name="bounce">
        <p v-if="show">hello</p>
        <p v-else>bye</p>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            show: false
          };
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了一个过渡效果，在`transition`组件中的 2 p 元素之间过渡。

使用`v-if`和`v-else`指令自动转换。

我们还可以在任意数量的元素之间转换。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .bounce-enter-active {
        animation: bounce-in 0.8s;
      }
      .bounce-leave-active {
        animation: bounce-in 0.8s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.8);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="count++">increment</button>
      <transition name="bounce">
        <p v-if="count % 3 === 0" key="0">foo</p>
        <p v-else-if="count % 3 === 1" key="1">bar</p>
        <p v-else key="2">baz</p>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            count: 0
          };
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有`v-else-if`和`v-else`指令来确保同一时间只有一个被渲染。

这样，我们可以继续使用`transition`组件在多个元素之间制作动画。

# 过渡模式

如果我们处理更复杂的动画，我们可以设置`mode`道具来设置运行的过渡效果。

默认情况下，`in-out`和`out-in`转换同时运行。

有了这个道具，我们可以让我们的组件应用这样或那样的效果。

通常情况下，`out-in`模式就是我们想要的。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .bounce-enter-active {
        animation: bounce-in 1.3s;
      }
      .bounce-leave-active {
        animation: bounce-in 1.3s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.8);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="show = !show">toggle</button>
      <transition name="bounce" mode="out-in">
        <p v-if="show">hello</p>
        <p v-else>bye</p>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            show: false
          };
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

有了`mode`道具组，我们不会看到多个元素同时被激活。

![](img/6a16dae4bb7f0a26f95fca09810a5bcf.png)

Photo by [Ivan Diaz](https://unsplash.com/@mdi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些道具和指令创建多个元素之间的过渡。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**