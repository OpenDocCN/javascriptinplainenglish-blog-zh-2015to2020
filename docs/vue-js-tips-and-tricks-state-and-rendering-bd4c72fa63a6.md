# Vue.js 提示和技巧—状态和渲染

> 原文：<https://javascript.plainenglish.io/vue-js-tips-and-tricks-state-and-rendering-bd4c72fa63a6?source=collection_archive---------9----------------------->

![](img/e9722d0e755402d627136f8121c3e6db.png)

Photo by [Jamie Street](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解一些开发 Vue 应用程序时需要注意的技巧和诀窍，以节省时间并减少产生 bug 的机会。

# 使用 Vuex 进行应用程序范围的状态管理

Vuex 是一个很棒的库，为 Vue.js 带来了应用程序范围的状态管理。这是管理 Vue 应用程序状态的最简单的解决方案。

我们要记住的是使用 getters 获取数据，使用 mutations 设置基本数据。这些状态存储在存储器中。动作用于更复杂的商店数据更新。它用于提交多个变化和异步存储更新。模块让我们把一切分成可管理的部分。

例如，我们可以如下使用它:

`index.js`

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state, payload) {
      state.count += payload;
    }
  },
  getters: {
    count: state => state.count
  }
});Vue.component("app-button", {
  template: "<button @click='increment(2)'>Increment</button>",
  methods: {
    ...Vuex.mapMutations(["increment"])
  }
});
Vue.component("app-counter", {
  template: "<p>{{count}}</p>",
  computed: {
    ...Vuex.mapGetters(["count"])
  }
});new Vue({
  el: "#app",
  store
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex@2.0.0"></script>
  </head>
  <body>
    <div id="app">
      <app-button></app-button>
      <app-counter></app-counter>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，`app-button`和`app-counter`是兄弟姐妹，所以如果没有一些帮助，很难在它们之间传递数据。因此，我们可以使用 Vuex 创建一个存储`count`状态的存储，两个组件都可以访问它。

我们只需将`store`传递到我们的主 Vue 实例中，然后我们可以用`mapMutations`方法映射方法中带有`mapMutations`的 get 突变。

同样，我们调用`mapGetters`方法将 getters 映射到计算的属性。

在`mutations`属性中，我们有`increment`方法，它采用一个`payload`参数来更新`count`状态。

`getters`具有返回`count`状态的`count`。

在`app-button`组件中，我们有从`mapMutations`方法获得的带有`increment`方法的点击处理程序，该方法调用带有`2`的`payload` 的`increment`变异。

那么我们每次点击“然后”按钮时,`state.count`就会更新 2。因此，我们看到按钮更新了屏幕上的`count`。

# 了解 Vue 组件实例如何工作

我们必须知道 Vue 实例是如何工作的，这样我们才不会创建缓慢且有问题的代码。

例如，我们必须知道`data`属性应该有一个函数作为它的值，因为我们不想与其他实例共享数据。

此外，因为如果`data`属性是一个函数，Vue 就不必完全实例化整个组件，所以节省了内存。其他所有东西，如方法、计算属性定义和生命周期钩子，都只创建和存储一次。

我们应该知道我们需要`this`来访问`data`返回的属性，因为我们正在访问组件实例的数据。我们传入`Vue.component`或从单文件组件导出的对象仅用于定义组件。

![](img/9fbbcdb1e38b9a10dac2ad1ff5f61766.png)

Photo by [Elias Castillo](https://unsplash.com/@eli_j?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 正确重新插入 Vue 组件

我们应该正确地重新放置 Vue 组件。最好的方法是在一个组件上设置一个`key`道具，这样当它的内容更新时，Vue 就可以识别出更新的组件并相应地重新编辑它。

例如，当我们使用`v-for`时，我们应该将`key`道具的值设置为一个唯一的值，如下所示:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    people: [
      { id: 1, name: "John" },
      { id: 2, name: "Sarah" },
      { id: 3, name: "Jane" }
    ]
  },
  methods: {
    remove(index) {
      this.people.splice(index, 1);
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vuex@2.0.0"></script>
  </head>
  <body>
    <div id="app">
      <p v-for="(p, index)  in people" :key="p.id">
        {{p.name}} - {{index}} <button @click="remove(index)">Delete</button>
      </p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有`key`道具，它被设置为`p.id`的值。它是唯一的，所以当我们单击“删除”按钮时，它可以用来识别要删除和重新提交的正确组件。

# 结论

Vuex 非常适合应用范围内的状态管理。此外，我们必须了解 Vue 实例是如何工作的，这样我们就不会犯像在组件中为对象设置`data`这样的错误。最后，我们应该将`key`道具添加到使用`v-for`渲染的任何东西中，这样当我们操纵项目时，重新渲染就可以正确完成。

## **简明英语团队备注**

你知道我们有四种出版物吗？给他们一句如下的话来表达爱意: [**JavaScript 简单明了**](https://medium.com/javascript-in-plain-english) 、[T21【AI】简单明了](https://medium.com/ai-in-plain-english) 、[**UX**](https://medium.com/ux-in-plain-english)、[、 **Python 简单明了**、](https://medium.com/python-in-plain-english)、**——谢谢你们，继续学习吧！**

此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！**