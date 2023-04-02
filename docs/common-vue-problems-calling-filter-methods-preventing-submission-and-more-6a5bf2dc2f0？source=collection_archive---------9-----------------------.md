# 常见的 Vue 问题—调用过滤器方法、阻止提交等等

> 原文：<https://javascript.plainenglish.io/common-vue-problems-calling-filter-methods-preventing-submission-and-more-6a5bf2dc2f0?source=collection_archive---------9----------------------->

![](img/6b824e0e805f6bcf96947b47ec999604.png)

Photo by [Deniz Altindas](https://unsplash.com/@omeganova?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在 Vue 实例中调用过滤器方法

通过使用`this.$options.filters`对象，可以在 Vue 组件中访问过滤器方法。

例如，如果我们的 Vue 应用中有一个`capitalize`过滤器，我们可以写:

```
this.$options.filters.capitalize(this.word);
```

称之为。

同样，我们可以使用`Vue.filter`方法来做同样的事情。

我们可以写:

```
const text = Vue.filter('capitalize')(this.word);
```

# Vue 配置文件位置

使用 Vue CLI 创建的 Vue app 项目位于`vue.config.js`。

如果它不在根文件夹中，我们可以将其添加到根文件夹中。

此外，我们可以使用`vue config`命令来检查或修改全局 CLI 配置。

# 清除 Vuex 存储中的状态

我们添加了一个突变来清除 Bue 存储中的状态。

例如，我们可以写:

```
const mutations = {
  resetState (state) {
    Object.assign(state, getDefaultState())
  }
}export default {
  state,
  getters: {},
  actions,
  mutations
}
```

我们使用`Object.assign`将默认值合并到现有的状态对象中，这样我们就不会丢失状态观察器。

我们可以为`getDefaultState`定义如下。

例如，我们可以写:

```
const getDefaultState = () => {
  return {
    foo: [],
    bar: 'empty'
  }
}
```

这应该在顶部，这样我们可以在任何地方使用它。

`state`可以初始化为`getDefault`状态，如下所示:

```
const state = getDefaultState()
```

然后我们可以做一个动作来提交`resetState`动作:

```
const actions = {
  resetCartState ({ commit }) {
    commit('resetState')
  },
  // ...
}
```

# v-bind 中的条件:样式

我们可以用`v-bind:style`指令有条件地应用样式。

为此，我们可以写:

```
v-bind:style= "[condition ? { style1 } : { style2 }]"
```

我们有一个根据`condition`的值填充了不同样式对象的数组。

同样，我们可以把它放在数组之外:

```
v-bind:style= "condition ? { style1 } : { style2 }"
```

# 在 Vue 路由器的 beforeEnter 挂钩中访问异步存储数据

我们可以在我们的`beforeEnter`钩子中分派动作。

例如，我们可以写:

```
import store from './vuex/store';{
  path: '/foo',
  component: Foo,
  beforeEnter: (to, from, next) =>
  {
    store.dispatch('start')
    .then(response => {
      // ...
    }, error => {
      // handle error here
    })         
  }
}
```

上面的代码中有我们可能在代码中有的 route 对象。

在`response`中，我们从动作中获取数据。

现在，随着路线的加载，我们得到了我们想要的数据。

# 呈现新行字符

如果我们把新的行字符放在 pre 标签周围，我们就可以呈现它。

例如，我们可以写:

```
<pre>
  text with
  newline
</pre>
```

如果我们想要样式化任何其他元素来显示新的行，我们可以用`white-space: pre`样式来样式化它。

# 限制 v-for 中元素的迭代

为了限制`v-for`中元素的迭代，如果是数组，我们可以使用`slice`方法。

例如，我们可以写:

```
<div v-for="value in array.slice(0, 5)"></div>
```

然后我们在`array`中渲染前 5 个数组条目，因为我们将 0 和 5 传递给了`slice`。

要对对象进行同样的操作，我们必须访问索引并检查。

例如，我们写道:

```
<div v-for="(value, index) in object" v-if="index < 5"></div>
```

然后我们用从 0 到 4 的`index`渲染关键帧。

# 从文本输入中按 Enter 键时阻止提交

为了防止在文本输入中按 enter 键时提交，我们可以使用`enter`和`prevent`修饰符。

例如，我们可以写:

```
<input type='text' v-on:keydown.enter.prevent='add' />
```

当我们按下回车键时，我们会阻止提交，因为我们有`prevent`修饰符和`enter`修饰符。

我们必须按下键停止提交，因为当我们按下回车键时提交已经完成。

![](img/cd60ba1520f64edf83cadf82daad5b1a.png)

Photo by [Natalie Rhea Riggs](https://unsplash.com/@natalie_rhea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过使用数组实例`slice`方法或检查索引来限制`v-for`的迭代器。

同样，我们可以用`this.$options.filter`对象调用`filter`方法。

我们可以做一个动作和突变来清除商店的状态。

## 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容！**