# 解决常见的 Vue 问题—列表过滤、全局库、Vuex 等等

> 原文：<https://javascript.plainenglish.io/solving-common-vue-problems-list-filtering-global-libraries-vuex-and-more-73e14f0cd6be?source=collection_archive---------10----------------------->

![](img/6ea72633bc55133ba7382a2e3321158b.png)

Photo by [The Creative Exchange](https://unsplash.com/@thecreative_exchange?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 过滤 Vue 组件中的列表

筛选列表的正确方法是使用计算属性。

这是因为计算的属性在运行`v-for`之前返回值。

例如，我们可以写:

```
computed: {
  filteredItems() {
    return this.items.filter(item => item.type.toLowerCase().includes(this.search.toLowerCase()))
  }
}
```

我们将过滤逻辑放在`filterItems`计算的属性中。

然后，我们可以通过编写以下代码来遍历计算出的属性:

```
<div v-for="item of filteredItems" >
  <p>{{item.name}}</p>
</div>
```

我们只是将`filteredItems`计算的属性作为变量传入模板。

# 在 Vue 组件中进行 HTTP 调用

我们可以使用 Fetch API 或 Axios 等第三方 HTTP 客户端库在 Vue 组件中进行 HTTP 调用。

例如，我们可以写:

```
methods: {
  async getData(){
    const res = await fetch('/some/url')
    const data = await res.json();
    this.data = data;
  }
}
```

`fetch`返回一个承诺，所以我们可以用`async` 和`await` 来表示。

# 从 URL 中删除哈希

我们可以通过使用历史模式从 Vue 路由器生成的 URL 中删除哈希。

要启用它，我们可以写:

```
const router = new VueRouter({
  mode: 'history'
})
```

我们将`mode`设置为`'history'`，这样我们就可以移除散列。

# 听道具变化

我们可以通过使用观察器来监听道具的变化。

为了给道具添加一个监视器，我们可以给`watch`属性添加一个方法。

例如，我们可以写:

```
{
  ...
  props: ['myProp'],
  watch: { 
    myProp(newVal, oldVal) { 
      console.log(newVal, oldVal)
    }
  }
  ...
}
```

我们有`watch`属性和`myProp`属性，如`props`属性的值所示。

我们只是做了一个与 prop 同名的方法，带有一个分别接受新值和旧值的签名。

然后我们获取`newVal`值以获得新值，获取`oldVal`值以获得旧值。

为了在设置了 prop 值时立即开始观察，我们可以添加`immediate`属性并将其设置为`true`。

例如，我们可以写:

```
watch: {
  myProp: {
    immediate: true, 
    handler (val, oldVal) {
      // ...
    }
  }
}
```

# 跨任何组件的通信

我们可以使用`this.$dispatch`方法在任何组件之间进行通信，以分派一个传播到所有组件的事件。

然后我们可以使用`this.$on`来监听由`this.$dispatch`发送的事件。

例如，我们可以在一个组件中编写以下内容:

```
export default {
  ...
  created() {
    this.$dispatch('child-created', this)
  }
  ...
}
```

然后在另一个组件中，我们可以写:

```
export default {
  ...
  created() {
    this.$on('child-created', (child) => {
      console.log(child)
    })
  }
  ...
}
```

我们在`created`钩子中发出事件，这样当组件被创建但 DOM 内容还没有被加载时，事件就会被发出。

在第二个组件中，我们在`created`钩子中添加了事件监听器，这样一旦组件代码被加载，我们就可以监听事件。

`child-created`是事件名称。

`child`参数在`$dispatch`的第二个自变量中有事件数据。

# 在 Vue 应用程序中全局使用 Axios

为了使 Axios 全球可用，我们可以在创建`Vue`实例之前将其设置为`Vue.prototype`的属性。

这样，如果我们创建了`Vue`实例，它将与`Vue`实例一起返回。

例如，我们可以写:

```
import Axios from 'axios'Vue.prototype.$http = Axios;
```

然后在我们的组件中，我们可以通过使用`this.$http`变量来访问它。

例如，如果我们想发出一个 get 请求，我们可以写:

```
this.$http.get('https://example.com')
```

# Vuex 动作 vs 突变

Vuex 变异是同步代码，接受一个名称和一个处理程序。

它用于更新状态。

例如，我们可以写:

```
import Vuex from 'vuex'const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment(state) {
      state.count++
    }
  }
})
```

然后当我们调度`'increment'`动作时`state.count++`更新`count`状态。

一个动作就像一个突变，但是它可以调度多个突变。

它也可以是异步的。

例如，我们可以写:

```
incrementBy({ dispatch }, amount) {
  dispatch('INCREMENT', amount)
}
```

`dispatch`功能是调度突变的功能。

![](img/92f6495e338856ac2cae784608f48900.png)

Photo by [Natalie Thornley](https://unsplash.com/@natthornley?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vuex 动作和突变是有区别的。

要过滤项目，我们应该在计算属性中进行。

我们可以把东西放到`Vue.prototype`中，让它们在全球范围内可用。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**