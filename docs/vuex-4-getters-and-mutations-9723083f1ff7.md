# Vuex 4 —吸气剂和突变

> 原文：<https://javascript.plainenglish.io/vuex-4-getters-and-mutations-9723083f1ff7?source=collection_archive---------11----------------------->

![](img/54b81b236b257be607b4677562a73379.png)

Photo by [Alekon pictures](https://unsplash.com/@alekonpictures?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuex 4 处于测试阶段，可能会有变化。

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 方法风格的 Getters

我们可以创建接受一个或多个参数的 getters。

为此，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <div>
        <p>{{getTodoById(1).text}}</p>
      </div>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          todos: [
            { id: 1, text: "eat", done: true },
            { id: 2, text: "sleep", done: false }
          ]
        },
        getters: {
          getTodoById: (state) => (id) => {
            return state.todos.find((todo) => todo.id === id);
          }
        }
      }); const app = Vue.createApp({
        computed: {
          ...Vuex.mapGetters(["getTodoById"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了一个状态为`todos`的商店。

我们还有`getTodosById` getter 方法，它返回一个函数，该函数接受`id`参数并返回与给定的`id`值匹配的`state.todos`条目。

在 root Vue 实例中，我们调用了`Vuex.mapGetters`来将 getters 映射到方法。

这样，我们可以像在模板中那样在组件中调用它。

# 突变

我们可以使用突变来改变我们的存储状态。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="decrement">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          decrement(state) {
            state.count--;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          decrement() {
            this.$store.commit("decrement");
          }
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有`decrement`突变，它将`state.count`的值减少 1。

然后我们可以调用`this.$store.commit`方法来调用我们的突变来更新`count`状态。

我们还有一个计算的`count`属性来获取`count`状态的值。

# 提交有效负载

我们可以创造一种突变来获取有效载荷。

变异方法接受值为的第二个参数。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://unpkg.com/vuex@4.0.0-beta.4/dist/vuex.global.js"></script>
    <title>App</title>
  </head>
  <body>
    <div id="app">
      <button @click="decrement">decrement</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          decrement(state, n) {
            state.count -= n;
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          decrement() {
            this.$store.commit("decrement", 5);
          }
        },
        computed: {
          count() {
            return this.$store.state.count;
          }
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们给`decrement`变异方法增加了一个`n`参数，用`n`减去`state.count`。

然后我们用第二个参数调用`commit`来传递我们想要减去`state.count`的值。

现在，当我们单击减量按钮时，我们可以看到显示的`count`值。

# 结论

我们可以用 Vuex 4 创建突变来修改状态数据。

getter 可以定义为一个方法风格的 getter，让我们传入参数。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**