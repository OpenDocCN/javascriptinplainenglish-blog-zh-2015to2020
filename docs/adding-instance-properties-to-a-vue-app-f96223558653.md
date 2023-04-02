# 向 Vue 应用程序添加实例属性

> 原文：<https://javascript.plainenglish.io/adding-instance-properties-to-a-vue-app-f96223558653?source=collection_archive---------1----------------------->

![](img/bafb49c1e564c90de60e7b6732591dac.png)

Photo by [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 使开发前端应用程序变得容易。然而，我们仍然有可能遇到问题。

我们可能遇到的一个问题是添加全局可用的实例变量。

在本文中，我们将研究如何创建全局可用的实例变量。

# 添加实例属性

我们可以通过向`Vue.prototype`属性添加属性来为`Vue`实例添加属性。

例如，我们可以写:

```
Vue.prototype.$appName = 'example app'
```

然后我们可以实例化`Vue`实例，我们可以通过`this.$appName`得到属性。

例如，我们可以写:

```
new Vue({
  beforeCreate() {
    console.log(this.$appName)
  }
})
```

然后我们会在控制台上看到`'example app'`。

# 冲突

如果我们设置一个实例属性的值，那么我们可能在不同的生命周期钩子中得到不同的值。

假设我们添加了如下实例属性:

```
Vue.prototype.appName = 'example app';
```

如果我们有:

```
new Vue({
  data: {
    appName: 'other app'
  },
  beforeCreate() {
    console.log(this.appName)
  },
  created() {
    console.log(this.appName)
  }
})
```

然后在`beforeCreate`勾上，我们看到原来的名字，就是`'example app'`。

在`created`钩上，我们看到`'other app'`。

这是因为我们只有在创建`Vue`实例时才会覆盖`Vue.prototype.appBame`的值。

为了避免这个问题，我们添加了`$`作为实例变量的前缀。

# 在全球范围内将 Axios 添加到我们的 Vue 应用程序中

这对于添加我们希望在应用程序中全局使用的库非常有用。

例如，我们可以写:

```
import axios from 'axios';Vue.prototype.$http = axios;
```

然后，当我们实例化我们的`Vue`实例时，我们可以使用`this.$http`而不是重复地将`axios`导入到我们所有的组件中。

例如，我们可以写:

```
new Vue({
  el: '#app',
  data: {
    data:  {}
  },
  created() {
    var vm = this
    this.$http
      .get('https://api.agify.io/?name=michael')
      .then((response) => {
        vm.data= response.data
      })
  }
})
```

# 原型方法的语境

当我们访问原型方法时，我们必须使用常规函数而不是箭头函数。

这是因为箭头函数没有绑定到`this`。

因此，如果我们将它们用于顶级函数，如果我们引用其中的`this`的话，我们将无法访问`Vue`实例。

例如，如果我们有方法:

```
Vue.prototype.$upperCase = function(propertyName) {
  this[propertyName] = this[propertyName]
    .toUpperCase();
}
```

那么我们必须用一个如下的正则函数来调用我们的`$upperCase`方法:

```
new Vue({
  data: {
    message: 'foo'
  },
  created() {
    this.$upperCase('message')
    console.log(this.message);
  }
})
```

我们会得到`'FOO'`因为我们在常规函数中调用了`this.$upperCase`。

如果我们在箭头函数中调用它:

```
new Vue({
  data: {
    message: 'foo'
  },
  created: () => {
    this.$upperCase('message')
    console.log(this.message);
  }
})
```

我们将得到“未捕获类型错误:无法读取未定义的属性”。

这是因为`this`的值是错误的，因为我们设置了一个箭头函数作为`created`的值。

# 交替模式

我们可以使用 mixins 或 help 对象向组件添加方法或属性。

我们可以写:

```
const App = Object.freeze({
  name: 'example app',
})
```

然后，我们可以通过编写以下内容来访问它:

```
new Vue({
  data: {
    name: App.name
  },
})
```

我们可以通过编写以下代码来创建 mixin:

```
const aMixin = {
  methods: {
    hello() {
      console.log('hello')
    }
  }
}
```

然后我们可以通过书写来使用它:

```
const Component = Vue.extend({
  mixins: [aMixin]
})
```

或者:

```
new Vue({
  mixins: [mixin]
})
```

将`hello`方法合并到`Component`中。

![](img/4d7e5725a1751cd8c842379b094dc39c.png)

Photo by [salih su](https://unsplash.com/@younth?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以扩展`Vue`的原型，添加我们可以在整个应用程序中使用的全局属性和方法。

我们必须用常规函数引用它们，这样`this`的值就是我们的方法被调用的组件。

## 坦白地说

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**