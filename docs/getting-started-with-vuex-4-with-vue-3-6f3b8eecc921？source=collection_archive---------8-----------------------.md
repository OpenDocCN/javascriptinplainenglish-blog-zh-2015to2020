# 使用 Vue 3 开始使用 Vuex 4

> 原文：<https://javascript.plainenglish.io/getting-started-with-vuex-4-with-vue-3-6f3b8eecc921?source=collection_archive---------8----------------------->

![](img/1d9415cec26309196d0c212406b3c37d.png)

Photo by [Ryan Wallace](https://unsplash.com/@accrualbowtie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Vuex 4 处于测试阶段，可能会有变化。**

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 装置

我们可以从 CDN 安装。

为此，我们为 Vuex 添加脚本标记。

然后我们可以用它来编写应用程序:

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
      <button @click="increment">increment</button>
      <p>{{count}}</p>
    </div>
    <script>
      const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment(state) {
            state.count++;
          }
        }
      }); const app = Vue.createApp({
        methods: {
          increment() {
            this.$store.commit("increment");
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

我们包含了 Vuex 的全球版本，这样我们就可以将它与 HTML 脚本标签一起使用。

在上面的代码中，我们用`Vuex.Store`构造函数创建了一个商店。

`state`有商店的状态。

我们有`count`州与伯爵。

`mutations`有储存突变。

它有`inctrement`突变方法来增加`count`状态。

然后在我们的根 Vue 实例中，我们有调用`this.$store.commit`方法来增加`count`状态的`increment`方法。

我们有计算出的`count`属性从存储中返回`count`状态。

商店状态可通过`this.$store.state`属性访问。

当我们单击 increment 按钮时，我们调用`increment`方法。

然后我们看到`count`状态通过 computed 属性更新并显示在模板中。

我们必须记住加上:

```
app.use(store);
```

以便商店可以使用我们的 Vue 3 应用程序。

这些变化是反应性的，所以无论何时商店的状态发生变化，我们都会得到组件的最新变化。

# 吸气剂

我们可以添加 getters 来简化状态获取。

我们可以使用 getters 来获取状态的最新值，而不是计算属性。

要添加一个 getter，我们可以写:

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
        <p v-for="d of doneTodos" :key="d.id">{{d.text}}</p>
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
          doneTodos: (state) => {
            return state.todos.filter((todo) => todo.done);
          }
        }
      }); const app = Vue.createApp({
        computed: {
          ...Vuex.mapGetters(["doneTodos"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

在我们的存储中创建一个`doneTodos` getter，并通过`Vuex.mapGetters`方法在我们的 Vue 实例中使用它。

参数是带有 getters 名称的字符串数组。

我们将计算出的属性扩展到`computed`对象中，这样我们就可以访问组件中的。

在模板中，我们呈现来自 getter 的项目。

`doneTodos`方法具有带状态的`state`参数。

它还可以接收其他 getters 作为第二个参数。

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
      <div>
        <p v-for="d of doneTodos" :key="d.id">{{d.text}}</p>
        <p>{{doneTodosCount}} done</p>
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
          doneTodos: (state) => {
            return state.todos.filter((todo) => todo.done);
          },
          doneTodosCount: (state, getters) => {
            return getters.doneTodos.length;
          }
        }
      }); const app = Vue.createApp({
        computed: {
          ...Vuex.mapGetters(["doneTodos", "doneTodosCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们有带有`getters`对象的`doneTodosCount` getter，该对象有我们的 getter。

我们用它来访问`doneTodos` getter，用`mapGetters`来显示，使它在我们的组件中可访问。

然后我们应该看到将`done`设置为`true`的 todos，并在我们的组件中显示`1 done`。

# 结论

我们可以用 Vuex 4 创建一个简单的 Vue 3 应用程序，包含一个简单的存储和 getters。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**