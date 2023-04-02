# 解决常见的 Vue 问题——道具和动态组件等

> 原文：<https://javascript.plainenglish.io/solving-common-vue-problems-props-and-dynamic-components-and-more-652dd5095284?source=collection_archive---------0----------------------->

![](img/642ff642e1b324fe13ec2f4a360234d0.png)

Photo by [Michael Mims](https://unsplash.com/@michaelrmims?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 使开发前端应用程序变得容易。然而，我们仍然有可能遇到问题。

在本文中，我们将查看一些常见问题，并了解如何解决它们。

# 将道具传递给动态组件

我们可以使用`v-bind`指令将道具传递给动态组件。

例如，我们可以写:

```
<component :is="currentComponent" v-bind="currentProps"></component>
```

为了显示动态组件，我们添加了带有`is`道具的`component`组件。

`currentComponent`是带有组件名称的字符串。

`v-bind`有一个带有道具的物体，我们想传递给现在正在渲染的组件。

`currentProperties`拥有或返还道具的东西。

为了根据不同的组件名称传递不同的道具，我们可以使`currentProperties`成为一个计算属性。

例如，我们可以写:

```
data() {
  return {
    currentComponent: 'fooComponent',
  }
},
computed: {
  currentProps() {
    if (this.currentComponent === 'fooComponent') {
      return { foo: 'bar' }
    }
  }
}
```

`currentProps`在给定组件名称的情况下，返回我们想要传递的道具。

我们也可以将一个对象传递给`v-bind`，这样我们就可以写:

```
<component :is="currentComponent" v-bind="{ foo: 'bar' }"></component>
```

# 通过@click 调用多个函数

我们可以在为`@click`设定的值中调用任意多个函数。

所以我们可以写道:

```
<div @click="fn1(); fn2();"></div>
```

或:

```
<div v-on:click="fn1(); fn2();"></div>
```

# 在 Vue CLI 项目上设置 favicon.ico

我们可以将`favicon,ico`放在`public`文件夹中。

在`index.html`中，我们可以写:

```
<link rel="icon" href="<%= BASE_URL %>favicon.ico" />
```

让 Vue CLI 插值`BASE_URL`的路径。

# 未知自定义元素错误

如果我们得到未知的自定义元素错误，这意味着我们忘记注册组件。

例如:如果我们想引用`example-component`，首先要写注册:

```
components: {
  'example-component': ExampleComponent
},
```

在我们的组件对象中。

然后我们可以在模板中通过书写引用:

```
<example-component>...</example-component>
```

# 将计算属性与 v-for 一起使用

我们可以像使用任何其他属性一样，使用带有`v-for`的计算属性。

例如，如果我们有:

```
computed: {
  fullNames() {
    return this.items.map((item) => {
      return `${item.firstname} ${item.lastname}`;
    });
  }
}
```

然后，我们可以通过编写以下内容在模板中使用它:

```
<div v-for="name of fullNames">
   <p>{{name}}</p>
</div>
```

computed 属性返回从原始数据派生的一些数据。

在我们的示例中，我们返回了一个从`this.items`数组派生的数组。

然后，我们对其使用`v-for`就像对任何其他属性一样。

# 有条件地向元素添加属性

如果我们使用`v-bind`或`:`简称，我们可以把一个表达式放入一个属性中。

例如，我们可以写:

```
<input :required="isRequired">
```

或:

```
<input v-bind:required="isRequired">
```

在这两个例子中，我们将`isRequired`表达式转换为`v-bind`表达式。

如果`isRequired`为`true`，则`required`被设置为`true`。

# 更改模板分隔符

默认情况下，模板插值分隔符是双花括号。

例如，我们有:

```
{{text}}
```

把`text`的值放到页面上。

要更改分隔符，我们可以将`delimiters`属性添加到我们传递给`Vue`构造函数的对象中。

例如，我们可以写:

```
new Vue({
  el: '#app',
  data,
  delimiters: ["<%","%>"]
});
```

使用`<%`和`%>`作为开始和结束分隔符。

在我们的模板中，我们写道:

```
<% text %>
```

来界定我们的表达。

# data() { return {} }与 data 的区别:()=> ({ })

之间的区别:

```
data() { return {} }
```

并且:

```
data:() => ({ })
```

在我们的分量很大。

在第一个例子中，我们可以引用带有`this`的组件。

另一方面，我们不能在第二个例子中这样做，因为它是一个箭头函数。

因此，如果我们不打算在`data`方法中访问`this`，我们应该只使用第二个例子。

![](img/9b455e3f4b09c6b34ead7616c0530c5e.png)

Photo by [Artur Aldyrkhanov](https://unsplash.com/@aldyrkhanov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`v-bind`将道具传递给动态组件。

如果我们想为不同的组件传入不同的属性，我们可以用一个计算过的属性返回它们。

我们也可以改变模板分隔符。

属性也可以有条件地添加。

我们可以点击调用多个函数。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**