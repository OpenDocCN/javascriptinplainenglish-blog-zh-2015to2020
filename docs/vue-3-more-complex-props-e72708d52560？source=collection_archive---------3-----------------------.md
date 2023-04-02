# Vue 3 —更复杂的道具

> 原文：<https://javascript.plainenglish.io/vue-3-more-complex-props-e72708d52560?source=collection_archive---------3----------------------->

![](img/8f2384f6e78bc1a40f69fdb661e70395.png)

Photo by [Brian McGowan](https://unsplash.com/@sushioutlaw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看如何使用 Vue 3 的道具。

# 传递一个数字

我们可以将数字传递给道具。

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
      <blog-post v-for="post of posts" :likes="post.likes"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [{ title: "hello world", author: "james", likes: 100 }]
          };
        }
      }); app.component("blog-post", {
        props: {
          likes: Number
        },
        template: `<p>{{likes}}</p>`
      }); app.mount("#app");
    </script>
  </body>
</html>
```

创建接受`likes`属性的`blog-post`组件。

`likes`是一个数字，所以我们必须将`:`放在`likes`之前，以便我们传入一个表达式。

我们也可以传入一个数字:

```
<blog-post :likes="100"></blog-post>
```

# 传递一个布尔值

要传入一个布尔值，我们可以写:

```
<blog-post is-published></blog-post>
```

将`true`传入`is-published`支柱。

为了传入`false`，我们已经写出了整个表达式:

```
<blog-post :is-published='false'></blog-post>
```

我们可以传入其他表达式。

# 传递数组

我们可以传入一个数组作为属性值。

所以我们可以写:

```
<blog-post :comment-ids="[1, 2, 3]"></blog-post>
```

或者我们可以写:

```
<blog-post :comment-ids="post.commentIds"></blog-post>
```

# 传递对象

Vue 道具可以带物件。

所以我们可以写:

```
<blog-post
  :author="{
    firstName: 'james',
    lastName: 'smith'
  }"
></blog-post>
```

我们将一个对象传递给`author`道具。

`:`告诉 Vue 对象不是一个字符串。

我们还可以传入另一个返回对象的表达式。

# 传递对象的属性

要将对象的属性作为道具传入，我们可以使用不带参数的`v-bind`。

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
      <blog-post v-for="post of posts" v-bind="post"></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [{ title: "hello world", author: "james", likes: 100 }]
          };
        }
      }); app.component("blog-post", {
        props: {
          title: String,
          author: String,
          likes: Number
        },
        template: `
          <div>
            <h1>{{title}}</h1>
            <p>author: {{author}}</p>
            <p>likes: {{likes}}</p>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

然后`post`的属性将作为属性值传入`blog-post`。

属性名是属性名。

这是对以下内容的简写:

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
        :title="post.title"
        :author="post.author"
        :likes="post.likes"
      ></blog-post>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            posts: [{ title: "hello world", author: "james", likes: 100 }]
          };
        }
      }); app.component("blog-post", {
        props: {
          title: String,
          author: String,
          likes: Number
        },
        template: `
          <div>
            <h1>{{title}}</h1>
            <p>author: {{author}}</p>
            <p>likes: {{likes}}</p>
          </div>
        `
      }); app.mount("#app");
    </script>
  </body>
</html>
```

正如我们所看到的，它比速记要长得多，重复得多。

![](img/87b848e0b69e6d04b0514a8f246025fa.png)

Photo by [Luís Alberto Nogueira](https://unsplash.com/@bebetobn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vue 组件轻松地传递各种数据。

我们只需要用`v-bind`或`:`来简称。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**