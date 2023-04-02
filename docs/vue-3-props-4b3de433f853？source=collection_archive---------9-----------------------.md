# Vue 3 —道具

> 原文：<https://javascript.plainenglish.io/vue-3-props-4b3de433f853?source=collection_archive---------9----------------------->

![](img/c17766a7e8f96dfad6e750115a0d97ad.png)

Photo by [Chandler Cruttenden](https://unsplash.com/@chanphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看如何使用 Vue 3 的道具。

# 小道具

组件可以接受道具，这样我们就可以将数据从父组件传递给它。

我们可以通过将定义为字符串数组来添加属性。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post title="hello world"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        props: ["title"],
        template: `<h1>{{title}}</h1>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们创建了一个带有`props`属性的`blog-post`组件来定义我们接受哪些道具。

该数组将道具的名称作为字符串。

然后我们可以像处理`title`一样传递道具。

道具只是属性，可以有我们想要的任何值。

# 道具类型

为了使传递道具更容易，我们可以检查道具的数据类型。

这样，如果我们传入一些我们不期望的值，我们会得到一个错误。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post title="hello world"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("blog-post", {
        props: {
          title: String
        },
        template: `<h1>{{title}}</h1>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

将`title`道具类型设置为字符串。

我们可以传入任何其他构造函数来设置道具的有效类型，比如:

```
props: {
  title: String,
  likes: Number,
  date: Date,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise
}
```

这也是 ur 组件的良好文档。

# 传递静态或动态道具

静态道具可以用`v-bind`传入。

我们在前面的例子中使用了`title`道具。

我们有:

```
<blog-post title="hello world"></blog-post>
```

它有一个`title`属性，是一个静态字符串。

如果我们想传入其他任何东西，那么我们需要使用`v-bind`或简称`:`。

例如，我们可以写:

```
<blog-post :title="post.title"></blog-post>
```

传入`post`对象的`title`属性。

我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post v-for="post of posts" :title="post.title"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [
              { title: "hello world", author: "james" },
              { title: "good news", author: "james" },
              { title: "bad news", author: "james" }
            ]
          };
        }
      }); app.component("blog-post", {
        props: {
          title: String
        },
        template: `<h1>{{title}}</h1>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

将`post`的`title`属性传递给`title`属性。

我们也可以传入一个表达式。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <blog-post
        v-for="post of posts"
        :title="`${post.title} by ${post.author}`"
      ></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [
              { title: "hello world", author: "james" },
              { title: "good news", author: "mary" },
              { title: "bad news", author: "jane" }
            ]
          };
        }
      }); app.component("blog-post", {
        props: {
          title: String
        },
        template: `<h1>{{title}}</h1>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们从`title`和`author`属性创建了一个字符串，并将其作为`title`属性的值传入。

![](img/a7742c7c22fb032467f395ec381c446c.png)

Photo by [Chris Leipelt](https://unsplash.com/@cleipelt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以注册 props，让我们将值传递给子组件。

同样，我们可以用构造函数作为`prop`属性的值来验证数据类型。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**