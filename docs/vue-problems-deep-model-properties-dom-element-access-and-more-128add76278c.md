# Vue 问题-深层模型属性、DOM 元素访问等

> 原文：<https://javascript.plainenglish.io/vue-problems-deep-model-properties-dom-element-access-and-more-128add76278c?source=collection_archive---------10----------------------->

![](img/31f20ca8b4da80202ba45b97ca4dd01b.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 使开发前端应用程序变得容易。然而，我们仍然有可能遇到问题。

在本文中，我们将查看一些常见问题，并了解如何解决它们。

# 具有深度属性的 Vue.js v-model

我们可以通过首先初始化将`v-model`绑定到深度嵌套的属性。

例如，我们可以写:

```
data(){
  return {
    filters: { name: null },
  }
}
```

在我们的组件中初始化数据。

然后我们可以写道:

```
<input v-model="filters.name">
```

# 在项目中使用全局变量

如果我们有一个全局变量，那么我们必须在组件代码中引用它，然后才能在模板中使用它。

例如，我们可以写:

```
<script>
window.globalVar = 'global';new Vue({
  el: "#app",
  data: {
  },
  computed: {
    globalVar() {
      return window.globalVar;
    },
 },
  methods: {
    ...
  }
})
</script>
```

我们添加了一个计算的`globalVar`属性，通过返回它来引用`window.globalVar`。

现在我们可以通过编写以下内容在模板中使用`globalVar`:

```
<div id="app">
  <p>
    {{globalVar}}
  </p>
</div>
```

我们可以用任何全局变量来实现。

# 使用 Vuex 将数据从父母发送给孩子

我们可以通过首先在父节点中提交一个动作，使用 Vuex 将数据从父节点发送到子节点。

然后，我们总是可以使用 getters 获取子组件中的最新值。

例如，我们可以写:

```
...const actions = {
  updateCategory ({commit}, payload) {
    commit('updateCategory', payload)
  }
}const mutations = {
  updateCategory(state, payload) { 
    state.category = payload
  }
}
export default {
    state,
    getters,
    actions,
    mutations
}
```

创建一个带有`updateCategory`突变的 Vuex 商店。

然后我们可以在我们的组件代码中使用`mapActions`添加动作:

```
methods: {
  ...mapActions(['updateCategory']),
}
```

然后我们可以在代码中调用`this.updateCategory('newCategory')`。

在子组件中，我们调用`mapGetters`将吸气剂映射到子组件中:

```
computed: {
  ...mapGetters(['getCategory'])
},
```

然后我们可以用`this.category`状态得到类别。

# 获取包含组件的元素

通过给我们想要得到的元素分配一个引用，我们可以得到一个没有组件的元素。

然后，我们可以在组件代码中引用该元素。

例如，我们可以写:

```
<div id="app">
  <span ref="someName">hello</span>
</div>
```

然后，我们可以通过编写以下内容来获得 span element 对象:

```
this.$refs.someName
```

在我们的代码中。

如果我们在`v-for`渲染的元素中添加一个引用，那么我们得到一个元素数组。

例如，如果我们写:

```
<div v-for="age in ages">
  <span ref="ageSpan">{{ age }}</span>
</div>
```

然后`this.$refs.ageSpan`将是跨度元素的数组。

# v-bind:样式和背景图像

如果我们想用`v-bind:style`设置背景图像，我们可以写如下:

```
<div v-bind:style="{ backgroundImage: `url(${image})` }"></div>
```

然后我们将`image`变量插入到`url`字符串中。

或者，我们可以在组件代码的图像上调用`require`。

然后我们可以引用组件中的对象作为`style`值。

例如，我们可以写:

```
<template>
  <div :style="cssProps"></div>
</template><script>
  export default {
    data() {
      return {
        cssProps: {
          backgroundImage: `url(${require('@/assets/img.jpg')})`
        }
      }
    }
  }
</script>
```

我们有具有`backgroundImage`属性的`cssProps`对象。

然后我们把它设为`:style`的值。

# 在 Vue 方法中使用 setTimeout

我们可以在 Vue 组件方法中使用`setTimeout`。

例如，我们可以写:

```
setTimeout(() => {
   this.aMethod()
}, 1000);
```

我们在组件代码中称之为`this.aMethod`。

# 使用$emit 传递参数

我们可以用第二个参数调用`$emit`来传递事件数据。

例如，我们可以写:

```
methods: {
  updateName(name) { 
    this.bus.$emit('updateName', name); 
  },
}
```

在一个组件中发出事件。

然后在另一个组件中，我们可以写:

```
created() {
  this.bus.$on('updateName', (name) => alert(name));
}
```

`this.bus`是一个`Vue`实例。

# 在 Vue 组件中使用保活组件

我们可以使用`keep-alive`组件来防止内部组件的状态被重置。

例如，我们可以写:

```
<keep-alive>
  //...
</keep-alive>
```

现在里面的组件即使被卸载也不会被重置。

![](img/bb5307cb8bac2a1952fcfd20460b13a6.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过使用`backgroundImage`属性来添加样式。

然后，我们可以将 URL 插入到`url`字符串中，或者需要图像。

如果我们分配 refs，然后引用 ref，就可以得到元素。

# 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**