# 常见的 Vue 问题—引导、Vuex 路由限制等

> 原文：<https://javascript.plainenglish.io/common-vue-problems-bootstrap-vuex-route-restrictions-and-more-58cffb684312?source=collection_archive---------16----------------------->

![](img/d6a1082c85dad9dc0aa1ebf6b737ece9.png)

Photo by [小谢](https://unsplash.com/@guogete?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在我们的应用中使用引导程序

如果我们只想使用来自 Bootstrap 的 CSS，那么我们可以添加普通的 Bootstrap 包并从中加载 SASS 文件。

要加载 SASS 文件，我们可以使用`sass-loader`和`node-sass`。

要安装它们，我们运行:

```
npm install sass-loader node-sass --save-dev
```

然后，我们在 Webpack 配置文件中添加加载 SASS 文件的路径:

```
...
sassLoader: {
  includePaths: [
    path.resolve(projectRoot, 'node_modules/bootstrap/scss/'),
  ],
},
...
```

现在，我们可以在组件中导入引导 SASS 文件:

```
<style lang="scss">
  @import "bootstrap";
</style>
```

如果我们想使用 Bootstrap 中的 JavaScript 组件，那么我们应该使用 BootstrapVue。

# 使用 SASS 资源加载器

我们可以通过编写以下代码将 SASS 预处理器添加到我们的`vue.config.js`文件中:

```
module.exports = {
  css: {
    loaderOptions: {
      sass: {
        data: `
          @import "@/scss/_variables.scss";
          @import "@/scss/_mixins.scss";
        `
      }
    }
  }
};
```

不需要额外的依赖项，因为它们包含在 Vue CLI 项目中。

# 在 Vue 的另一个组件中调用方法

如果我们不从父组件的子组件中调用方法，我们就不能直接调用另一个组件的方法。

相反，我们必须通过添加一个全局 Vue 实例来传递事件，我们可以用它来发出和监听事件。

例如，我们可以写:

```
window.Event = new Vue();
```

创建一个`Vue`实例。

然后我们可以在上面写下`$emit`:

```
Event.$emit('createImage', item, response)
```

在一个组件中。

然后在另一个组件的`mounted`钩子上我们可以写:

```
mounted() {
  Event.$on('createImage', (item, response) => {
    // ...
  }
}
```

我们监听`createImage`事件，并在回调参数中获取传递给`$emit`的参数。

# 在 Vue 组件中添加 Web 组件

我们可以直接在 Vue 组件中添加 web 组件。

例如，我们可以写:

```
<template id="app">
  <web-component>...</web-component>
</template>
```

鉴于`web-component`是一个 web 组件，我们可以直接包含它。

# v-html 的局限性

`v-html`只能用来渲染普通的 HTML 代码。

我们不能用它来渲染 Vue 组件。

例如，如果我们有一个组件:

```
Vue.component('component', {
  template: '<span>component</span>',
})
```

那么我们不能有这样的代码:

```
new Vue({
  el: '#app',
  data(){
    return {
      comp: '<component></component>'
    }
  }
})...
<div id="app">  
  <span v-html="comp"></span>
</div>
```

这将不起作用，因为`component`是一个组件。

所有数据绑定和 Vue 组件都被忽略。

相反，我们必须呈现普通的 HTML:

```
new Vue({
  el: '#app',
  data(){
    return {
      comp: '<p>foo</p>'
    }
  }
})
...
<div id="app">  
  <span v-html="comp"></span>
</div>
```

这将用`v-html`来呈现，因为我们有一个 p 元素。

# 在多个元素上挂载多个 Vue 实例

如果我们想在多个元素上附加多个`Vue`实例，我们可以写:

```
const vues = document.querySelectorAll(".app");
[...vues].forEach((el, index) => new Vue({el, data: { message: `hello ${index}`}}))
```

我们用类`app`得到所有的元素。

然后我们在`vues`上使用扩展操作符，这样我们就可以在它上面使用`forEach`，

在回调中，我们可以创建新的`Vue`实例。

# Vue 路由器路径中的正则表达式

我们可以在 Vue 路由器路径中使用正则表达式。

例如，我们可以写:

```
{ path: 'item/:id(\\d+)', name: 'itemCard', component: ItemCard }
```

然后我们指定`id`参数必须是数字。

# 绑定到 v-model 的自定义选择组件

如果我们想要创建绑定到`v-model`的定制选择组件，那么我们的组件必须发出`input`事件并接受`value`属性。

`v-model`是这两种事物的简写。

例如:

```
<custom-select v-bind:value="val" v-on:input="val = $event.target.value"></custom-select>
```

可以改写为:

```
<custom-select v-model="val"></custom-select>
```

然后我们可以在`custom-event`中做任何事情来发出`input`事件并获取`value`道具。

![](img/2d6b27d425e281f3d1c628bba5ea65bf.png)

Photo by [Behzad Ghaffarian](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

`v-model`是发出`input`事件并获取`value`属性的简称。

我们可以用 SASS 加载器包添加引导 SASS 文件。

Vue 路由器路由可以使用正则表达式来限制参数值。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**