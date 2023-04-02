# Vuex 4 —模块

> 原文：<https://javascript.plainenglish.io/vuex-4-modules-f946fc869155?source=collection_archive---------5----------------------->

![](img/ee3317ec76b6f4da046046b99a068bc2.png)

Photo by [Vincentiu Solomon](https://unsplash.com/@vincentiu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Vuex 4 处于测试阶段，可能会有变化。**

Vuex 是一个流行的 Vue 状态管理库。

Vuex 4 是专为 Vue 3 设计的版本。

在本文中，我们将了解如何将 Vuex 4 与 Vue 3 配合使用。

# 模块

我们可以将我们的 Vuex 4 存储划分成模块，这样我们就可以将我们的状态划分成多个更小的部分。

每个模块都有自己的状态、突变、动作和 getters。

# 模块本地状态

每个模块都有自己的本地状态。我们可以通过编写以下代码将模块的状态映射到计算的属性中，并将变化映射到方法中:

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
      <p>{{doubleCount}}</p>
    </div>
    <script>
      const moduleA = {
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count * 2;
          }
        }
      }; const store = new Vuex.Store({
        modules: {
          moduleA
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["increment"])
        },
        computed: {
          ...Vuex.mapGetters(["doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了带有`modules`属性的存储，以便在存储中包含我们的模块。

然后在我们的 Vue 实例中，我们用`mapMutations`和`mapGetters`方法将突变映射到方法，用 getters 映射到计算的属性。

我们可以对动作和状态做同样的事情。

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
      <button @click="increment">increment</button>
      <p>{{moduleA.count}}</p>
    </div>
    <script>
      const moduleA = {
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        actions: {
          increment({ commit }) {
            commit("increment");
          }
        }
      }; const store = new Vuex.Store({
        modules: {
          moduleA
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["increment"])
        },
        computed: {
          ...Vuex.mapState({ moduleA: (state) => state.moduleA })
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用一组动作名调用了`mapActions`方法。

用一个对象调用`mapState`方法，该对象带有返回模块状态的方法。

状态在对象中，模块名作为属性名。

# 命名空间

我们可以命名我们的模块。

这样，我们就不会把所有东西都放在一个全局名称空间下。

此外，这让多个模块对相同的突变或动作类型做出反应。

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
      <button @click="this['moduleA/increment']">increment</button>
      <p>{{moduleA.count}}</p>
    </div>
    <script>
      const moduleA = {
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        actions: {
          increment({ commit }) {
            commit("increment");
          }
        }
      }; const store = new Vuex.Store({
        modules: {
          moduleA: {
            namespaced: true,
            ...moduleA
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapActions(["moduleA/increment"])
        },
        computed: {
          ...Vuex.mapState({ moduleA: (state) => state.moduleA })
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

因为我们将`namespaced`属性设置为`true`，所以我们用名称空间名称来映射动作。

我们可以对 getters 和突变做同样的事情。

为此，我们写道:

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
      <button @click="this['moduleA/increment']">increment</button>
      <p>{{this['moduleA/doubleCount']}}</p>
    </div>
    <script>
      const moduleA = {
        state: () => ({
          count: 0
        }),
        mutations: {
          increment(state) {
            state.count++;
          }
        },
        getters: {
          doubleCount(state) {
            return state.count;
          }
        }
      }; const store = new Vuex.Store({
        modules: {
          moduleA: {
            namespaced: true,
            ...moduleA
          }
        }
      });
      const app = Vue.createApp({
        methods: {
          ...Vuex.mapMutations(["moduleA/increment"])
        },
        computed: {
          ...Vuex.mapGetters(["moduleA/doubleCount"])
        }
      });
      app.use(store);
      app.mount("#app");
    </script>
  </body>
</html>
```

我们用名称空间名称调用了`mapMutations`和`mapGetters`，因为我们在模块中将`namespaced`设置为`true`。

# 结论

我们可以创建我们的模块并命名它们，这样我们就可以有多个较小的存储部分来管理它们自己的状态。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**