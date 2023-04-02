# Vue 3 —过渡和动画

> 原文：<https://javascript.plainenglish.io/vue-3-transitions-and-animations-bd2276067e87?source=collection_archive---------8----------------------->

![](img/319aa9e64a5716e554edafa84abde205.png)

Photo by [Pontus Wellgraf](https://unsplash.com/@wellgraf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看 CSS 过渡和动画。

# CSS 过渡

组件最常用于 CSS 过渡。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .fade-enter-active {
        transition: all 0.3s ease-out;
      } .fade-leave-active {
        transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
      } .fade-enter-from,
      .fade-leave-to {
        transform: translateX(20px);
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

我们有`transition`组件，它有`name`属性来设置转换类名的前缀。

我们在 CSS 中设置`transition`属性，让我们设置过渡效果。

`ease-out`用于输入效果。

而`cubic-bezier`是我们的缓和曲线，用来调整过渡的速度。

# CSS 动画

CSS 动画的应用方式与 CSS 过渡相同。

过渡和动画的不同之处在于插入元素后不会立即移除`v-enter-from`。

而是在`animationend`事件被触发后被移除。

例如，我们可以通过编写以下代码来创建带有关键帧的动画:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .bounce-enter-active {
        animation: bounce-in 1.5s;
      }
      .bounce-leave-active {
        animation: bounce-in 1.5s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.5);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="show = !show">
        Toggle
      </button> <transition name="bounce">
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

我们在 CSS 中有`keyframes`，这样我们可以在我们想要的阶段定义我们的动画效果。

我们在开始、中途和结束时都有一个`transform`效果。

# 自定义过渡类

我们可以设置自定义的转换类，而不是使用预设的类名。

为此，我们只需设置一些道具。

我们可以设置的道具有:

*   `enter-from-class`
*   `enter-active-class`
*   `enter-to-class`
*   `leave-from-class`
*   `leave-active-class`
*   `leave-to-class`

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .enter-active {
        animation: bounce-in 1.5s;
      }
      .leave-active {
        animation: bounce-in 1.5s reverse;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0);
        }
        50% {
          transform: scale(1.5);
        }
        100% {
          transform: scale(1);
        }
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="show = !show">
        Toggle
      </button> <transition
        enter-active-class="enter-active"
        leave-active-class="leave-active"
      >
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

我们用`enter-active`和`leave-active`类来代替默认的类名，因为我们设置了`enter-active-clas`和`leave-active-class`属性。

它们会覆盖默认的类名。

![](img/a732635a482ede6c2265e473488b4317.png)

Photo by [Rhythm Goyal](https://unsplash.com/@rhythm596?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以更改动画和过渡的名称。

过渡和动画之间略有不同。

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**