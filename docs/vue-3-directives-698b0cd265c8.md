# Vue 3 —指令

> 原文：<https://javascript.plainenglish.io/vue-3-directives-698b0cd265c8?source=collection_archive---------13----------------------->

![](img/bccdbc5c3d4a672c1312000e9f09fd38.png)

Photo by [Crystal Jo](https://unsplash.com/@crystalsjo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究如何创建更复杂的指令。

# 指示性参数

我们可以通过从`binding.arg`属性中获取值来获取指令参数。

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
      <p v-absolute:[direction]="50">foo</p>
    </div> <script>
      const app = Vue.createApp({
        data() {
          return {
            direction: "right"
          };
        }
      }); app.directive("absolute", {
        mounted(el, binding) {
          el.style.position = "absolute";
          const s = binding.arg || "top";
          el.style[s] = `${binding.value}px`;
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了有一个`mounted`钩子的`absolute`指令。

它接受一个具有`arg`属性的`binding`参数，参数值由我们传入指令的方括号中。

因此，它将是`direction`值。

我们用`binding.value`设置`style`的属性，这是我们传递到等号右边的值。

此外，我们可以通过传递一个表达式作为指令的值来确定指令的值。

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
      <input type="range" min="0" max="100" v-model="padding" />
      <p v-absolute:[direction]="padding">foo</p>
    </div> <script>
      const app = Vue.createApp({
        data() {
          return {
            direction: "left",
            padding: 0
          };
        }
      }); app.directive("absolute", {
        mounted(el, binding) {
          el.style.position = "absolute";
          const s = binding.arg || "top";
          el.style[s] = `${binding.value}px`;
        },
        updated(el, binding) {
          const s = binding.arg || "top";
          el.style[s] = `${binding.value}px`;
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有添加了`updated`钩子的`absolute`指令。

`updated`钩子将获取指令的`value`的任何更新。

因此，当我们移动滑块时,“foo”文本将随之移动。

# 函数速记

我们可以用函数速记来缩短我们的指令定义。

如果我们的指令中只有`mounted`和`updated`钩子，那么我们可以使用它。

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
      <input type="range" min="0" max="100" v-model="padding" />
      <p v-absolute:[direction]="padding">foo</p>
    </div> <script>
      const app = Vue.createApp({
        data() {
          return {
            direction: "left",
            padding: 0
          };
        }
      }); app.directive("absolute", (el, binding) => {
        el.style.position = "absolute";
        const s = binding.arg || "top";
        el.style[s] = `${binding.value}px`;
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们缩短了我们的`absolute`指令，以包含一个回调，而不是一个带有钩子的对象。

它做的事情和上一个例子一样，因为我们只有`mounted`和`update`钩子。

这是一种避免重复代码的简便方法。

# 对象文字

如果我们在指令中需要多个值，我们可以传递一个对象文本给它。

然后`binding.value`有我们传入的对象。

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
      <p v-custom-text="{ color: 'green', text: 'hello!' }"></p>
    </div> <script>
      const app = Vue.createApp({}); app.directive("custom-text", (el, binding) => {
        const { color, text } = binding.value;
        el.style.color = color;
        el.textContent = text;
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建一个接受对象的`custom-text`指令。

我们从`binding.value`获得对象的`color`和`text`属性。

![](img/792a35e78badfb89c23592081b1cff1b.png)

Photo by [Mark König](https://unsplash.com/@markkoenig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用一些快捷键更容易地创建指令。

此外，指令可以接受参数和值。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**