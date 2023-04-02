# Vue.js 观察器简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-watchers-3d3382842f33?source=collection_archive---------3----------------------->

![](img/6882bcd0c080b3bbbdc98fe9203311c6.png)

Photo by [Cristina Gottardi](https://unsplash.com/@cristina_gottardi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何定义和使用 Vue.js 观察器。

# 什么时候应该使用观察器？

当我们希望观察反应性属性中的数据变化时，我们可以使用观察器，并在值变化时进行一些异步或昂贵的选择。

例如，我们可以用它来观察输入的变化，然后当我们输入一些东西时，从 [Chuck Norris API](http://www.icndb.com/api/) 得到一个笑话。

首先，我们将以下内容放入`src/index.js`:

```
new Vue({
  el: "#app",
  data: {
    message: "",
    joke: ""
  },
  created() {
    this.debounceGetAllCapsMessage = _.debounce(this.getAllCapsMessage, 500);
  },
  methods: {
    async getAllCapsMessage() {
      const res = await fetch("https://api.icndb.com/jokes/random");
      const result = await res.json();
      this.joke = result.value.joke;
    }
  },
  watch: {
    message(newMessage, oldMessage) {
      this.debounceGetAllCapsMessage();
    }
  }
});
```

上面的代码观察`message`属性的变化，然后调用`debounceGetAllCapsMessage`方法，该方法在半秒钟的延迟后得到一个笑话。

`watch`方法将新值作为第一个参数，旧值作为第二个参数。

然后我们把下面的放在`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
  </head> <body>
    <div id="app">
      <input type="text" v-model="message" />
      <p>{{joke}}</p>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

正如我们所看到的，我们可以运行一个异步操作，在执行操作之前添加一个延迟，然后获得最终结果。

我们也可以使用`vm.$watch`来观察一个值。最多需要 3 个参数。

第一个是属性的路径，它是一个字符串。也可以是一个或多个事物组合起来观看的函数。

第二个参数是值改变时运行的函数。

`vm.$watch`的第三个参数是一个具有某些属性的对象。它们包括:

*   `immediate` —观察初始值的设置

该方法返回一个停止监视值的函数。这是一个可选参数。

![](img/89644db165f0e4b69368a894ee81fcb8.png)

Photo by [Eryk](https://unsplash.com/@eryk10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以如下使用它。在`src/index.js`中，我们有:

```
const vm = new Vue({
  el: "#app",
  data: {
    message: "",
    joke: ""
  }
});vm.$watch(
  "message",
  _.debounce(async function() {
    const res = await fetch("[https://api.icndb.com/jokes/random](https://api.icndb.com/jokes/random)");
    const result = await res.json();
    this.joke = result.value.joke;
  }, 500)
);
```

然后`index.html`，我们有:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
  </head> <body>
    <div id="app">
      <input type="text" v-model="message" />
      <p>{{joke}}</p>
    </div>
    <script src="./src/index.js"></script>
  </body>
</html>
```

和我们之前吃的一样。

在这两个例子中，我们应该在显示半秒钟延迟后得到一个新笑话。

# 结论

我们可以使用 options 和 object 中的`watch`方法和`vm.$watch`来观察值的变化。

`vm.$watch`方法监视一个给定路径的属性或返回它的函数或多个属性的组合。

`vm.$watch`采用一个值改变处理程序，我们可以在其中运行代码来处理一个反应属性的值何时改变。

`watch`方法在值改变时运行代码。

另外，`watch`方法将新值作为第一个参数，旧值作为第二个参数。