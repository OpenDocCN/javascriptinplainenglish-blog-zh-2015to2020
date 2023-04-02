# 常见的 Vue 问题—导入、阵列更新和共享代码

> 原文：<https://javascript.plainenglish.io/common-vue-problems-imports-array-updates-and-sharing-code-9f9bbcb9d1f8?source=collection_archive---------8----------------------->

![](img/564a5e9e56fdf4c8d4216cb259439c18.png)

Photo by [Muhammad Ruqiyaddin](https://unsplash.com/@mruqi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在模块外部导入模块

我们可以将值为`module`的类型属性添加到脚本标签中，以表明该脚本是一个模块。

然后我们可以在里面导入其他模块。

但是，最好的方法是用 Vue CLI 创建一个项目。

然后我们可以在任何地方导入模块。

它将被编译成可以在现代浏览器上运行的代码。

# 更新页面上的数据而不刷新

为了更新页面而不刷新，我们更新页面上的状态变量。

例如，我们可以写:

```
Vue.component('favorite', {
  template: '<button type="button" @click.prevent="updateFaviteOption" :class="{ 'is-favorited': isFavorited }">favorite</button>',
  props: [
    'storeId',
    'isFavorited'
  ],
  data() {
    return {
      isFavorited: false,
      data: {}
    }
  },
  methods: {
    updateFavoriteOption() {
      this.isFavorited = !this.isFavorited;
      const res = await fetch('/some/url');
      const data = await res.json();   
      this.data = data;
    }
  }
});
```

我们只需添加状态变量作为在`data`中返回的对象的属性。

然后我们可以根据自己的意愿通过给它赋值来更新它。

# Vue 和 CORS 请求

为了让 CORS 请求工作，我们必须将正确的标题添加到我们的后端应用程序。

我们必须添加`Access-Control-Allow-Origin`头，它可以设置为`*`或我们的网站。

同样，我们必须设置`Access-Control-Allow-Headers`标题。

我们将其设置为`X-Requested-With, Content-Type, Accept, Origin, Authorization`。

我们还必须设置`Access-Control-Allow-Methods`头，它被设置为我们允许 CORS 请求的方法。

我们必须添加`GET, POST, PUT, DELETE, OPTIONS`来让所有请求在不同于服务器应用程序的域中完成。

`OPTIONS`需要预检请求，这让我们知道是否可以提出请求。

其余的动词发出实际的请求。

# 更新数组不会更新页面

当我们更新一个数组时，页面可能不会被更新。

如果我们试图用新值更新现有的数组项，就会发生这种情况。

要更新现有的数组条目并更新页面，我们必须使用`this.$set`。

例如，我们可以写:

```
this.$set(this.arr, 0, 9);
```

在方法上。

第一个参数是数组。

第二个是要更新的项目的索引。

第三个参数是用来更新它的参数。

# _ 这个。$store 未定义

如果我们对一个方法使用箭头函数而不是常规函数，我们会得到`this.$store`未定义。

箭头函数不会绑定到组件中`this`的组件。

所以我们应该对任何生命周期挂钩或方法使用常规函数。

# 将 Props 组件传递给根实例

我们可以调用`this.$emit`来发出一个事件并将数据传递给父组件。

例如，我们可以写:

```
this.$emit('update', this.name);
```

在子组件的方法中。

然后我们可以写:

```
<child @update="changeName"></child>
```

在父组件的方法中。

我们调用将事件对象作为参数的`changeName`方法。

在组件代码中，我们写道:

```
{
  ...
  methods: {
    changeName(name) {
      this.getname = name;
    }
  }
  ...
}
```

`name`具有传递给`$emit`的第二个参数的值。

# Vue 中的角度服务当量

在 Vue 中没有与 Angular 服务完全相同的实体。

如果我们想添加一个无状态服务，那么我们可以添加一个 mixin。

如果我们想添加一个有状态的服务，那么我们可以使用 Vuex 来共享数据。

此外，我们可以从模块中导出值并使用它们。

我们也可以使用 JavaScript 全局对象，这些对象由所有组件共享。

此外，我们可以扩展`Vue.prototype`属性，将我们想要的添加到`Vue`实例中。

那么这将在整个应用程序中可用。

我们可以添加一个 mixin:

```
const myMixin = {
  methods: {
    hello() {
      console.log('hello')
    }
  }
}const Component = Vue.extend({
  mixins: [myMixin]
})
```

我们可以通过编写以下代码来添加全局 mixin:

```
Vue.mixin({
  methods: {
    hello() {
      console.log('hello')
    }
  }
})
```

那么我们就不必在组件中添加一个`mixins`属性。

为了给`Vue.prototype`添加一个属性，我们可以写:

```
import axios from 'axios';
Vue.prototype.$http = axios;
```

然后当我们创建组件或者一个`Vue`实例时，我们可以使用`this.$http`来引用它。

![](img/df764dbd21b0b0bced8c241408b5046e.png)

Photo by [Chris Benson](https://unsplash.com/@lordmaui?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

如果我们想使用模块，我们应该使用 Vue CLI 创建一个项目。

有很多方法可以用 Vue 重用代码。

为了让 Vue 在更新现有数组项目时更新页面，我们使用了`this.$set`。

## **用简单英语写的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**