# Vue 提示— CSS 框架和观察嵌套数据

> 原文：<https://javascript.plainenglish.io/vue-tips-css-frameworks-and-watching-nested-data-ac33a30c95cb?source=collection_archive---------11----------------------->

![](img/6c091ca5ce3cc04195d4f49d5467fd90.png)

Photo by [Joshua Reddekopp](https://unsplash.com/@joshuaryanphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何使用 CSS 框架来节省我们手动设置样式和查看嵌套数据的时间。

# 使用现成的 CSS 框架

现成的 CSS 框架节省了我们设计样式的时间，否则我们必须手动设计样式。有许多框架可供选择，包括 Tailwind CSS、Foundation、布尔玛和 Bootstrap。

它们附带了类和 JavaScript 代码来设计静态组件，如输入和表格，还创建动态组件，如模态和弹出窗口。

其中一些有自己的 Vue 组件。例如，Buefy 是一个基于布尔玛的图书馆。BootstrapVue 是基于 Bootstrap 的组件库。

如果有一个 Vue 组件库，那么我们应该使用它，因为它们可以更好地与我们应用程序中的其他 Vue 组件集成。它们像任何其他 Vue 组件一样接受道具并发出事件。

例如，我们可以使用 Buefy 来利用布尔玛框架，而无需基于布尔玛开发我们自己的风格化组件，如下所示:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    name: ""
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/buefy/dist/buefy.min.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/buefy/dist/buefy.min.css" />
  </head>
  <body>
    <div id="app">
      <b-field label="Name" type="is-danger" message="Name">
        <b-input type="name" v-model="name" maxlength="30"> </b-input>
      </b-field>
      <p>{{name}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有来自 Buefy 的`b-field`组件，它是一个输入表单字段组件。然后我们有我们的`b-input`组件，它是实际的输入组件。

标签由`b-field`组件呈现，因此我们可以看到标签。此外，还有用`type`道具和`message`道具提供显示信息的样式。

输入有输入验证，并用`v-model`绑定到我们选择的变量。

我们通过使用库和引用提供的组件完成了所有这些，所以这是快速使用样式化组件的最佳选择。

![](img/cd5ada11e51e508963f18e6bd3012bb7.png)

Photo by [Marty Southwell](https://unsplash.com/@martysouthwell?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在 Vue 中观察嵌套数据

我们可以通过在组件选项中给`watch`对象添加一个函数来观察数据。

这对于观察嵌套对象和数组的更新很有用。在这些情况下，Vue 不知道发生了什么变化，除非我们显式地添加 watcher 并将其设置为观察对象的结构变化。这是因为 Vue 发现对该对象的引用没有改变。

例如，我们可以使用观察器来观察数组的变化，如下所示:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    nums: [1, 2, 3]
  },
  methods: {
    append() {
      this.nums.push(Math.max(...this.nums) + 1);
    }
  },
  watch: {
    nums: {
      deep: true,
      handler() {
        console.log("nums changed");
      }
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button @click="append">Append</button>
      <p v-for="n in nums">{{n}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有一个`append`方法将数字推入到被调用的`nums`数组中。然后我们有了内部带有`nums`对象的`watch`对象来观察`nums`的变化。我们将`deep`设置为`true`,以便它观察所有的结构变化。当它被改变时`handler`就会运行。

然后，当我们单击 Append 按钮时，`handler`功能将会运行。

还有一个`immediate`选项，以便在字段初始化时运行观察器。例如，我们可以如下使用它:

```
new Vue({
  el: "#app",
  data: {
    nums: [1, 2, 3]
  },
  methods: {
    append() {
      this.nums.push(Math.max(...this.nums) + 1);
    }
  },
  watch: {
    nums: {
      deep: true,
      immediate: true,
      handler() {
        console.log("nums changed");
      }
    }
  }
});
```

然后我们会看到第一次加载应用程序时记录的`nums changed`。

如果我们想使用同一个处理函数来查看多个项目，我们可以将其设置为`methods`对象中的函数。

例如，我们可以写:

`index.js`:

```
new Vue({
  el: "#app",
  data: {
    nums: [1, 2, 3],
    nums2: [4, 5, 6]
  },
  methods: {
    append() {
      this.nums.push(Math.max(...this.nums) + 1);
    },
    appendTo2() {
      this.nums2.push(Math.max(...this.nums2) + 1);
    },
    watch() {
      console.log("nums changed");
    }
  },
  watch: {
    nums: {
      deep: true,
      immediate: true,
      handler: "watch"
    },
    nums2: {
      deep: true,
      immediate: true,
      handler: "watch"
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <button @click="append">Append</button>
      <button @click="appendTo2">Append 2</button>
      <p v-for="n in nums">{{n}}</p>
      <p v-for="n in nums2">{{n}}</p>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有 2 个按钮，当我们通过分别运行`append`或`appendTo2`函数单击它时，它们将数字附加到 2 个独立的数组中。

然后，由于我们将`handler`设置为`'watch'`，调用了`methods`中的`watch`方法，所以如果我们单击任何按钮，我们将看到`“nums changed”`被记录。

# 结论

现成的 CSS 框架对于减少我们需要的样式工作来说是很棒的。一些框架有组件库，这样我们就不用花时间自己设计了。

我们应该使用观察器来观察嵌套很深的对象和数组的值的变化。它可以选择开始观察组件是否加载，也可以选择观察深度嵌套的对象，这是 computed properties 无法做到的。

## 来自简明英语团队的说明

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****