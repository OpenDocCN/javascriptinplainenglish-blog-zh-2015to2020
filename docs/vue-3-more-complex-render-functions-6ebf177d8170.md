# Vue 3 —更复杂的渲染功能

> 原文：<https://javascript.plainenglish.io/vue-3-more-complex-render-functions-6ebf177d8170?source=collection_archive---------11----------------------->

![](img/1c23e671d9395cec3280af952266f042.png)

Photo by [Curioso Photography](https://unsplash.com/@curioso?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将看看如何用 Vue 3 创建渲染函数。

# 用渲染函数替换模板特征

我们可以用渲染函数替换模板特征。

像`v-if`和`v-for`这样的指令可以在渲染函数中替换。

`v-if`可以替换成一个`if`语句。

`v-for`可以用一系列物品代替。

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
      <todo-list :todos="todos"></todo-list>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            todos: [{ name: "eat" }, { name: "drink" }, { name: "sleep" }]
          };
        }
      }); app.component("todo-list", {
        props: ["todos"],
        render() {
          if (this.todos.length) {
            return Vue.h(
              "div",
              this.todos.map(todo => {
                return Vue.h("div", todo.name);
              })
            );
          } else {
            return Vue.h("div", "no todos.");
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们用`if`语句创建了`todo-list`组件来检查`this.todos`数组的长度。

我们调用了`map`来返回带有`name`的 div 数组。

# `v-on`

与`v-on`指令等价的是第二个参数中对象的`on`方法。

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
      <custom-div></custom-div>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("custom-div", {
        render() {
          return Vue.h(
            "div",
            {
              onClick: $event => console.log("clicked", $event.target)
            },
            "click me"
          );
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了一个带有 div 的`custom-div`组件。

第二个参数是一个具有`onClick`方法的对象，它监听 div 上的点击。

第三个参数包含 div 标记之间的内容。

# v 型车

`v-model`的等价物是`modelValue` prop 和`onInput`方法。

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
      <custom-input v-model="value"></custom-input>
      <p>{{value}}</p>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            value: ""
          };
        }
      }); app.component("custom-input", {
        props: ["modelValue"],
        render() {
          return Vue.h("input", {
            modelValue: this.modelValue,
            onInput: ev => this.$emit("update:modelValue", ev.target.value)
          });
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们有带`custom-input`组件的`modelValue`支柱。

`this.modelValue`具有道具的价值。

我们将它设置为`modelValue`属性的值。

然后我们发出`update:modelValue`事件将值发送给父节点。

现在，当我们输入一些东西时，它将与`value`状态同步。

所以我们会看到我们输入的显示。

# 事件修改器

事件修改器也可以用渲染函数代替。

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
      <custom-div></custom-div>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("custom-div", {
        render() {
          return Vue.h(
            "div",
            {
              onClick: {
                handler: ev => console.log(ev),
                capture: true
              },
              onKeyUp: {
                handler: ev => console.log(ev),
                once: true
              },
              onMouseOver: {
                handler: ev => console.log(ev),
                once: true,
                capture: true
              }
            },
            "click me"
          );
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

添加事件处理程序方法。

选项是修改器。

![](img/dcb24c873f4517b27273d69bd3ff40fe.png)

Photo by [Daniel DiNuzzo](https://unsplash.com/@ddinuzzo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用相应的渲染函数替换不同的指令。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**