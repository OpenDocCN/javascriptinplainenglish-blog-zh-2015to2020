# 模板的更多方法来 Vue 组件

> 原文：<https://javascript.plainenglish.io/more-ways-to-a-templates-to-vue-components-d9fc2b706d50?source=collection_archive---------9----------------------->

![](img/a1937b0a27715346f55f5c8461d3e907.png)

Photo by [Daniel DiNuzzo](https://unsplash.com/@ddinuzzo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何向 Vue 应用添加模板，包括渲染功能、JSX 和单文件组件。

# 渲染函数

渲染函数返回由 Vue 渲染的元素。这些元素都是用普通的 JavaScript 定义的。它不需要编译，并且它给我们提供了对 JavaScript 功能的完全访问，而不是由指令提供的。

例如，我们可以定义一个 render 函数来呈现一个按钮，该按钮带有一个 click 处理程序来增加计数，还带有一个 p 元素来显示计数，如下所示:

`index.js`:

```
Vue.component("app-counter", {
  render(createElement) {
    const that = this;
    return createElement("div", [
      createElement(
        "button",
        {
          on: {
            click() {
              that.count++;
            }
          }
        },
        "Increment"
      ),
      createElement("p", this.count)
    ]);
  },
  data() {
    return { count: 0 };
  }
});new Vue({
  el: "#app"
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <app-counter> </app-counter>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们调用`createElement`来创建一个包装器 div。然后在顶层`createElement`调用的第二个参数中，我们用更多的`createElement`调用创建了一个子元素。

对于按钮，`createElement`调用的第二个参数有一个带有 click 处理程序的对象。对于 p 标签，我们有元素的文本，即`this.count`。

呈现函数的好处是我们不需要像`v-if`或`v-for`这样的指令来有条件地呈现事物或分别呈现列表。

例如，我们可以用`Array.prototype.map`代替`v-for`:

`index.js`:

```
Vue.component("app-list", {
  render(createElement) {
    const that = this;
    return createElement("div", [
      ...that.fruits.map(f => createElement("p", f))
    ]);
  },
  data() {
    return { fruits: ["apple", "orange", "banana"] };
  }
});new Vue({
  el: "#app"
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <app-list> </app-list>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

而不是`v-if`，我们只有一个简单的`if`语句:

```
Vue.component("app-toggle", {
  render(createElement) {
    const that = this;
    return createElement("div", [
      createElement(
        "button",
        {
          on: {
            click() {
              that.showFoo = !that.showFoo;
            }
          }
        },
        "Toggle"
      ),
      that.showFoo ? createElement("p", "foo") : undefined
    ]);
  },
  data() {
    return { showFoo: false };
  }
});new Vue({
  el: "#app"
});
```

`index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <app-toggle> </app-toggle>
    </div>
    <script src="index.js"></script>
  </body>
</html>
```

在上面的代码中，我们有:

```
that.showFoo ? createElement("p", "foo") : undefined
```

有条件地添加 p 元素。它由按钮的单击处理程序控制，即:

```
click() {
  that.showFoo = !that.showFoo;
}
```

![](img/ab5170ce50715c02e108c8cec5ef7589.png)

Photo by [John Price](https://unsplash.com/@johnprice?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# JSX

我们也可以在渲染选项中使用 JSX。这将我们从所有必须通过调用`createElement`进行的输入和调试中解放出来。

例如，我们可以将 JSX 渲染如下:

```
<script>
export default {
  name: "app",
  data() {
    return {
      count: 0
    };
  },
  render() {
    return (
      <div>
        <button onClick={() => this.count++}>Increment</button>
        <p>{this.count}</p>
      </div>
    );
  }
};
</script>
```

它看起来像 HTML，但实际上是 JavaScript。在开始新项目时，将 JSX 添加到我们的应用程序的最简单方法是使用 Vue CLI。

在上面的代码中，我们用`onClick`道具附加了一个 click listen 我们的按钮。

为了使用这个应用程序，我们需要把它转换成普通的 JavaScript。

# 单列组件

单文件组件是使用 Vue CLI 创建的 Vue 应用程序的默认组件类型。

如果我们选择了 Vue CLI 向导中的所有默认选项，我们可以毫不费力地创建单个文件组件。例如，我们可以写:

```
<template>
  <div id="app">
    <button @click="count++">Increment</button>
    <p>{{count}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      count: 0
    };
  }
};
</script>
```

我们所要做的就是包含一个`template`标签来呈现显示给用户的项目。我们用`script`标签来包含逻辑。

使用 Vue CLI，单文件组件通过`npm run build`命令构建到普通的 JavaScript 应用程序中，因此我们可以在生产中运行它。

# 结论

最直接的方式是单文件组件，这恰好是构造 Vue 组件和应用的默认方式。它适用于简单和复杂的应用程序，无需选择任何选项或进行任何配置。

另一种方法是在`render` 函数中使用 JSX。它类似于单文件组件，但是我们使用普通的 JavaScript 而不是 HTML 进行渲染。

`render`函数也可以有普通的 JavaScript。我们通过调用`createElement`来创建元素。如果我们有太多的元素，嵌套就变得很困难，但是好处是我们可以用 JavaScript 代替指令进行动态操作。

## **来自简明英语团队的通知**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****