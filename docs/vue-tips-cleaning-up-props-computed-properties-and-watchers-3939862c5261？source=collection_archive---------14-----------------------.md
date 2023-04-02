# Vue 提示——清理道具、计算属性和观察者

> 原文：<https://javascript.plainenglish.io/vue-tips-cleaning-up-props-computed-properties-and-watchers-3939862c5261?source=collection_archive---------14----------------------->

![](img/f280f6e867102f56c9ef66b7863435ec.png)

Photo by [Samuel Sng](https://unsplash.com/@samuelsngx?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将探讨如何使开发 Vue 应用程序变得更加容易，包括减少组件接受的属性，以及消除计算属性和观察器之间的混淆。

# 清理道具

我们应该减少一个组件需要的道具数量。这使得组件更难处理，代码也更长。

我们可以用几种方法做到这一点。最好的方法是将它们作为一个道具而不是多个道具的对象的属性来传递。例如，我们可以编写以下代码来实现这一点:

`components/Button.vue`:

```
<template>
  <button :style="styles">Button</button>
</template><script>
export default {
  name: "Button",
  props: {
    styles: Object
  }
};
</script>
```

`App.vue`:

```
<template>
  <div id="app">
    <Button :styles="{color: 'white', backgroundColor: 'black', outline: 'none'}"/>
  </div>
</template><script>
import Button from "./components/Button";export default {
  name: "App",
  components: {
    Button
  }
};
</script>
```

在上面的代码中，我们传入了一个对象作为`style`道具的值。这比为每种风格传入多个道具要简洁得多。

另一个例子是用`v-bind`指令传入多个属性:

`components/Button.vue`:

```
<template>
  <button v-bind="attrs">Button</button>
</template><script>
export default {
  name: "Button",
  props: {
    attrs: Object
  }
};
</script>
```

`App.vue`:

```
<template>
  <div id="app">
    <Button :attrs="{autofocus: 'true', name  : 'foo', type: 'button', value: 'foo'}"/>
  </div>
</template><script>
import Button from "./components/Button";export default {
  name: "App",
  components: {
    Button
  }
};
</script>
```

在上面的代码中，我们将`attrs`属性传入`Button`组件，然后我们使用`v-bind=”attrs”`将它们作为属性应用于 out 按钮元素。

当我们在开发人员控制台中检查按钮时，应该会看到应用的属性。

![](img/e1bd3ad1c92871c09b101bf73705fce9.png)

Photo by [Xenia Bogarova](https://unsplash.com/@xeniabogarova?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 不要混淆计算属性和观察器

计算属性应该尽可能多地用于派生属性，除非我们需要观察某些东西并从它们产生副作用。

计算属性让我们从现有数据中派生出新数据。例如，如果我们有`firstName`和`lastName`并且我们想要从`firstName`和`lastName`创建一个属性`fullName`，我们创建一个计算属性如下:

```
<template>
  <div id="app">{{fullName}}</div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      firstName: "Jane",
      lastName: "Smith"
    };
  },
  computed: {
    fullName() {
      return `${this.firstName} ${this.lastName}`;
    }
  }
};
</script>
```

在上面的代码中，我们有`computed`属性，它有返回``${this.firstName} ${this.lastName}``的`fullName`方法。

然后我们将它作为一个属性引用，就像我们在模板`{{fullName}}`中的`data`一样。

计算的属性是反应性的。如果`firstName`或`lastName`发生变化，将返回一个新值。因此，它总是最新的。

要使用观察器做到这一点，我们必须编写以下代码:

```
<template>
  <div id="app">{{fullName}}</div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      firstName: "Jane",
      lastName: "Smith",
      fullName: ""
    };
  },
  watch: {
    firstName: {
      immediate: true,
      handler() {
        this.fullName = `${this.firstName} ${this.lastName}`;
      }
    },
    lastName: {
      immediate: true,
      handler() {
        this.fullName = `${this.firstName} ${this.lastName}`;
      }
    }
  }
};
</script>
```

我们可以看到，上面的代码要复杂得多，我们还得记得将`immediate`设置为`true`，这样除了后续的，我们还能看到初始值的变化。在`handler`功能中，我们每次都要设置`this.fullName`。

与计算属性做同样的事情，这是一种更加复杂和容易出错的方法。

计算属性是纯函数，而观察器处理程序会产生副作用，这也使得它们更难测试。

观察器的用处在于产生副作用。从上面的代码中我们可以看到，它用于通过将`this.fullName`设置为`this.firstName`和`this.lastName`的组合来创建副作用。

例如，我们可以如下使用它:

```
<template>
  <div id="app">
    <input v-model="name">
    <p>{{info.age}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      name: "",
      info: {}
    };
  },
  watch: {
    async name(val) {
      const response = await fetch(`https://api.agify.io?name=${val}`);
      this.info = await response.json();
    }
  }
};
</script>
```

在上面的代码中，我们观察`name`字段的值，然后调用 Agify API 从名称中获取一些数据。

然后我们将响应值设置为`this.info`。我们不能对计算属性这样做，因为我们不想在计算属性中返回一个承诺。

# 结论

我们应该清理我们的道具，将它们作为对象而不是多个道具传入。

计算属性适用于从现有数据中导出数据，而观察器适用于提交副作用，例如当我们需要运行异步代码时。

## **简明英语笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****