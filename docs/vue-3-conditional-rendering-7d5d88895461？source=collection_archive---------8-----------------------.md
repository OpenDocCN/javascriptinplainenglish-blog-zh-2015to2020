# Vue 3 —条件渲染

> 原文：<https://javascript.plainenglish.io/vue-3-conditional-rendering-7d5d88895461?source=collection_archive---------8----------------------->

![](img/340aeb87301b8b8d113a79a2a404bdf3.png)

Photo by [Daniel DiNuzzo](https://unsplash.com/@ddinuzzo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究用 Vue 有条件地渲染项目。

# v-否则

我们可以使用`v-else`来显示一个替代项，其中`v-if`中的值为 falsy。

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
      <button @click="on = !on">toggle</button>
      <h1 v-if="on">hello</h1>
      <h1 v-else>bye</h1>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            on: true
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

当我们将`on`切换到`true`时，会显示“你好”。

否则，将显示“再见”。

`v-else`必须紧跟`v-if`或`v-else-if`元素。

否则不会被认可。

# `v-if`在`<template>`上的条件组

`v-if`可以用在`template`上，这样我们就可以有条件地显示一组元素。

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
      <button @click="on = !on">toggle</button>
      <template v-if="on">
        <h1>Title</h1>
        <p>Paragraph 1</p>
        <p>Paragraph 2</p>
      </template>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            on: true
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们将`v-if`添加到了`template`中，这样我们可以一次切换里面的所有内容。

# `v-else-if`

如果案例是`true`，我们可以使用`v-else-if`来显示一些内容。

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
      <div v-if="type === 'a'">
        a
      </div>
      <div v-else-if="type === 'b'">
        b
      </div>
      <div v-else-if="type === 'c'">
        c
      </div>
      <div v-else>
        something else
      </div>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            type: "a"
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们有`v-else-if`指令根据`type`的值显示不同的项目。

由于我们将其设置为`'a'`，我们将看到显示“a”。

`v-else-if`必须紧跟在`v-if`或另一个`v-else-if`元素之后。

# `v-show`

指令还让我们有条件地在屏幕上呈现项目。

但是不同的是，`v-show`元素总是呈现在 DOM 上，如果它的值是 falsy，它就会被 CSS 隐藏。

`v-show`不支持`template`元素，不能与`v-else`一起使用。

例如，我们可以这样使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <button @click="on = !on">toggle</button>
      <h1 v-show="on">hello</h1>
    </div>
    <script>
      const vm = Vue.createApp({
        data() {
          return {
            on: true
          };
        }
      }).mount("#app");
    </script>
  </body>
</html>
```

我们有`v-show`指令，它将切换 CSS `display`属性来显示或隐藏 h1 元素。

# `v-if` vs `v-show`

`v-if`是真正的条件渲染，因为所有的事件监听器和子组件在不渲染时都会被销毁。

`v-if`是懒惰的，所以如果它的值是 falsy，那么直到它变成`true`才会被渲染。

`v-show`要简单得多，它只是切换`display` CSS 属性来改变显示。

如果我们需要经常切换某些东西，`v-show`更好，而`v-if`适用于其他情况。

![](img/be43ad0f6f0708b52a3a661ecb105a0a.png)

Photo by [berenice melis](https://unsplash.com/@brrknees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`v-if`和`v-show`来有条件的渲染物品。

`v-if`改变 DOM 结构，`v-show`只改变 CSS。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**