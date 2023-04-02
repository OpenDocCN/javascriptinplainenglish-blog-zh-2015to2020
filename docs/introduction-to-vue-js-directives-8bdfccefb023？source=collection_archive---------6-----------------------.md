# Vue.js 指令简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-directives-8bdfccefb023?source=collection_archive---------6----------------------->

![](img/3848cc9e7fc6be5a419c22c3243856b1.png)

Photo by [Tony Yeung](https://unsplash.com/@mrtonyyeung?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看如何用 Vue.js 指令操作 DOM。

# 基础

除了使用像`v-for`和`v-if`这样的内置指令，Vue 还允许我们注册或拥有自定义指令。

这对于不能用组件轻松完成的低级 DOM 操作很有用。

例如，我们可以制作一个如下:

`src/index.js`:

```
Vue.directive("focus", {
  inserted(el) {
    el.focus();
  }
});const vm = new Vue({
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
      <input v-focus />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们定义了`focus`指令，通过给它添加一个`v-`前缀来引用它。

当 DOM 元素被插入 DOM 时,`focus`指令调用 DOM 元素上的`focus`。`inserted`方法是一个钩子，当应用了指令的元素被插入 DOM 时，它处理这种情况。

`el`参数有 DOM 元素，这就是为什么我们可以对它调用`focus`方法。

因此，当我们加载页面时，我们会看到`input`元素被聚焦。这是`focus`指令的工作。

上面的代码全局注册了该指令。我们也可以在本地注册该指令，如下所示:

`src/index.js`:

```
const focus = {
  inserted(el) {
    el.focus();
  }
};const vm = new Vue({
  el: "#app",
  directives: {
    focus
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
      <input v-focus />
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后`focus`指令将只对注册它的组件可用。

# 钩子函数

指令可以有以下钩子函数。它们都是可选的。

*   `bind` —仅在指令首次绑定到元素时调用一次。它用于一次性设置工作
*   `inserted` —当绑定元素插入其父节点时调用。它保证了父节点的存在。
*   `updarted` —在包含组件 VNode 更新之后但可能在子组件更新之前调用。VNode 是虚拟 DOM 中的一个节点，代表虚拟节点。
*   `componentUpdated` —当包含组件的 VNode 及其子组件的 VNode 更新时调用
*   `unbind` —当指令与元素解除绑定时，仅调用一次

![](img/64b2b0fc6a34ee9e15910435fde05c15.png)

Photo by [Meduana](https://unsplash.com/@meduana?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 指令挂钩参数

上面的挂钩接受以下参数:

## 埃尔

指令绑定到的元素。它可以用来操作 DOM

## 有约束力的

包含以下属性的对象:

*   `name` —不带`v-`前缀的指令名称
*   `value` —传递给指令的值。例如，如果我们有`v-foo='1'`，那么值就是`1`。
*   `oldValue` —前一个值。这仅适用于`update`和`compoentUpdated`。无论值是否改变，它都是可用的。
*   `expression` —字符串形式的绑定表达式。例如，如果我们有`v-foo='1 + 2'`，那么它就是`1 + 2`。
*   `arg`—传入指令的参数。例如，如果我们有`v-foo:bar`，那么`arg`就是`‘bar‘`。
*   `modifiers` —带有修改器的对象。如果我们有`v-foo.bar.baz`，那么它将是`{ bae: true, baz: true }`。

除了`el`之外的都是只读的。

例如，我们可以显示从参数传递的数据，如下所示:

`src/index.js`:

```
const foo = {
  inserted(el, binding, vnode) {
    const stringify = JSON.stringify;
    el.innerHTML = `
      name: 
        ${stringify(binding.name)}
      <br>
      value:  
       ${stringify(binding.value)}
      <br>
      expression:
        ${stringify(binding.expression)}
      <br>
      argument
       ${stringify(binding.arg)}
      <br>
      modifiers:
        ${stringify(binding.modifiers)}
      <br>
      vnode keys:
       ${Object.keys(vnode).join(", ")}`;
  }
};const vm = new Vue({
  el: "#app",
  directives: {
    foo
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
      <div v-foo:bar.baz="1 + 2"></div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到:

```
name: "foo"
value: 3
expression: "1 + 2"
argument "bar"
modifiers: {"baz":true}
vnode keys: tag, data, children, text, elm, ns, context, fnContext, fnOptions, fnScopeId, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce, asyncFactory, asyncMeta, isAsyncPlaceholder
```

# 结论

我们可以定义自己的 Vue 指令，通过将它们绑定到特定的 DOM 元素来操作 DOM 元素，然后运行这些元素的方法。

它通过在 Vue 操纵虚拟节点的某些阶段运行钩子来工作。

钩子将 DOM element 对象作为第一个参数，并将一个`binding`对象作为第一个参数，该对象包含关于指令的数据，如名称、传入的值、修饰符和参数。

来自 JavaScript 的简单英语提示:我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[submissions@javascriptinplainenglish.com](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。