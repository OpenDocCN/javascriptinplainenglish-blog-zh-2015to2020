# Vue 最佳实践—道具、间距和命名

> 原文：<https://javascript.plainenglish.io/vue-best-practices-props-spacing-and-naming-6b43f00b3c3c?source=collection_archive---------2----------------------->

![](img/ccb2113e0b003b13deaf07e19355e372.png)

Photo by [Sven Hornburg](https://unsplash.com/@_s9h8_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究一些关于道具、间距和命名的最佳实践。

# 证明名称大小写

在 Vue 应用中，道具的命名应该保持一致。

它们应该是 camelCase 中有效的 JavaScript 标识符名称。

例如，我们应该写:

```
<template>
  <div></div>
</template><script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>
```

正确的名称是`msg`，这是一个有效的 JavaScript 标识符名称。

# 默认道具

拥有默认道具是个好主意。

这样，如果我们忘记传入一个属性，我们仍然有一个属性的默认值。

例如，我们可以写:

```
<template>
  <div></div>
</template><script>
export default {
  name: "HelloWorld",
  props: {
    msg: {
      type: String,
      default: "hello"
    }
  }
};
</script>
```

我们向我们的道具对象添加了一个`default`字段。

现在，如果我们没有为`msg`道具传入任何东西，那么它将被设置为`'hello'`道具。

# 道具类型

我们可以在我们的 Vue 组件道具中添加道具类型，这样我们就不会传入类型错误的道具。

例如，我们可以写:

```
<template>
  <div></div>
</template><script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>
```

那么`msg`道具将永远是一个字符串。

如果不是，我们会得到一个错误。

# 将 HTML 元素内容放在新行中

如果我们有 HTML 元素的内容，那么我们可以把它们放在一个新的行。

例如，我们可以写:

```
<template>
  <tr>
    <td>{{ foo }}</td>
    <td>{{ bar }}</td>
  </tr>
</template><script>
export default {
  name: "App",
  data() {
    return {
      foo: 1,
      bar: 2
    };
  }
};
</script>
```

然后我们有 2 个`td` 元素在它们自己的线上。

把它们排成一行更容易阅读。

# v-bind 指令样式

我们可以使用`:`而不是`v-bind`来节省我们的打字时间。

例如，我们可以写:

```
<template>
  <HelloWorld :msg="msg"/>
</template><script>
import HelloWorld from "./components/HelloWorld";export default {
  name: "App",
  components: {
    HelloWorld
  },
  data() {
    return {
      msg: "hi"
    };
  }
};
</script>
```

我们有`:msg`，是`v-bind`的简称。

我们用速记省去了打字。

# v-on 风格

我们可以用`@`代替`v-on`。例如，我们可以写:

```
<template>
  <button @click="onClick">click me</button>
</template><script>
export default {
  name: "App",
  methods: {
    onClick() {
      alert('hello')
    }
  }
};
</script>
```

`@`是`v-on:click`的简称。

我们用速记省去了打字。

# v-html 的使用

我们在使用`v-html`的时候要小心，不要让攻击执行跨站脚本攻击。

我们应该尽可能地避免它，并对任何可能包含代码的内容进行转义。

# 这在模板中

`this`不应该使用的是 Vue 模板。在大多数情况下，它可能是作为复制和粘贴的错误添加的。

例如，不写:

```
<a :href="this.url">link</a>
```

我们应该写:

```
<a :href="url">link</a>
```

# 数组括号间距

在我们的 Vue 应用程序中，我们应该让数组括号保持间距，以便于阅读。

例如，我们可以写:

```
const arr = ['foo', 'bar'];
```

# 箭头间距

箭头函数应该有一致的间距。

例如，我们应该写:

```
const fn = () => {
  //...
}
```

箭头前后各有一个空格。

![](img/45288d70dfabf18f66a25233c4454891.png)

Photo by [Micheile Henderson](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 块间距

块间距应该一致，这样我们就可以写:

```
const fn = () => {
  //...
}
```

我们有两个缩进空间。

# 大括号样式

我们应该将左大括号放在声明的同一行。

例如，我们应该写:

```
if (foo) {
  baz();
} else {
  bar();
}
```

在`if`和`else`块的`else`的右括号后面有左大括号。

# 茶包

camelCase 应该用于声明变量。

这是一个普遍接受的约定，所以我们应该使用它来保持一致性。

例如，我们写道:

```
let fooBar = 1;
```

# 结论

我们应该使用 camelCase 作为适当的名称和变量。

间距应该在我们的代码中保持一致。

默认道具和道具类型验证也是防止错误的好方法。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**