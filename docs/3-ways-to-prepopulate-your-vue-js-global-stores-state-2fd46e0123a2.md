# 预填充 Vue.js 全球商店状态的 3 种方法

> 原文：<https://javascript.plainenglish.io/3-ways-to-prepopulate-your-vue-js-global-stores-state-2fd46e0123a2?source=collection_archive---------5----------------------->

![](img/dcad033c7c1157f76558b6c447844ac0.png)

当您构建出 [Vue.js](https://vuejs.org/) 应用程序，并且它们开始达到一定的规模时，您可能会遇到全局状态管理的需求。方便的是，核心开发团队为 Vue.js 应用程序提供了事实上的状态管理库 [Vuex](https://vuex.vuejs.org/) 。

入门非常简单，我假设您已经熟悉了 Vuex 的实现。这个帖子毕竟不是关于入门的。如果你需要的话，我建议你查看一下[文档](https://vuex.vuejs.org/)。

Vuex 使管理全局数据存储变得更加简单，对于下面的例子，我们假设我们有一个看起来像这样的存储:

```
import Vue from 'vue'
import Vuex from 'vuex'Vue.use(Vuex)const store = new Vuex.Store({
  state: {
    user: null
  },
  mutations: {
    setUser (state, user) {
      state.user = user
    }
  },
})
```

商店的状态以一个空的`user`对象和一个可以更新状态的`setUser`变异开始。然后，在我们的应用程序中，我们可能希望显示用户的详细信息:

```
<template>
  <div>
    <p v-if="user">Hi {{ user.name }}, welcome back!</p>
    <p v-else>You should probably log in.</p>
  </div>
</template><script>
export default {
  computed {
    user() {
      return this.$store.state.user
    }
  }
}
</script>
```

因此，当应用程序加载时，如果用户已经登录，它会向用户显示一条欢迎消息。否则，它会告诉他们需要登录。我知道这是一个微不足道的例子，但是希望您遇到过类似的情况。

如果你像我一样，问题来了:

> *如何在应用加载前将数据添加到我的商店？*

嗯，有几个选择。

# 设置初始状态

预填充全局存储最简单的方法是在创建存储时设置初始状态:

```
import Vue from 'vue'
import Vuex from 'vuex'Vue.use(Vuex)const store = new Vuex.Store({
  state: {
    user: { name: "Austin" }
  },
  mutations: {
    setUser (user) {
      state.user = user
    }
  }
})
```

显然，这只有在您提前知道用户的详细信息时才有效。当我们构建应用程序时，我们可能不知道用户的名字，但是还有另一个选择。

然而，我们可以利用`[localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)`来保存用户信息的副本。当他们登录时，您在`localStorage`中设置详细信息，当他们登录我们的时，您从`localStorage`中删除详细信息。

当应用程序加载时，您可以从`localStorage`获取用户详细信息并进入初始状态:

```
import Vue from 'vue'
import Vuex from 'vuex'Vue.use(Vuex)const store = new Vuex.Store({
  state: {
    user: localStorage.get('user')
  },
  mutations: {
    setUser (user) {
      state.user = user
    }
  }
})
```

如果您正在处理的数据不需要非常严格的安全限制，那么这种方法非常有效。我推荐使用`[vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate)`库来帮助实现自动化。

请记住，您不应该在`localStorage`中存储非常敏感的数据，如身份验证令牌，因为它可能成为 XSS 攻击的目标[。因此，我们的例子对用户名有效，但对 auth token 之类的东西无效。这些应该只存储在内存中(仍然可以是 Vuex，只是不持久)。](https://codeburst.io/web-storage-and-xss-attacks-4f83b0d08725)

# 当应用装载时请求数据

现在让我们说，无论出于什么原因，我们不想在`localStorage`中存储数据。我们的下一个选择可能是让我们的初始状态为空，并允许我们的应用程序挂载。一旦安装了应用程序，我们可以向服务器发出一些 HTTP 请求来获取数据，然后更新全局状态:

```
<template>
  <div>
    <p v-if="user">Hi {{ user.name }}, welcome back!</p>
    <p v-else>You should probably log in.</p>
  </div>
</template><script>
export default {
  computed {
    user() {
      return this.$store.state.user
    }
  },
  async mounted() {
    const user = await getUser() // Assume getUser returns a user object with a name property
    this.$store.commit('setUser', user)
  }
}
</script>
```

这很好，但现在我们有一个奇怪的用户体验。应用程序将加载并发送请求，但是当用户等待请求返回时，他们会看到“您可能需要登录”当请求返回时，假设他们有一个登录的会话，该消息很快变成“嗨`{{ user.name }}`，欢迎回来！”。这种闪光看起来很滑稽。

要修复这种闪烁，我们可以在请求发出时简单地显示一个加载元素:

```
<template>
  <div>
    <p v-if="loading">Loading...</p>
    <p v-else-if="user">Hi {{ user.name }}, welcome back!</p>
    <p v-else>You should probably log in.</p>
  </div>
</template><script>
export default {
  data: () => ({
    loading: false
  }),
  computed {
    user() {
      return this.$store.state.user
    }
  },
  async mounted() {
    this.loading = true
    const user = await fetch('/user').then(r => r.json()) // Assume getUser returns a user object with a name property
    this.$store.commit('setUser', user)
    this.loading = false
  }
}
</script>
```

请记住，这是一个非常简单的例子。在您的系统中，您可能有一个用于加载动画的专用组件，并且您可能有一个`[<router-view>](https://router.vuejs.org/api/#router-view)`组件来代替这里的用户消息。您也可以选择从 Vuex 操作发出 HTTP 请求。这个概念仍然适用。

# 在应用程序加载前请求数据

我要看的最后一个例子是发出与上一个类似的 HTTP 请求，但是等待请求返回，并在应用程序有机会加载之前更新存储库。

如果我们记住 Vuex 存储只是一个具有一些属性和方法的对象，我们可以像对待任何其他 JavaScript 对象一样对待它。

我们可以将我们的存储导入到我们的`main.js`文件中(或者您的应用程序的入口点),并在挂载应用程序之前调用我们的 HTTP 请求:

```
import Vue from "vue"
import store from "./store"
import App from "./App.vue"fetch('/user')
  .then(r => r.json())
  .then((user) => {
    store.commit('setUser', user)
    new Vue({
      store,
      render: (h) => h(App),
    }).$mount("#app")
  })
  .catch((error) => {
    // Don't forget to handle this
  })
```

这种方法的好处是，在应用程序加载之前，可以将需要从 API 获取的数据预加载到全局存储中。这是避免前面提到的 janky 跳转或管理一些加载逻辑问题的一种便捷方式。

**然而……**

这里有一个重要的警告。的确，当 HTTP 请求挂起时，您不必担心显示加载微调器，但同时，您的应用程序中没有显示任何内容。如果你的应用是一个单页应用，那么你的用户可能会一直盯着一个空白的页面，直到请求返回。

因此，您并没有真正解决延迟问题，只是决定在等待数据时显示什么样的 UI 体验。

# 结束语

对于哪种方法是最好的，我没有任何硬性规定。实际上，您可以使用这三种方法，这取决于您获取的数据以及您的应用程序的需求。

我还应该提到，虽然我的例子是发出`fetch`请求，然后使用 Vuex 变异直接提交到存储。您可以很容易地使用 Vuex 动作来实现`fetch`。您也可以将这些相同的原则应用于任何其他状态管理工具，比如 Vue.observable。

这就是现在，如果你有任何意见或问题，请让我知道。[推特](https://twitter.com/Stegosource)是联系我的好地方，你可以[注册时事通讯](https://3bb5fb5a.sibforms.com/serve/MUIEAAOwgrWtf43Lfv80ES_hibAhazPDEy4w9IxRIda1b8g1GNnmHYkDfvIKG-Ox35EtWkJfMyCMBTQ3nG2msGhc3WnHa7XKfkgBzYdL3ASbIEckbn47QtJDIvpOskWQuRIXYI-7dVuM5F25yKdcJch7VN8aAbrpEn8_PMXWpqENTJ6r9bOZgHj6vnAQwHDsdwXDOZIonAP3x3vx)获取更多类似的更新。

*原载于*[*https://austingil.com*](https://austingil.com/3-ways-prepopulate-vue-js-global-stores-state/)