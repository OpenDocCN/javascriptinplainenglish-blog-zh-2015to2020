# Vue.js 混合-全局混合和自定义合并策略

> 原文：<https://javascript.plainenglish.io/vue-js-mixins-global-mixins-and-custom-merge-strategies-187f7dac6a8f?source=collection_archive---------5----------------------->

![](img/d13fa26f25c2395783a1dcaf90400527.png)

Photo by [Kym Ellis](https://unsplash.com/@kymellis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究全局混合和将混合合并到组件中的定制合并策略。

# 全球混合

我们可以创造一种适用于全球的混合。这些混合将影响随后创建的每个 Vue 实例。因此，我们在使用它们时应该小心。

我们可以用它来注入适用于所有组件的定制处理逻辑。

例如，我们可以如下定义全局混合:

```
Vue.mixin({
  created() {
    const foo = this.$options.foo;
    console.log(foo);
  }
});new Vue({
  el: "#app",
  foo: "foo"
});
```

然后我们会看到`foo`被记录，因为`foo`选项是从`created`钩子中的`this.$options.foo`中检索的。

当`created`钩子被调用时，我们会看到`console.log`的输出，因为它被调用了。

上面的例子说明了应该如何使用它。应该很少使用全局混合，因为我们想最小化由全局混合的冲突产生的错误的机会。

# 自定义选项合并策略

我们可以调整将 mixin 代码合并到组件中的策略。

为此，我们给`Vue.config.optionMergeStrategies`分配一个函数。

例如，我们可以这样写:

```
Vue.config.optionMergeStrategies.myOption = (toVal, fromVal) => {
  return {
    ...toVal,
    ...fromVal
  };
};Vue.mixin({
  myOption: { foo: 1 }
});const vm = Vue.extend({
  myOption: { foo: 2 }
});console.dir(vm.options.myOption);
```

我们看到`toVal`是来自 mixin 的`{ foo: 1 }`，而`fromVal`是来自我们用`Vue.extend`创建的组件的`{ foo: 2 }`。

因此，我们通过让组件的`myOption`优先，将 mixin 中的`myOption`合并到组件中。

这意味着`vm.options.myOption`具有值`{ foo: 2 }`。

我们可以用`new Vue`写同样的东西如下:

```
Vue.config.optionMergeStrategies.myOption = (toVal, fromVal) => {
  return {
    ...toVal,
    ...fromVal
  };
};const mixin = {
  myOption: { foo: 1 }
};const vm = new Vue({
  el: "#app",
  myOption: { foo: 2 },
  mixins: [mixin]
});console.dir(vm.$options.myOption);
```

我们看到`vm.options.myOption`再次具有值`{ foo: 2 }`。

我们也可以将它们翻转过来，这意味着如果它们有相同的名称，mixin 的选项优先于组件的选项:

```
Vue.config.optionMergeStrategies.myOption = (toVal, fromVal) => {
  return {
    ...fromVal,
    ...toVal
  };
};const mixin = {
  myOption: { foo: 1 }
};const vm = new Vue({
  el: "#app",
  myOption: { foo: 2 },
  mixins: [mixin]
});console.dir(vm.$options.myOption);
```

然后我们从`console.log`得到`{ foo: 1 }`，这意味着`mixin`的`myOption` 优先于组件的`myOption`。

我们可以通过使用`Vue.config.optionMergeStrategies`得到各种合并选项的策略。

常见的选项项目有一些策略，如生命周期挂钩方法、`watch`、`computed`、`methods`、`directives`等。

几乎所有我们可以放入 Vue 的选项，不是我们自己的自定义选项，都在`Vue.config.optionMergeStrategies`对象中。

我们可以使用这些预设的策略来为自己的选项设置合并策略。

例如，如果我们想要合并`myOption`的`methods`策略，我们可以写如下:

```
const strategies = Vue.config.optionMergeStrategies;
strategies.myOption = strategies.methods;
```

那么合并`myOption`的合并策略将与我们用于方法的策略相同，也就是说，如果组件的方法与 mixin 的方法同名，那么它们将优先于 mixin 的方法。

![](img/75b7c10bb2999910a7e05efc2405abe9.png)

Photo by [Matt Heaton](https://unsplash.com/@mattisrad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以定义全局 mixin 来将选项从 mixin 合并到所有组件。

这不应该经常使用，因为它会产生难以跟踪的冲突。

它应该只用于我们自己自定义的选项。

我们还可以创建自己的 mixin 合并策略，方法是给`Vue.config.optionMergeStrategies.optionName`添加一个属性，并设置一个函数，分别从 mixin 和 component 获取选项对象，然后返回合并后的对象。

最后，我们可以通过使用`Vue.config.optionMergeStrategies`对象来访问完整的策略列表。

【JavaScript 用简单英语写的一句话:我们总是对帮助推广优质内容感兴趣。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的 Medium 用户名给我们发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)，我们会把你添加为作者。