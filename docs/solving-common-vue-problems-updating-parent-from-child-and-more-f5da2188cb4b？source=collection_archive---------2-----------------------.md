# 解决常见的 Vue 问题—从子更新父及更多

> 原文：<https://javascript.plainenglish.io/solving-common-vue-problems-updating-parent-from-child-and-more-f5da2188cb4b?source=collection_archive---------2----------------------->

![](img/97136f461a57b2de7fba128c013c17ff.png)

Photo by [Rainier Ridao](https://unsplash.com/@rainierridao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 从子模型更新父模型

我们可以通过在子组件中发出一个事件，然后在父组件中监听它，从子组件中更新父组件的模型。

例如，我们可以写:

```
this.$emit('update', newData)
```

在孩子身上。

然后在父组件中，我们可以写:

```
<child :myProp="value" @update="onUpdate($event)" />
```

我们可以用带有第二个参数`$emit`的`$event`对象从`child`获取值。

`onUpdate`让我们运行使用该值的代码。

例如，我们可以这样定义`onUpdate`方法:

```
methods: {
  onUpdate(newData) {
    this.value = newData
  }
}
```

# 使用 Vue 路由器导航到新路线

如果我们用文字来定义我们的路线:

`main.js`

```
import Vue from "vue";
import VueRouter from "vue-router";...Vue.use(VueRouter);export const router = new VueRouter({
  mode: 'hash',
  base: "./",
  routes: [
    { path: "/", component: Home },
    { path: "/foo", component: Foo }
  ]
})new Vue({
  router,
  ...
}).$mount("#app");
```

然后，我们可以通过编写以下内容导航到组件中的路线:

```
this.$router.push("/foo");
```

如果我们的应用程序中有 Vuex 商店，我们还可以在 Vuex 操作中导入导航。

例如，我们可以写:

```
import { router } from "../main.js"export const someAction = ({commit}) => {
  router.push("/welcome");
}
```

# 向组件添加非反应性数据

我们可能希望在组件中添加非反应性数据。

这样，我们就不会像处理正常状态数据那样更改数据。

为此，我们可以将其设置为`this.$options`属性的值。

例如，我们可以写:

```
this.$options.arr = [1, 2];
```

在我们的组件代码中。

然后在我们的模板中，我们可以写:

```
<template>
  <ul>
    <li v-for="item in $options.arr">{{ item }}</li>
  </ul>
</template>
```

# 使用限定范围的样式

我们可以用 CSS 模块访问作用域样式。

子级的作用域 CSS 会影响父级的作用域 CSS。

如果我们不想这样做，那么我们可以使用 CSS 模块。

例如，我们可以写:

```
<template>
<div :class="$style.baz">
  <Bar></Bar>
</div>
</template><style module>
.baz {
  color: green;
}
</style>
```

在`webpack.config.js`中，我们添加:

```
{
  module: {
    rules: [
      // ... 
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: true,
              // customize generated class names
              localIdentName: '[local]_[hash:base64:8]'
            }
          }
        ]
      }
    ]
  }
}
```

加载`css-loader`。

我们有:

```
{
  loader: 'css-loader',
  options: {
    modules: true,     
    localIdentName: '[local]_[hash:base64:8]'
  }
}
```

将 CSS 加载器添加到我们的项目中。

# 有条件地禁用输入

我们可以通过使用带有修饰符`disabled`的`v-bind`或者简称为`:`语法来使`disabled`道具动态化。

例如，我们可以写:

```
<input type="text" :disabled="isDisabled">
```

当`isDisabled`为`true`时，输入被禁用。

等价地，我们可以写:

```
<input type="text" v-bind:disabled="isDisabled">
```

得到同样的结果。

# 点击切换类别

我们可以通过使用`v-class`指令来切换类。

例如，我们可以写:

```
<div
  class="initial" 
  v-class="{ active: isActive }">
  hello
</div>
```

同样，我们可以使用带有`class`属性的`v-bind`或`:`来做同样的事情:

```
<div
  class="initial" 
  :class="{ active: isActive }">
  hello
</div>
```

在这两种情况下，`active`道具只有在`isActive`为`true`时才会被应用。

`:class`和`v-bind:class`一样，所以我们也可以写成:

```
<div
  class="initial" 
  v-bind:class="{ active: isActive }">
  hello
</div>
```

# 向 href 属性传递一个值

要动态设置`href`属性，我们可以使用`v-bind`或`:`来设置。

该值将是一个返回字符串的表达式。

例如，我们可以写:

```
<a v-bind:href="`/articles/${a.id}`">
```

或者:

```
<a :href="`/articles/${a.id}`">
```

![](img/99d7ca8350449dbe1f00743ad6bc0191.png)

Photo by [Tai's Captures](https://unsplash.com/@taiscaptures?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`$emit`方法将数据从子节点发送到父节点。

为了加载作用域样式，我们可以在`styles`标签上使用`scoped`关键字。

或者我们可以使用 CSS 模块。

要动态设置属性，我们可以使用`v-bind`或者`:`。

为了动态地设置类，我们也可以使用`v-class`。

## **简单英语的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**