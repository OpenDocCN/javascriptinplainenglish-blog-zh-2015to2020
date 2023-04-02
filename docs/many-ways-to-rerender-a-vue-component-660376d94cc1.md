# 重新渲染 Vue 组件的许多方法

> 原文：<https://javascript.plainenglish.io/many-ways-to-rerender-a-vue-component-660376d94cc1?source=collection_archive---------0----------------------->

![](img/0bb098ffb2fa14175306b28640e865b8.png)

Photo by [Tadeusz Lakota](https://unsplash.com/@tadekl?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将探讨重新呈现 Vue 组件的方法，以及哪种方法是正确的。

# 坏方法——重新加载整个页面

拥有单页面应用程序的全部意义在于，不必重新加载整个页面来更新数据。因此，重新加载整个页面来呈现组件是不好的。

例如，我们不应该编写类似下面这样的代码:

```
Vue.component("foo", {
  data() {
    return {
      names: ["foo", "bar", "baz"]
    };
  },
  methods: {
    reload() {
      location.reload();
    }
  },
  template: `
  <div>
    <button @click='reload'>Reload</button>
    <p v-for='n in names'>{{n}}</p>
  </div>
  `
});
```

我们有`location.reload()`，我们不应该在单页应用程序中调用它。

# 另一个糟糕的方法——v-if 黑客

仅当条件为`true`时，`v-if`指令才呈现页面。如果是`false`，就不会在 DOM 里。

`v-if`黑客的工作方式是将`v-if`条件设置为`false`，然后通过调用`$nextTick`在下一个 DOM 更新周期将其设置回`true`。

例如，我们可以编写如下代码，将渲染条件切换到`false`，然后在下一个更新周期将其切换回`true`:

```
Vue.component("foo", {
  data() {
    return {
      names: ["foo", "bar", "baz"],
      render: true
    };
  },
  methods: {
    reload() {
      this.render = false;
      this.$nextTick(() => {
        this.render = true;
      });
    }
  },
  template: `
  <div v-if='render'>
    <button @click='reload'>Reload</button>
    <p v-for='n in names'>{{n}}</p>
  </div>
  `
});
```

我们是这样写的:

```
this.render = false;
this.$nextTick(() => {
  this.render = true;
});
```

我们将`this.render`设置为`false`，然后在`$nextTick`回调中将`this.render`设置回`true`。这将把 div 从 DOM 中移除，然后再把它放回去。

这将在调用`$nextTick`回调后删除旧的 div 组件并创建一个新的 div 组件。

`$nexTick`也返回一个承诺，所以我们可以写:

```
Vue.component("foo", {
  data() {
    return {
      names: ["foo", "bar", "baz"],
      render: true
    };
  },
  methods: {
    async reload() {
      this.render = false;
      await this.$nextTick();
      this.render = true;
    }
  },
  template: `
  <div v-if='render'>
    <button @click='reload'>Reload</button>
    <p v-for='n in names'>{{n}}</p>
  </div>
  `
});
```

# 更好的方法—调用$forceUpdate

我们可以调用`$forceUpdate`来强制更新一个 Vue 组件。`$forceUpdate`不会刷新计算属性的值，所以对刷新计算属性不起作用。

为了使用它，我们可以编写以下代码:

```
Vue.component("foo", {
  data() {
    return {
      names: ["foo", "bar", "baz"],
    };
  },
  methods: {
    reload() {
      this.$forceUpdate();
    }
  },
  template: `
  <div>
    <button @click='reload'>Reload</button>
    <p v-for='n in names'>{{n}}</p>
  </div>
  `
});
```

我们只需要添加`this.$forceUpdate();`就可以让组件重新加载。

![](img/9b1446c04641070f43db38877a9fdcea.png)

Photo by [Kumiko SHIMIZU](https://unsplash.com/@shimikumi32?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 最好的方法——添加关键道具

确保我们的项目正确刷新的最好方法是给我们渲染的项目添加一个具有唯一值的`key`道具。只有当`key`值与我们正在更新的相匹配时，它才会重新创建组件。

`key`必须绑定到列表上的特定项目，这样 Vue 就知道要更新什么。如果是这种情况，它将只重新呈现列表中已更改的项目，因为它知道哪个对象是由我们将`key`值设置为的唯一 ID 更新的。

例如，我们可以给我们的`v-for`物品添加一个`key`道具，如下所示:

```
Vue.component("foo", {
  data() {
    return {
      names: [
        { id: 1, name: "foo" },
        { id: 2, name: "bar" },
        { id: 3, name: "baz" }
      ]
    };
  },
  template: `
  <div>    
    <p v-for='n in names' :key='n.id'>{{n.name}}</p>
  </div>
  `
});
```

我们只需用我们想要的值绑定到`key`属性，它会根据需要更新。

# 结论

确保`v-for`项目正确重新呈现的最佳方式是使用`key`道具。我们只需要传入一个唯一的 ID。`$forceUpdate`如果需要，也可以用来重新加载页面。

我们永远不应该使用黑客或重新加载整个页面。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****