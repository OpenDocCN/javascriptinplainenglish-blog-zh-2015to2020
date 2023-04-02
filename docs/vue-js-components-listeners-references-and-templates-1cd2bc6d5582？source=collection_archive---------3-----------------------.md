# Vue.js 组件—监听器、引用和模板

> 原文：<https://javascript.plainenglish.io/vue-js-components-listeners-references-and-templates-1cd2bc6d5582?source=collection_archive---------3----------------------->

![](img/e5d93431d47f040ced13e0b289b82696.png)

Photo by [Kym Ellis](https://unsplash.com/@kymellis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究编程事件侦听器、递归、循环引用以及定义模板的其他方法。

# 编程事件侦听器

除了用`$emit`发射事件，用`v-on`监听事件，Vue 还在其事件接口中提供了其他方法。以下功能可用:

*   收听`$on(eventName, eventHandler)`的事件
*   使用`$once(eventName, eventHandler)`只监听一次事件
*   停止收听`$off(eventName, eventHandler)`的事件

当我们需要以编程方式监听事件时，它们是可用的。

我们可以如下使用它:

`src/index.js`:

```
Vue.component("foo-button", {
  template: `<button [@click](http://twitter.com/click)='$root.$emit("toggleFoo")'>Toggle Foo</button>`
});new Vue({
  el: "#app",
  data: {
    foo: ""
  },
  mounted() {
    this.$root.$on("toggleFoo", function() {
      this.foo = this.foo === "foo" ? "" : "foo";
    });
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
      <foo-button></foo-button>
      {{foo}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们调用了`$root.$emit`来发出根 Vue 实例中的事件。然后根 Vue 实例可以监听事件，就像我们上面提到的那样，它是用`this.$root.$on`挂载的。

最后，当我们单击 Toggle Foo 时，我们看到单词`foo`随着我们单击按钮而切换开和关。

Vue 的事件系统不同于浏览器的事件目标 API。但是，它们的工作方式仍然相似。

`$emit`、`$on`和`$off`不是`dispatchEvent`、`addEventListener`和`removeEventListener`的别名。

# 循环引用

## 递归组件

我们可以通过向组件添加`name`属性来递归引用组件。

例如，我们可以编写以下代码来显示递归嵌套对象中的数据:

`src/index.js`:

```
Vue.component("foo-box", {
  name: "foo-box",
  props: ["foo"],
  template: `
    <div>
      <p>{{foo.val}}</p>      
      <foo-box v-if='foo.foo' :foo='foo.foo'></foo-box>
    </div>
  `
});new Vue({
  el: "#app",
  data: {
    foo: {
      val: "foo",
      foo: {
        val: "foo",
        foo: {
          val: "foo"
        }
      }
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
      <foo-box :foo="foo"></foo-box>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们看到:

```
foofoofoo
```

显示在屏幕上。

我们必须确保我们有一个`v-if`或另一个条件来在某个点结束递归渲染。否则，我们将得到一个无限循环，并将导致“超出最大堆栈大小”错误

## 组件之间的循环引用

`Vue.component`全部自动解析对组件的循环引用。

如果我们使用模块系统，那么我们需要在`beforeCreate`钩子中明确地`require`它或者`import`它。

为了要求它，我们可以写:

```
beforeCreate() {
  this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
}
```

如果我们使用的是 Webpack 的`import`，我们可以写类似的东西:

```
components: {
  TreeFolderContents: () => import('./tree-folder-contents.vue')
}
```

对于`Vue.components`定义的组件，我们可以使用它们，无需做如下任何更改:

`src/index.js`:

```
Vue.component("tree-folder-contents", {
  props: ["children"],
  template: `
    <ul>
      <li v-for="child in children">
        <tree-folder v-if="child.children" :folder="child"/>
        <span v-else>{{ child.name }}</span>
      </li>
    </ul>
  `
});Vue.component("tree-folder", {
  props: ["folder"],
  template: `
    <p>
      <span>{{ folder.name }}</span>
      <tree-folder-contents :children="folder.children"/>
    </p>
  `
});new Vue({
  el: "#app",
  data: {
    folder: {
      name: "folder",
      children: [{ name: "folder2" }]
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
      <tree-folder :folder="folder"></tree-folder>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

我们将看到`tree-folder-contents`和`tree-folder`分别是一些`tree-folder`和`tree-folder-contents`组件的父代。

![](img/adfb470c2d3a2e63d7cee3c723c21547.png)

Photo by [Jason Briscoe](https://unsplash.com/@jsnbrsc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 内嵌模板

当`inline-template`属性出现在子组件上时，组件将使用其内部内容作为模板，而不是将其视为分布式内容。

然而，内部可用数据的范围会令人困惑，因为我们可以访问子组件的范围，而不是标签中父组件的范围。

例如，如果我们有:

`src/index.js`:

```
Vue.component("bar", {
  data() {
    return {
      baz: "bar"
    };
  },
  template: `
    <p></p>
  `
});new Vue({
  el: "#app",
  data: {
    foo: "foo"
  }
});
```

`index.js`:

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
      <bar inline-template>
        <div>
          {{baz}}
        </div>
      </bar>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

则`{{bar}}`是从`bar`组件引用`baz`。它也覆盖了我们在`bar`中定义的模板。

# 结论

我们可以通过`$on`、`$off`和`$once`以编程方式定义事件侦听器。

要递归引用一个组件，我们必须向组件添加`name`属性。我们必须小心不要创建无限递归。

如果我们使用`Vue.component`来定义我们的组件，循环引用会自动解析。然而，如果我们使用单文件组件，那么我们就有了`import`的`require`了，这取决于我们使用的构建工具。

我们可以用`inline-template`属性在组件中定义模板。

## **来自 JavaScript 的普通英语注释:**

我们一直对帮助推广高质量内容感兴趣。如果您想将某篇文章提交给 JavaScript，请用您的中位用户名在[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)发邮件给我们，我们会将您添加为作者。