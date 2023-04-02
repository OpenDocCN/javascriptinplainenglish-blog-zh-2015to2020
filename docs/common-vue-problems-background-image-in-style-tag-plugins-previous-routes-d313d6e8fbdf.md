# 常见的 Vue 问题——样式标签、插件、以前路线中的背景图像

> 原文：<https://javascript.plainenglish.io/common-vue-problems-background-image-in-style-tag-plugins-previous-routes-d313d6e8fbdf?source=collection_archive---------5----------------------->

![](img/2e8c0c29a4c03e63f3c120533cec51a5.png)

Photo by [Pacto Visual](https://unsplash.com/@pactovisual?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在样式标签中添加背景图像

我们可以写:

```
url('~@/assets/image.png').
```

将`assets`文件夹中的图像作为背景图像。

所以我们可以写:

```
background-image: url('~@/assets/image.png');
```

在我们的`style`标签中。

`@`将解析为一个别名。`~`表示解析到一个模块请求。

这样，图像将被视为一个模块。

# 在使用 Vue 路由器中创建 404 组件

一个 404 组件可以用`'*'`路径创建，所以我们可以写:

```
{ path: '*', component: NotFound },
```

在路线数组中添加它。

# 在 Vue 应用中包含全局功能

有几种方法可以包含全局函数。

我们可以将它作为静态属性附加到`Vue`构造函数上。

为此，我们写道:

```
Vue.globalMethod = () => {}
```

我们还可以通过编写以下代码来添加一个全局资产，如指令或过滤器:

```
Vue.directive('my-directive', {})
```

或者:

```
Vue.filter('my-filter', {})
```

我们也可以通过编写以下代码将其添加为实例方法:

```
Vue.prototype.globalMethod = function(){}
```

我们可以通过编写以下代码将所有这些东西放在一个插件中:

```
MyPlugin.install = function (Vue, options) {
  Vue.globalMethod = ... Vue.directive('my-directive', {}) Vue.prototype.$globalMethod = ...
}
```

我们定义了`install`方法，让我们可以将所有这些东西添加到任何使用我们插件的应用程序中。

然后我们可以用`Vue.use`在我们的应用程序中注册插件:

```
Vue.use(MyPlugin)
```

现在它可以在任何地方使用。

我们可以通过编写以下代码来调用静态全局方法:

```
Vue.globalMethod()
```

# 重新安装组件

我们可以通过设置`key`道具来重新安装一个组件。

通过这种方式，`key`值发生了变化，因此我们重新安装了组件。

例如，我们写道:

```
<div id="app">
  <my-component :key="key"></my-component>
  <button @click="key = !key">reload</button>
</div>
```

当我们点击按钮打开和关闭按键时，`my-component`将重新加载。

# 侦听窗口滚动事件

我们甚至可以通过在`created`钩子中添加事件监听器来监听窗口滚动。

此外，当组件被卸载时，我们必须记住删除事件监听器，以便我们自己清理。

例如，我们可以写:

```
export default {
  created () {
    window.addEventListener('scroll', this.handleScroll);
  },
  destroyed () {
    window.removeEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll (event) {
      // ...
    }
  }
}
```

在创建组件时会运行`created`钩子，所以我们可以用`addEventListener`在那里添加事件监听器。

然后当组件被破坏时，我们调用`removeListener`。

# 订购项目和 v-for

为了按照我们想要的顺序呈现数组项目，我们应该创建一个计算属性来对项目进行排序，并用`v-for`来呈现。

例如，我们可以通过编写以下内容来创建它:

```
computed: {
  orderedBooks() {
    return _.orderBy(this.books, 'author')
  }
}
```

然后我们可以用`v-for`而不是`books`来渲染`orderedBooks`数组。

我们写道:

```
<p v-for="book in orderedBooks">{{ book.author }}</p>
```

# 这之间的区别。$set 和 Vue.set

`this.$set`和`Vue.set`都让我们更新一个对象或数组条目，并触发模板中的更新。

`this.$set`仅在组件实例中可用。

`Vue.set` in 用于我们引用了一个反应性对象，但我们没有在定义状态的组件实例之外引用它们的情况。

# 在 Vue 路由器中获取上一页 URL

我们可以从`beforeRouteEnter`钩子中的`from`参数得到 Vue 路由器中的上一页 URL。

例如，我们可以写:

```
data() {
 return {
   ...
   prevRoute: null
 }
},
beforeRouteEnter(to, from, next) {
  next(vm => {
    vm.prevRoute = from
  })
},
```

上面的代码是组件代码的一部分。

我们只需给它加上`beforeRouteEnter`挂钩，然后就可以得到之前导航到的路线。

![](img/4a25031f6d48438308a15e0bc8d8678a.png)

Photo by [Max Baskakov](https://unsplash.com/@snowboardinec?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以从`beforeRouteEnter`钩子中得到之前路线的 URL。

要添加图像，我们可以在路径中使用前缀`~@`将图像作为模块导入 CSS 中。

全局属性可以作为静态变量或实例变量添加。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**