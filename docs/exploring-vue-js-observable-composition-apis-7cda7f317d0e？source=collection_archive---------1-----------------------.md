# 探索 Vue.js 可观察和组合 API

> 原文：<https://javascript.plainenglish.io/exploring-vue-js-observable-composition-apis-7cda7f317d0e?source=collection_archive---------1----------------------->

![](img/40b37f78f4a97d2843855d838651ee84.png)

虽然到目前为止 Vue.js 生态系统中最流行的状态管理解决方案是 Vuex，但不要忽视其他有趣的解决方案，如 observable 和 composition APIs，这一点很重要。通过写这篇文章，我假设你已经知道如何使用 Vuex 和它是如何工作的？所以我不会进一步解释 Vuex。让我们探索可观察的和组合的 API

# 检查 Vue.observable API

Vue.js 2.6 引入了一些新特性，包括全局[可观察 API](https://vuejs.org/v2/api/#Vue-observable) 。

现在可以在 Vue.js 组件的范围之外创建反应对象了。此外，不使用 Vuex，您也可以创建商店。当您需要共享几个组件的状态时，它将非常适合用于简单的场景。让我们看看如何在项目中实现 vue.observable。

首先，创建`store.js`:

```
import Vue from "vue";
export const store = Vue.observable({
  data: []
});
export const mutations = {
  setData(payload) {
    store.data = payload;
  }
};
```

听起来很容易，接近 Vuex 对不对？
现在你只需要在一个组件中使用它。为了访问状态，就像在 Vuex 中一样，我们将使用计算的属性和突变方法:

```
<template>
    <div id="app">
        <button [@click](http://twitter.com/click)="fetch">Get the data</button><div v-if="!getData.length">Loading the data</div><div v-else="getData.length">
            <div v-for="data in getData" :key="data.id">
                <h5>{{ data.title }}</h5>
                <p>{{ data.description }}</p>
            </div>
        </div>
    </div>
</template><script>
import { store, mutations } from "./store";
export default {
  name: "app",
  computed: {
    getData() {
      return store.data;
    }
  },
  methods: {
    setData: mutations.setData,
    async fetch() {
      const req = await window.fetch("[http://example.com/tasks](http://example.com/tasks)");
      const res = await req.json();
      return this.setData(res);
    }
  }
};</script>
```

就是这样！漂亮又简单，对吧？下面是如何实现 observable。

# 检查合成 API

让我们看看在使用组合 API 的情况下，将会是什么样的组件:

```
<template>
    <div>
        <input type="text" v-model="state.input" placeholder="Add New Task" />
        <input type="submit" [@click](http://twitter.com/click)="addTask()" />
        <ul>
            <li v-for="(item, index) in state.tasks" :key="item">
                {{ item }}
                <button [@click](http://twitter.com/click)="deleteTask(index)">X</button>
            </li>
        </ul>
    </div>
</template><script>
import { reactive } from "[@vue/composition-api](http://twitter.com/vue/composition-api)";
export default {
  setup() {
    const { state, addTask, deleteTask } = useTaskList();
    return {
      state,
      addTask,
      deleteTask
    };
  }
};function useTaskList() {
  let state = reactive({
    input: "",
    tasks: []
  });function addTask() {
    state.tasks.push(state.input);
    state.input = "";
  }function deleteTask(index) {
    state.tasks.splice(index, 1);
  }
  return {
    state,
    addTask,
    deleteTask
  };
}</script>
```

让我们看看这个组件是怎么回事？正如你所看到的，与通常的 Vue 组件不同，这里没有`data`和`methods`选项。代替那些选项，你会看到一些叫做`setup.`的新方法

`setup`方法应该返回一个对象，该对象将包含我们组件中应该可用的任何内容。

一个重要的注意事项是，新的变量 state 指向反应值。Vue3 提供了直接控制暴露哪些活性元素的能力。同样在前面，我们必须在组件的`methods`选项中声明所有函数，然而，在上面的例子中，我们使用了我们创建的`useTaskList` 方法，在该方法中我们对函数进行了分组。它可以在一个单独的文件中，并导入到组件中，以使其更加清晰。

同样需要注意的是，我们需要从模板中访问的所有内容都由*`useTaskList()`和`setup()`方法返回。*

# *结论*

*好了，现在你已经看到了这两个选项的作用。选择哪一个取决于你，取决于你对相关项目的需求。*