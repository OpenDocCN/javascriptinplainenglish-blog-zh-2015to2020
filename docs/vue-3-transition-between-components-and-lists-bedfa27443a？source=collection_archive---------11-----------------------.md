# Vue 3 —组件和列表之间的转换

> 原文：<https://javascript.plainenglish.io/vue-3-transition-between-components-and-lists-bedfa27443a?source=collection_archive---------11----------------------->

![](img/2e3129da5ca3d9463d565546de991e5d.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究如何在组件和列表之间创建过渡效果。

# 组件之间的转换

我们可以用`transition`和`component`组件在组件之间转换。

`component`组件用于通过设置组件名称在组件之间动态切换。

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
      <button @click="currentComponent = 'foo'">foo</button>
      <button @click="currentComponent = 'bar'">bar</button>
      <button @click="currentComponent = 'baz'">baz</button>
      <transition name="bounce" mode="out-in">
        <component :is="currentComponent"></component>
      </transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            currentComponent: "foo"
          };
        },
        components: {
          foo: {
            template: "<div>foo</div>"
          },
          bar: {
            template: "<div>bar</div>"
          },
          baz: {
            template: "<div>baz</div>"
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了 3 个组件，`foo`、`bar`和`baz`。

为了在它们之间转换，我们创建了 3 个按钮来改变组件名。

我们也有`name`道具和设置过渡名称。

我们在`style`标签中也有过渡样式。

当我们单击按钮时，在看到新组件的内容之前，我们会看到一个过渡效果。

# 列出过渡

我们可以给列表添加过渡效果。

为此，我们可以使用`transition-group`组件。

与`transition`不同，它呈现实际的元素。

默认呈现一个`span`。

我们可以用`tag`属性来改变这一点。

过渡模式不可用，因为我们不是在互斥元素之间交替。

里面的元素总是需要有一个唯一的`key`属性。

CSS 转换类将应用于内部元素，而不是容器本身。

# 列出进入/离开转换

我们可以用`transition-group`组件添加一个进入或离开转换列表。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <style>
      .list-enter-active,
      .list-leave-active {
        transition: all 1s ease;
      }
      .list-enter-from,
      .list-leave-to {
        opacity: 0;
        transform: translateY(35px);
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="add">Add</button>
      <button @click="remove">Remove</button>
      <transition-group name="list" tag="div">
        <p v-for="item in items" :key="item">
          {{ item }}
        </p>
      </transition-group>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            items: Array(10)
              .fill()
              .map(() => Math.random()),
            nextNum: 10
          };
        },
        methods: {
          randomIndex() {
            return Math.floor(Math.random() * this.items.length);
          },
          add() {
            this.items.splice(this.randomIndex(), 0, Math.random());
          },
          remove() {
            this.items.splice(this.randomIndex(), 1);
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用一组随机数创建了一个列表。

我们添加了添加和删除按钮，让我们可以添加和删除项目。

过渡效果在样式标签中。

我们添加的效果是不透明度变化和垂直平移效果。

在模板中，我们将`transition-group`组件的`name`属性设置为`list`，以此作为转换类名的前缀。

`tag`是我们为容器渲染的标签。

我们根据需要在 p 元素中添加了`key`道具，它是从`items`列表中呈现的。

这样，Vue 就可以知道每个项目在哪里，并正确地将它们制作成动画。

最后，我们有`add`和`remove`方法让我们添加和删除`items`数组中的项目。

`randomIndex`生成随机索引以添加或删除项目。

现在，当我们单击添加或删除时，我们将看到应用的过渡效果。

![](img/e612c384127a3e83f5cd9eac6425b0b4.png)

Photo by [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在组件之间转换，并为列表项添加转换效果。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**