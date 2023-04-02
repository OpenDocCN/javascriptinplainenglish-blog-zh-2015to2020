# 向 Vuex 商店添加操作

> 原文：<https://javascript.plainenglish.io/adding-actions-to-a-vuex-store-9d534fb3cac5?source=collection_archive---------5----------------------->

![](img/ceb28b32f6880ef2183eeecd1711af50.png)

Photo by [Robert Bye](https://unsplash.com/@robertbye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

借助 Vuex，我们可以将 Vue 应用的状态存储在一个中心位置。

在本文中，我们将了解如何向我们的 Vuex 商店添加动作来改变商店的状态。

# 什么是行动？

动作类似于突变，但是它们提交突变而不是突变状态。与突变不同，动作也可以提交异步操作。

例如，我们可以添加如下简单操作:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  },
  actions: {
    increase(context) {
      context.commit("increase");
    }
  }
});
```

一个动作接受一个`context`对象，我们可以用它来调用`commit`来提交一个变异。

# 调度操作

我们可以通过呼叫`store.dispatch('increase')`来调度行动。

这比直接提交突变有用得多，因为我们可以用它来运行异步操作。

例如，我们可以在 Vue app 中调度一个操作，如下所示:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state, payload) {
      state.count += payload.amount;
    }
  },
  actions: {
    increaseAsync({ commit }, payload) {
      setTimeout(() => {
        commit("increase", payload);
      }, 1000);
    }
  }
});new Vue({
  el: "#app",
  store,
  methods: {
    ...Vuex.mapActions(["increaseAsync"])
  },
  computed: {
    ...Vuex.mapState(["count"])
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vuex](https://unpkg.com/vuex)"></script>
  </head>
  <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="increaseAsync({amount: 10})">Increase</button>
      <p>{{count}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们用下面的代码实现了动作`increaseAsync`:

```
increaseAsync({ commit }, payload) {
  setTimeout(() => {
    commit("increase", payload);
  }, 1000);
}
```

在 action 方法中，我们用一个`payload`对象来提交`increase`变异，当我们调度 action 时会传递这个对象。

在 Vue 实例中，我们有:

```
methods: {
    ...Vuex.mapActions(["increaseAsync"])
},
computed: {
  ...Vuex.mapState(["count"])
}
```

它通过调用`mapActions`来映射存储中的动作，并通过使用`mapState`从存储中映射它们来添加计算属性，因此我们得到了一个`count`属性，它是从`state.count`作为计算属性计算出来的。

然后在模板中，当我们按下增加按钮 1 秒后更新`state.count`时，我们调用`increaseAsync`。

最后，我们应该看到数字更新了，因为我们从商店映射了状态。

![](img/ab8d004eefc869187fe49eb81e33f694.png)

Photo by [Daniel Monteiro](https://unsplash.com/@danielmonteirox?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 撰写动作

我们可以将回报承诺的行动串连起来。

例如，我们可以创建一个操作来更新 2 个计数，如下所示:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count1: 0,
    count2: 0
  },
  mutations: {
    increase(state, payload) {
      state.count1 += payload.amount;
    },
    decrease(state, payload) {
      state.count2 -= payload.amount;
    }
  },
  actions: {
    async increaseAsync({ commit }, payload) {
      return Promise.resolve(commit("increase", payload));
    },
    async decreaseAsync({ commit }, payload) {
      return Promise.resolve(commit("decrease", payload));
    },
    async updateCounts({ dispatch }, payload) {
      await dispatch("increaseAsync", payload);
      await dispatch("decreaseAsync", payload);
    }
  }
});new Vue({
  el: "#app",
  store,
  methods: {
    ...Vuex.mapActions(["updateCounts"])
  },
  computed: {
    ...Vuex.mapState(["count1", "count2"])
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://unpkg.com/vue/dist/vue.js](https://unpkg.com/vue/dist/vue.js)"></script>
    <script src="[https://unpkg.com/vuex](https://unpkg.com/vuex)"></script>
  </head>
  <body>
    <div id="app">
      <button [@click](http://twitter.com/click)="updateCounts({amount: 10})">Update Counts</button>
      <p>Count 1: {{count1}}</p>
      <p>Count 2: {{count2}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们用`increaseAsync`动作和`payload`让`updateCounts`动作开关调用`dispatch`。然后用`decreaseAsync`动作和`payload`调用`dispatch`。

`increaseAsync`提交`increase`突变，`decreaseAsync`提交`decrease`突变。

由于都有`Promise.resolve`，所以都是异步的。

然后我们用`mapActions`将商店的`updateCounts`动作包含在我们的 Vue 实例中。我们还包括了`count1`和`mapState`的`count2`州。

然后当我们点击更新计数按钮时，我们调用`updateCounts`，然后`count1`和`count2`随着我们点击按钮而更新。`count1`每点击一次应增加 10，而`count2`每点击一次应减少 10。

# 结论

我们可以使用动作来提交一个或多个突变或分派其他动作。

因为突变总是同步的，所以将存储操作组合在一起并运行异步代码很方便。

我们可以使用`mapActions`将它们包含在我们的组件中。

通过调用`dispatch`来分派动作，而通过`commit`方法来提交变异。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****