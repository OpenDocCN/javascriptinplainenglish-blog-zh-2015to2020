# Vue 3 —过渡

> 原文：<https://javascript.plainenglish.io/vue-3-transitions-b7f6a32ddd90?source=collection_archive---------10----------------------->

![](img/97b2b956a03f9db65c81c752d168b98a.png)

Photo by [Ashley Rich](https://unsplash.com/@a5hleyrich?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看部分过渡。

# 时机

UI 转换有时间让我们设置动画的持续时间。

我们可能对不同状态之间的转换有不同的计时。

如果转换没有中间状态，则计时在 0.1 秒和 0.4 秒之间

# 松开

这种放松让我们给动画增加了一些深度。

没有缓和，动画将是线性的和无趣的。

我们想用`ease-out`表示入口，用`ease-in`表示出口。

# 进入和离开过渡

我们可以使用`transition`包装器组件来添加进入和离开转换。

例如，我们可以用`transition`组件添加一个简单的转换，编写如下代码:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="[https://unpkg.com/vue@next](https://unpkg.com/vue@next)"></script>
    <style>
      .fade-enter-active,
      .fade-leave-active {
        transition: opacity 1s ease;
      }.fade-enter-from,
      .fade-leave-to {
        opacity: 0;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="show = !show">
        Toggle
      </button> <transition name="fade">
        <p v-if="show">hello</p>
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

我们通过用`transition`组件包装 p 元素来添加转换。

然后，我们将它的`name`属性设置为我们将用于样式化转换的类的前缀。

我们将其设置为`fade`，因此所有的过渡类都以`fade-`开始。

我们有`fade-enter-active`，`fade-leave-active`类来显示一个淡入淡出的效果。

我们有`fade-enter-from`和`fade-leave-to`类来使内容透明，以使内容在`show`为`false`时消失。

Vue 将检测目标元素是否应用了 CSS 过渡或动画。

CSS 转换类将在适当的时候被添加或删除。

如果没有检测到 CSS 转换或动画，也没有提供 JavaScript 挂钩，那么 ODM 操作将在下一个浏览器动画帧上运行。

# 过渡类

我们可以对几个过渡类应用样式。

`v-enter-from`在元素插入前应用，元素插入后移除一帧。

`v-enter-active`在动画插入之前应用，当过渡或动画结束时移除。

`v-enter-to`在元素插入后添加一帧，并在转场或动画结束时移除。

`v-leave-from`在 1 帧后触发并移除离开过渡时立即应用。

`v-leave-active`适用于整个离职阶段。

当“离开”过渡被触发时，它会立即被添加，当过渡或动画结束时，它会被移除。

`v-leave-to`在离开转场被触发后的一帧添加，并在转场或动画结束时移除。

![](img/f4f941cdc947b1a189b18375637851bc.png)

Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`transition`组件轻松添加过渡。

一旦我们使用了它，我们就可以添加 CSS 样式来应用我们想要的效果。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**