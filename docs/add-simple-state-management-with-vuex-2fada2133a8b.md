# 使用 Vuex 添加简单的状态管理

> 原文：<https://javascript.plainenglish.io/add-simple-state-management-with-vuex-2fada2133a8b?source=collection_archive---------10----------------------->

![](img/141040be48f90fb8bb05e292bcd5b37b.png)

Photo by [Heidi Fin](https://unsplash.com/@heidifin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

借助 Vuex，我们可以将 Vue 应用的状态存储在一个中心位置。

在本文中，我们将了解如何将 Vuex 添加到我们的应用程序中，并添加一个简单的商店。

# Vuex 是什么？

Vuex 用于在一个中心位置存储多个组件所需的状态。

它允许我们获取和设置共享状态，并将对共享状态的任何更改自动传播到所有组件。

![](img/a29b38b6ca5b5f2612a0ffc79c2cd98a.png)

Courtesy of [https://vuex.vuejs.org/](https://vuex.vuejs.org/)

在上面的工作流程图中，我们可以看到，当我们从后端 API 获得一些东西时，我们的代码会提交 Vuex 突变。

该突变将使用后端 API 数据更新状态，并且状态将在我们的 Vue 组件中更新。

我们还可以从 Vue 组件调度突变来改变 Vuex 存储状态。这种改变将传播到所有可以访问存储的组件。

# 入门指南

我们可以在 HTML 代码中包含带有`script`标签的 Vuex:

```
<script src="[https://unpkg.com/vuex](https://unpkg.com/vuex)"></script>
```

然后我们可以使用`Vuex.Store`创建一个简单的商店，如下所示:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});
```

上面的代码是一个存储州名`count`的商店。

然后我们可以通过运行以下命令提交`increase`变异:

```
store.commit("increase");
```

然后，我们可以通过运行以下命令获得突变完成后的状态:

```
console.log(store.state.count);
```

`store.state.count`是 Vuex 商店的`count`状态。然后我们应该看到 1 记录。

# 将 Vuex 状态放入 Vue 组件

我们可以通过添加一个计算属性将状态保存到我们的存储中。

因此，要将状态添加到我们的存储中，我们可以编写以下代码:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});new Vue({
  el: "#app",
  computed: {
    count() {
      return store.state.count;
    }
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
      <p>{{count}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

然后我们应该看到显示 0，因为`count`在`store`中最初是 0。

然后每当`store.state.count`更新时，计算的属性将被更新，视图将使用新值更新。

将商店注入所有子组件的一个更方便的方法是在根组件中添加`store`,如下所示:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});new Vue({
  el: "#app",
  store
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
      <p>{{$store.state.count}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

那么它将对所有子组件可用，我们不必担心为每个值添加计算属性。

它与子组件一起工作，没有太大的变化:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});Vue.component("counter", {
  template: `<div>{{ count }}</div>`,
  computed: {
    count() {
      return this.$store.state.count;
    }
  }
});new Vue({
  el: "#app",
  store
});
```

`this.$store`可用于`counter`组件，只需将其包含在根 Vue 组件中。

![](img/a330abf8e527f6d0c4ab972510cbd253.png)

Photo by [Clark Street Mercantile](https://unsplash.com/@mercantile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# mapState 助手

为了避免为存储中的每个状态添加新的计算属性，我们可以使用`mapState`助手来添加它。例如，我们可以编写以下代码来实现这一点:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});Vue.component("counter", {
  data() {
    return {
      localCount: 1
    };
  },
  template: `
    <div>
      <div>{{ count }}</div>
      <div>{{ countAlias }}</div>
      <div>{{ countPlusLocal }}</div>
    </div>
  `,
  computed: Vuex.mapState({
    count: state => state.count,
    countAlias: "count",
    countPlusLocal(state) {
      return state.count + this.localCount;
    }
  })
});new Vue({
  el: "#app",
  store
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
      <counter></counter>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

然后我们看到:

```
001
```

因为我们对`mapState`的称呼如下:

```
computed: Vuex.mapState({
  count: state => state.count,
  countAlias: "count",
  countPlusLocal(state) {
    return state.count + this.localCount;
  }
})
```

我们有:

```
count: state => state.count,
```

它从存储中获取`count`状态并返回它。`count`是 0，所以我们得到 0。

然后我们有:

```
countAlias: "count"
```

这是以下内容的简写:

```
count: state => state.count
```

最后，我们有:

```
countPlusLocal(state) {
  return state.count + this.localCount;
}
```

它将商店中的`state.count`加到`this.localCount`，我们将其设为 1。

# 对象扩展算子

我们可以通过对`mapState`应用扩展运算符，将本地计算的属性与`mapState`结合起来，如下所示:

`index.js`:

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase(state) {
      state.count++;
    }
  }
});Vue.component("counter", {
  data() {
    return {
      localCount: 1
    };
  },
  template: `
    <div>
      <div>{{ count }}</div>
      <div>{{ foo }}</div>      
    </div>
  `,
  computed: {
    foo() {
      return 2;
    },
    ...Vuex.mapState({
      count: "count"
    })
  }
});new Vue({
  el: "#app",
  store
});
```

然后我们得到:

```
02
```

显示，因为`foo`总是返回 2。

# 组件仍然可以有本地状态

组件仍然可以有自己的本地状态，所以我们不必把所有东西都放在 Vuex 存储中。

# 结论

我们可以在我们的应用程序中添加一个 Vuex store 来存储由多个组件共享的应用程序的状态。

为了使状态变得容易，我们可以将商店包含在根 Vue 组件中。

然后我们调用`mapState`助手来获得我们想要的任何组件的状态。

我们也可以用`computed`对象中的 spread 操作符将它与本地状态结合起来。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至:[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****