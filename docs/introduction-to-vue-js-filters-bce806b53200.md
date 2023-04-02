# Vue.js 过滤器简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-filters-bce806b53200?source=collection_archive---------9----------------------->

![](img/ed3942e22fece1262d11c02560b9d4c2.png)

Photo by [Dwayne Hills](https://unsplash.com/@dhillssr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何创建和使用 Vue.js 过滤器来将 JavaScript 表达式的更改应用到模板中。

# 定义和使用过滤器

过滤器应用于模板或`v-bind`表达式中小胡子插值内的 JavaScript 表达式。

例如，如果我们想在模板中以大写形式显示一个字符串，我们可以为它创建一个过滤器，并在模板中使用它。

我们可以编写如下代码:

`src/index.js`:

```
Vue.component("foo", {
  props: ["message"],
  template: `<p>{{message}}</p>`
});new Vue({
  el: "#app",
  data: {
    message: "Hello"
  },
  filters: {
    uppercase(value) {
      if (!value) return "";
      value = value.toString();
      return value.toUpperCase();
    }
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>
  <body>
    <div id="app">
      <p>{{message | uppercase}}</p>
      <foo :message="message | uppercase"></foo>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们在`filter`对象内的 Vue 实例中定义了一个过滤器。

我们只是返回了我们想要在模板中显示的内容，这是一个转换为大写的字符串。

`message`保持不变。

在模板中，我们将它应用于两个小胡子表达式和带有`:message`的`foo`组件中的`v-bind`。

上面的代码在本地定义了`uppercase`过滤器。我们也可以将其全局定义如下:

```
Vue.filter("uppercase", value => {
  if (!value) return "";
  value = value.toString();
  return value.toUpperCase();
});
```

我们上面有什么将使过滤器随处可用。

如果本地过滤器和全局过滤器都存在，则本地过滤器优先于全局过滤器。

我们可以通过链接来应用多个过滤器。例如，我们可以写:

```
{{ message | filterA | filterB }}
```

上面的表达式将采用`message`并通过`filterA`运行。然后`message`应用`filterA`后的结果将被传入`filterB`。

过滤器可以接受参数。例如，我们可以写:

```
{{ message | filterA(arg1, arg2) }}
```

`arg1`将作为第二个参数发送给`filterA`，而`arg2`将作为第三个参数传递给过滤函数。

我们可以定义和使用多个过滤器，如下所示:

`src/index.js`:

```
Vue.filter("greeting", (value, name, message) => {
  if (!value) return "Hello";
  value = value.toString();
  return `${value}, ${name}. ${message}`;
});Vue.filter("uppercase", value => {
  if (!value) return "";
  value = value.toString();
  return value.toUpperCase();
});new Vue({
  el: "#app",
  data: {
    message: "Hi"
  }
});
```

`index.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <title>App</title>
    <meta charset="UTF-8" />
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>
  </head>
  <body>
    <div id="app">
      <p>{{message | greeting('Joe', 'How are you?') | uppercase}}</p>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们定义了两个过滤器`greeting`和`uppercase`。

`greeting`过滤器将`message`放在前面，然后从参数中添加一些文本。

然后由`greeting`过滤器返回的内容通过`uppercase`过滤器以大写字符串的形式返回。

所以我们得到:

```
HI, JOE. HOW ARE YOU?
```

从浏览器显示。

![](img/73e0d9176134872cea8b63bb950ae5e6.png)

Photo by [Andrew Bae](https://unsplash.com/@mrandybae?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用过滤器来改变模板中的 JavaScript 表达式在`v-bind`和 mustache 插值表达式中的显示方式。

应用过滤器时，管道符号和过滤器名称位于表达式之后。

我们可以全局和局部地定义过滤器。他们也可以接受论点。

最后，我们可以通过用管道符号链接多个过滤器来应用它们。

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[](mailto:submissions@javascriptinplainenglish.com)**给我们，我们会把你添加为作者。**

**我们还推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！****