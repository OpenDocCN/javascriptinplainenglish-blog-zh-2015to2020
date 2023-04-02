# Vue.js 渲染函数-功能组件和 JSX

> 原文：<https://javascript.plainenglish.io/vue-js-render-functions-functional-components-and-jsx-543a43b5a897?source=collection_archive---------3----------------------->

![](img/6f463c3470de23e5168dd1e9d9bac172.png)

Photo by [Montse Monmo](https://unsplash.com/@monmo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究功能组件，并使用 JSX 而不是 JavaScript 来呈现 HTML。

# JSX

为了让我们键入更少的代码，我们可以使用 JSX 而不是调用`createElement`来创建渲染函数。

首先，我们必须添加使用下面的`package.json`:

```
{
  "name": "vanilla",
  "version": "1.0.0",
  "description": "",
  "main": "index.html",
  "scripts": {
    "start": "parcel index.html --open",
    "build": "parcel build index.html"
  },
  "dependencies": {
    "[@vue/babel-helper-vue-jsx-merg](http://twitter.com/vue/babel-helper-vue-jsx-merg)e-props": "1.0.0",
    "[@vue/babel-preset-jsx](http://twitter.com/vue/babel-preset-jsx)": "1.1.2"
  },
  "devDependencies": {
    "[@babel/core](http://twitter.com/babel/core)": "7.2.0",
    "parcel-bundler": "^1.6.1"
  },
  "keywords": []
}
```

然后我们运行`npm i`来安装这些包。

我们必须这样做，因为我们必须添加一些节点包来将 JSX 转换成浏览器可以呈现的东西。

完成代码后，我们将通过运行`npm start`来使用 Parcel 运行我们的应用程序。

然后将下面的预置添加到`.babelrc`项目的根文件夹中:

```
{
  "presets": ["@vue/babel-preset-jsx"]
}
```

最后，我们可以创建一个`heading`组件，让我们改变标题的大小，如下所示:

`src/index.js`:

```
Vue.component("heading", {
  render(createElement) {
    return createElement(`h${this.size}`, this.$slots.default);
  },
  props: {
    size: {
      type: Number,
      required: true
    }
  }
});new Vue({
  el: "#app",
  render() {
    return (
      <heading size={1}>
        <span>Hello</span> world!
      </heading>
    );
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
    <div id="app"></div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后当我们运行`npm start`时，我们看到显示`Hello world!`。

# 功能组件

功能组件让我们创建简单的组件，不管理任何状态，也没有生命周期方法。

我们可以通过传入`functional: true`来声明一个功能组件。

例如，我们可以如下定义一个功能组件:

`src/index.js`:

```
Vue.component("functional-component", {
  functional: true,
  props: {
    msg: {
      type: String
    }
  },
  render(createElement, context) {
    return createElement("p", context.props.msg);
  }
});new Vue({
  el: "#app",
  data: {
    msg: "foo"
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
      <functional-component msg="foo"></functional-component>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们通过将`functional`选项设置为`true`创建了一个名为`functional-component`的功能组件。

要获得道具，必须使用`context`而不是`this`，因为功能组件是无状态的，所以没有`this`。

最后，我们应该会看到`foo`显示出来。

在 Vue 2.3.0 之前，如果我们想在一个功能组件中接受 props，就需要`props`选项。但是，从 Vue 2.3.0 开始，我们可以省略`props`，组件节点上的属性将被隐式提取为道具。

从 Vue 2.5.0 开始，如果我们使用单文件组件，基于模板的功能组件可以用:

```
<template functional> </template>
```

一切都必须通过上下文传递，上下文是一个具有以下属性的对象:

*   `props` —带有提供道具的对象
*   `children` —VNode 子节点的数组
*   `slots` —返回插槽对象的函数
*   `scopedSlots` —具有作用域槽的对象。从 Vue 2.6.0 开始提供
*   `data` —传入`createElement`第二个参数的数据对象
*   `parent` —对父组件的引用
*   `listeners` —具有父注册侦听器的对象。这是`data.on`的别名，从 Vue 2.3.0 开始就有了
*   `injections` —如果使用了`inject`。这将有解决的注射。从 Vue 2.3.0 开始提供

![](img/257d24ea115c21efffd1f60612a17ccb.png)

Photo by [Matthew Henry](https://unsplash.com/@matthewhenry?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 将属性和事件传递给子元素或组件

我们可以通过`context.data`将整个选项对象传递给用`createElement`创建的子元素。

例如，我们可以编写以下代码来创建一个按钮功能组件:

`src/index.js`:

```
Vue.component("functional-button", {
  functional: true,
  render(createElement, context) {
    context.data = { ...context.data, domProps: { innerHTML: "Click Me" } };
    return createElement("button", context.data, context.children);
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
    <style>
      .button {
        background-color: yellow;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <functional-button class="button"></functional-button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们添加了`domProps`来设置按钮的名称，然后将其合并到现有的`context.data`中。`context.data`具有从模板传递过来的属性。于是我们有了`class`的传承。表示`context.data.class`就是`'yellow'`。

最后，我们得到一个黄色的文本，里面有 Click Me。

## 插槽()与子插槽

我们需要`slots().default`来获得没有名字的槽。我们需要`slots().name`来获得一个命名的槽。

例如，我们可以写:

`src/index.js`:

```
Vue.component("functional-component", {
  functional: true,
  render(createElement, context) {
    return createElement("div", {}, [
      context.slots().foo,
      context.slots().default
    ]);
  }
});new Vue({
  el: "#app"
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
      <functional-component>
        <template v-slot:foo>
          first
        </template>
        <p>second</p>
      </functional-component>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
firstsecond
```

显示在浏览器中，因为我们创建了一个`div`元素，数组中的`template`元素和`p`元素按此顺序排列。

`context.slots().foo`获取名为`foo`的插槽，`context.slots().default`获取没有名称的插槽。

# 结论

我们可以使用 JSX 而不是使用`createElement`来创建我们的 VNodes 来呈现 HTML。为此，我们必须添加一些节点包来将 JSX 转换成浏览器可以呈现的东西。

要创建没有状态的组件，我们可以通过将`functional`选项设置为`true`来创建功能组件，然后我们通过`render`函数中的第二个`context`参数来获取道具和其他数据。

## 用简单英语写的 JavaScript 的注释

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[T5T7，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**