# Vue.js 渲染函数简介

> 原文：<https://javascript.plainenglish.io/introduction-to-vue-js-render-functions-8b5976e6c389?source=collection_archive---------3----------------------->

![](img/29ee69c226429cceb396952696a60ec9.png)

Photo by [Paul Dufour](https://unsplash.com/@bill_bokeh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用程序框架，我们可以用它来开发交互式前端应用程序。

在本文中，我们将了解如何定义和使用 Vue.js 渲染函数。

# 基础

我们可以定义并使用渲染函数来以编程方式显示 Vue 应用程序中的元素。

一个可以从使用渲染函数中受益的例子是有很多`v-if`情况的组件。

例如，如果我们想用一个组件显示不同大小的标题，我们可以定义以下组件:

`src/index.js`:

```
Vue.component("variable-heading", {
  template: "#variable-heading-template",
  props: {
    size: {
      type: Number,
      required: true
    }
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
    <script type="text/x-template" id="variable-heading-template">
      <h1 v-if="size === 1">
        <slot></slot>
      </h1>
      <h2 v-else-if="size === 2">
        <slot></slot>
      </h2>
      <h3 v-else-if="size === 3">
        <slot></slot>
      </h3>
      <h4 v-else-if="size === 4">
        <slot></slot>
      </h4>
      <h5 v-else-if="size === 5">
        <slot></slot>
      </h5>
      <h6 v-else-if="size === 6">
        <slot></slot>
      </h6>
    </script>
    <div id="app">
      <variable-heading :size="1">foo</variable-heading>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码通过将`size`值作为道具传递给`variable-heading`组件，然后组件模板中的`v-if`将根据`size`道具的值决定显示哪个元素。

它很长，有重复的`slot`元素。

我们可以使用渲染函数将上面的代码简化为:

`src/index.js`:

```
Vue.component("variable-heading", {
  props: {
    size: {
      type: Number,
      required: true
    }
  },
  render(createElement) {
    return createElement(`h${this.size}`, this.$slots.default);
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
      <variable-heading :size="1">foo</variable-heading>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

上面的代码是通过在我们的`variable-heading`组件中添加`render`方法来工作的，该组件通过调用`createElement`函数返回一个虚拟 DOM 节点，简称 VNode，其中标签名作为第一个参数，子 HTML 节点作为第二个参数。

虚拟 DOM 节点告诉 Vue 在屏幕上呈现什么，屏幕将变成一个真实的 DOM 节点。

`this.$slot.default`在`variable-heading`的开始和结束标签之间传递子 HTML 节点。

# 节点、树和虚拟 DOM

HTML 代码通过呈现称为 DOM 树的动态元素树来动态呈现。文档对象模型的 DOM 标准，它将 HTML 结构抽象成一棵树。

一棵树有节点，一些像文本和 HTML 节点这样的节点会显示在浏览器的屏幕上。

Vue 为我们操纵和渲染 DOM 树，这样我们就不用自己动手了。

例如，我们可以创建一个具有如下渲染功能的组件:

```
Vue.component("variable-heading", {
  data() {
    return { msg: "hi" };
  },
  render(createElement) {
    return createElement("p", this.msg);
  }
});
```

# createElement 参数

`createElement`需要几个论点。

第一个参数是 HTML 标记名、组件选项或异步函数，它解析为前面提到的两种项目之一。这是必需的。

第二个参数是一个对象，其数据对应于我们要添加到模板中的属性。这是一个可选的论点。

第三个也是最后一个参数是使用`createElement`构建的子节点的字符串或数组。这也是可选的。

# 数据对象

数据对象有许多属性，可以控制所创建的 VNode 的属性、类、事件处理程序和内联样式。

以下是可用的属性:

## 班级

它是一个具有类名的对象，我们希望将这些类名包含在我们呈现的 HTML 元素的`class`属性中。

例如，我们可以这样写:

```
class: {
  foo: true,
  bar: false
}
```

那么`foo`类包含在`class`属性中，而`bar`没有。

## 风格

一个包含我们想要包含的样式的对象。例如，我们可以这样写:

```
style: {
  color: 'yello',
  fontSize: '12px'
},
```

然后，上面的样式将包含在呈现的元素中。

## 属性

`attrs`对象具有我们想要包含在 rendred HTML 元素中的属性和值。例如，我们可以写:

```
attrs: {
  id: 'foo'
}
```

那么我们呈现的 HTML 元素将具有 ID `foo`。

## 小道具

`props`对象拥有组件的道具和相应的值。例如，我们可以写:

```
props: {
  foo: 'bar'
}
```

然后我们可以传入值为`'bar'`的`foo`道具。

## 东普罗普斯

对象拥有我们想要传入的 DOM 属性。例如，我们可以写:

```
domProps: {
  innerHTML: 'foo'
}
```

然后，我们呈现的 HTML 元素内部有文本`foo`。

## 在

使用`on`对象，我们可以为呈现的 HTML 元素设置事件处理程序。它用于监听通过`$emit`发出的事件。

例如，我们可以写:

```
on: {
  click: this.clickHandler
}
```

收听 Vue 点击事件。

![](img/c90bcd1e9492e1a6b79633fbe8a5ad01.png)

Photo by [Bethany Legg](https://unsplash.com/@bkotynski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 纳提翁

我们可以用`nativeOn`对象监听本地浏览器事件。它和`on`有相同的对象，只是我们有本地事件处理程序，而不是 Vue 事件处理程序。

## 指令

我们可以传入`directives`数组来传入一个或多个带有值、参数、修饰符等的指令。

例如，我们可以写:

```
directives: [
  {
    name: 'custom-directive',
    value: '2',
    expression: '1 + 1',
    arg: 'foo',
    modifiers: {
      bar: true
    }
  }
],
```

然后我们可以将带有修饰符、参数、值和表达式的`custom-directive`指令应用到 VNode 中。

## 范围插槽和插槽

我们可以通过将指令设置为一个返回元素的函数来将槽传递到指令中。

对于作用域槽，我们可以写:

```
scopedSlots: {
  default: props => createElement('span', props.text)
}
```

对于插槽，我们可以将值设置为插槽的名称。

## 其他特殊的顶级属性

`key`获取键，`ref`获取字符串引用名，`refInFor`是一个布尔值，如果同一个`ref`名应用于多个元素，则该布尔值为`true`。

# 完整示例

我们可以设置上面的属性，如下例所示:

`src/index.js`:

```
Vue.component("anchor-text-block", {
  render(createElement) {
    const [slot] = this.$slots.default;
    const headingId = slot.text.toLowerCase().replace(/ /g, "-");return createElement("div", [
      createElement(
        "a",
        {
          attrs: {
            name: headingId,
            href: `#${headingId}`
          }
        },
        this.$slots.default
      ),
      createElement(
        "p",
        {
          style: {
            color: "green",
            fontSize: this.fontSize
          }
        },
        this.paragraphText
      )
    ]);
  },
  props: {
    fontSize: {
      type: Number,
      required: true
    },
    paragraphText: {
      type: String,
      required: true
    }
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
      <anchor-text-block paragraph-text="foo" :font-size="12"
        >link</anchor-text-block
      >
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们创建了一个`anchor-text-block`组件，它支持`paragraph-text`和`font-size`，如组件代码中的`props`属性所示。

模板中的 kebab case 自动映射到 JavaScript 代码中的 camelCase。

在`render`方法中，我们调用了`createElement`，它创建了一个`div`，然后在第二个参数中，我们创建了一个包含`a`和`p`元素的数组。这是我们传递两个道具的地方。

我们还将`attrs`属性设置为`#link`，因为我们将`link`作为槽的内部文本，因为这是我们在开始和结束标记之间添加的内容。

在`p`元素中，我们设置道具的`fontSize`样式，并将颜色设置为绿色。

# 结论

我们可以使用`render`函数来创建比我们通常的组件更灵活的组件。

它可以拥有组件通常拥有的一切，比如事件处理程序、指令、样式、属性、嵌套元素和组件等等。

## JavaScript 用简单的英语写的一个注释:

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**