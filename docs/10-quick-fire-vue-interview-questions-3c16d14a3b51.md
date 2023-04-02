# 10 个速战速决的面试问题

> 原文：<https://javascript.plainenglish.io/10-quick-fire-vue-interview-questions-3c16d14a3b51?source=collection_archive---------3----------------------->

## 你应该准备好 Vue.js 上的面试问题

![](img/40f21ef25f83d3eea1e12f6d9bbcde44.png)

[Edinburgh Napier University](https://www.napier.ac.uk/about-us/news/vetdogs)

如果你正在准备 Vue.js 面试，那么这是你应该准备回答的 10 个快速提问。

## 1.Vue.js 是什么？

Vue.js 是用于构建用户界面的前端 JavaScript 框架，侧重于单页应用程序。它提倡“高度解耦”,这使得开发者创建用户界面和快速原型变得非常容易。

## 2.Vue.js 有什么好处？

Vue.js 是轻量级的，因此性能很高。它对开发者友好，因此容易学习。它非常灵活，并且有很好的工具。

## 3.什么是 Vue 实例？

Vue 实例，在 Vue 应用程序中通常被称为`vm`，是 Vue 遵循的 MVVM 模式的视图模型。Vue 实例是 Vue 应用程序的根。

```
new Vue({
  render: h => h(App),
}).$mount(‘#app’)
```

一个新的 Vue 实例被创建并挂载到一个包含 id `#app`的 HTML 元素中。

## 4.v-if 和 v-show 有什么区别？

如果表达式的计算结果为`false`，那么`v-if`不会呈现 DOM 中的元素。在`v-show`的情况下，无论如何都会呈现 DOM 中的元素，但是如果`false`的话就会隐藏。`v-if`支持`v-else`和`v-else-if`，可以在`<template>`元素内部使用，`v-show`不支持。

## 5.什么是 Vue 组件？

Vue 组件是有名称的可重用的 Vue 实例。它们支持与根 Vue 实例相同的属性，如数据、方法、计算、观察、混合，以及生命周期方法(在组件中的编写方式略有不同)。下面是一个 Vue 组件的示例。

```
<template>
  <div>
    <p>{{ name }}</p>
  </div>
</template><script>
export default {
  name: ‘10 Quick-Fire Vue Interview Questions’
}
</script><style scoped>
p {
  padding: 10px;
  font-size: 30px;
}
</style>
```

## 6.在本地和全局注册一个组件有什么区别？

全局注册一个组件允许它们在整个应用程序中被重用，甚至在其他组件中。本地组件只能在注册它的组件中使用。

您可以使用`Vue.component(name, componentData)`全局注册一个组件，并通过将它添加到您想要添加的组件的`components`属性中来本地注册一个组件。

## 7.什么是道具？

属性是可以注册到组件的自定义属性。当从另一个组件或根 Vue 实例传递时，属性成为您传递给它的组件的属性。您可以指定多个属性并定义它们的类型。

```
Vue.component('myComponent', {
  props: ['name'],
  // OR
  // props: { name: String },
  template: '<div>Hello {{ name }}</div>'
})<my-component name="Giuseppe Campanelli"></my-component>
```

## 8.如何实现双向绑定？

您可以通过在 input 元素上指定`v-model`属性来实现双向绑定。这将确保随着元素值的改变，Vue 数据属性也会改变。如果数据属性发生变化，输入字段中的值也会发生变化。

```
<input type="text" v-model="firstName">
<p>Hello {{ firstName }}</p>
```

在本例中，如果通过 input 或 method 更改了`firstName`,这种更改将反映在两者上。

## 9.什么是混合蛋白？

Mixins 是一种灵活的方式，可以在组件之间分配功能。mixin 可以包含组件中的任何选项。当一个组件引用一个或多个 mixin 时，mixin 的选项将被“混合”到组件中。

在数据冲突的情况下，组件数据属性将优先。方法、组件和指令也是如此。至于同名的生命周期钩子，两者都将合并到一个数组中，在这个数组中，mixin 钩子将在组件钩子之前被调用。

```
var mixin = {
  data: function () {
    return {
      message: 'hello from mixin'
    }
  },
  created: function () {
    console.log('mixin hook called ')
  }
}new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'hello from component'
    }
  },
  created: function () {
    console.log('component hook called')
  }
})
```

在这个例子中，组件的消息数据属性将优先于 mixin 中的属性使用。在`created`生命周期挂钩的情况下，首先调用 mixin 中的那个，然后调用组件中的那个。

*注意:全局应用 mixin 会影响每个 Vue 实例。*

## 10.什么是 Vue 路由器？

Vue 路由器是 Vue.js 的官方路由器，用于创建不同路由的页面。支持嵌套路由、视图映射，高度可配置；路由参数、查询、通配符。

## 结论

希望这些问题已经为你即将到来的 Vue 面试做好了准备。请继续关注第 2 部分！