# Vue 组合 API 简介

> 原文：<https://javascript.plainenglish.io/introduction-to-the-vue-composition-api-1485d301c100?source=collection_archive---------3----------------------->

![](img/3c302b6419ee8dafdf18ec927b9ac6f8.png)

Photo by [Nazar Yakymenko](https://unsplash.com/@1stpersonviews?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看 Vue 2.x 的 Vue Composition API 插件，以更接近 Vue 3.x 的方式创建我们的组件。

# Vue 组合 API

Vue 组合 API 允许我们将可重用的代码移动到组合函数中，任何组件都可以使用`setup`组件选项。

有了它，我们不必担心 mixin 成员和 component 成员之间的名称冲突，因为所有成员都封装在它们自己的函数中，我们可以用新名称导入它们。

此外，我们不必重新创建组件实例来重用不同组件的逻辑。

Composition API 是在大型项目中构建的，其中有大量可重用的代码。

除了上面列出的好处，我们还有更好的类型推断。它的可读性也更好，因为我们可以将所有代码追溯到它们的组合函数，其中代码是在`setup`方法中声明的。

Vetur VS Code 扩展有助于使用 VS 代码开发 Vue 应用程序，当它与 Composition API 一起使用时，提供类型推断。

# 怎么用？

对于 Vue 2.x 项目，我们可以通过为 Vue CLI 项目安装 Vue Composition API 包来使用它，如下所示:

```
npm install @vue/composition-api
```

它也适用于不使用 Vue CLI 作为库的项目，我们可以通过脚本标记添加库，如下所示:

```
<script src="https://unpkg.com/@vue/composition-api/dist/vue-composition-api.umd.js"></script>
```

其余步骤假设我们正在构建一个 Vue CLI 项目。

一旦我们安装了它，我们可以通过首先在`main.js`中注册如下插件来使用它:

```
import Vue from "vue";
import App from "./App.vue";
import VueCompositionApi from "@vue/composition-api";Vue.use(VueCompositionApi);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

接下来，我们将实际使用 Vue Composition API 插件来构建我们的组件。

首先，我们在`components`文件夹中创建一个名为`Search.vue`的文件，如下:

```
<template>
  <div class="hello">
    <form @submit.prevent="handleSubmit">
      <label>{{label}}</label>
      <input type="text" v-model="state.name">
      <input type="submit" value="search">
    </form>
  </div>
</template><script>
import { reactive } from "@vue/composition-api";export default {
  name: "Search",
  props: {
    label: String
  },
  setup({ label }, { emit }) {
    const state = reactive({
      name: ""
    }); return {
      handleSubmit(event) {
        emit("search", state.name);
      },
      state
    };
  }
};
</script>
```

在上面的代码中，我们创建了一个组件，它采用一个名为`label`的道具，这是一个字符串。

然后我们可以通过从`setup`方法的第一个参数中获取道具的值，而不是用原来的 Vue API 获取道具作为`this`的属性。

另外，`emit`方法是从作为第二个参数传递给`setup`的对象中获取的，而不是从`this`的属性中获取的。

为了保存状态，我们从组合 API 包中调用`reactive`函数，向它传递一个带有状态属性的对象。

`state`和`reactive`相当于我们用常规 Vue API 在`data`方法中返回的对象。

常量`state`作为对象的属性返回，这样我们可以在模板中引用状态作为属性`state`。

为了用 Vue 组合 API 添加方法，我们将它们添加到我们返回的对象中，而不是像没有 Vue 组合 API 时那样作为`methods`属性的一个属性。

我们用`handleSubmit`方法做到了。

在我们的模板中，我们将`v-model`绑定到`state.name`，而不仅仅是`name`。但是我们引用了`handleSubmit`方法，就像我们以前引用方法一样。

当我们在输入中键入一些内容并点击搜索时，就会发出`search`事件。

接下来，在`App.vue`中，我们编写以下代码:

```
<template>
  <div id="app">
    <Search label="name" @search="search"/>
    <div>{{state.data.name}}</div>
  </div>
</template><script>
import Search from "./components/Search";
import { reactive } from "@vue/composition-api";export default {
  name: "App",
  components: {
    Search
  },
  setup() {
    const state = reactive({
      data: {}
    }); return {
      state,
      async search(ev) {
        const res = await fetch(`https://api.agify.io/?name=${ev}`);
        state.data = await res.json();
      }
    };
  }
};
</script>
```

在上面的代码中，我们有类似于`Search.vue`的结构。我们在这个文件中没有的是监听事件。

我们有`search`方法，它监听从`Search.vue`发出的`search`事件。在`search`方法中，我们获取一些数据，将其分配给`state`，并显示在模板上。

此外，我们将检索到的数据分配给了`state.data`。

我们可以使用 Vue Composition API 包中的`computed`函数添加计算属性。

例如，我们可以用下面的代码替换`App.vue`来创建一个计算属性并使用它:

```
<template>
  <div id="app">
    <Search label="name" @search="search"/>
    <div>{{state.name}}</div>
  </div>
</template><script>
import Search from "./components/Search";
import { computed, reactive } from "@vue/composition-api";export default {
  name: "App",
  components: {
    Search
  },
  setup() {
    const state = reactive({
      data: {},
      name: computed(() =>
        state.data.name ? `The name is: ${state.data.name}` : ""
      )
    }); return {
      state,
      async search(ev) {
        const res = await fetch(`https://api.agify.io/?name=${ev}`);
        state.data = await res.json();
      }
    };
  }
};
</script>
```

在上面的代码中，我们通过导入`computed`函数来创建一个计算属性，从而对`App.vue`做了一些小小的修改。

为了创建属性，我们添加了:

```
computed(() => state.data.name ? `The name is: ${state.data.name}` : "")
```

然后在模板中，我们通过编写以下内容来引用它:

```
<div>{{state.name}}</div>
```

现在，当我们键入一些内容并点击搜索时，我们会看到“名称是(您键入的任何内容)”。

![](img/d63713ccbad6b8eac969af66dd5ee8d0.png)

Photo by [Paul Gilmore](https://unsplash.com/@paulgilmore_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vue 组合 API 对于在复杂的应用程序中创建组件非常有用。它的组织减少了名称的冲突，并在我们的组件中提供了更好的类型推断。

我们仍然拥有我们习惯的一切，比如状态、方法、事件、模板和计算属性。只是他们在不同的地方。

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****