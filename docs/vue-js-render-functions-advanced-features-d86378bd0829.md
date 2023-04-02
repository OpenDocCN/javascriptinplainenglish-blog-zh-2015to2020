# Vue.js 渲染函数—高级功能

> 原文：<https://javascript.plainenglish.io/vue-js-render-functions-advanced-features-d86378bd0829?source=collection_archive---------3----------------------->

![](img/e3feec22a6d3cbccbcf218bbe1df716f.png)

Photo by [Robert Bye](https://unsplash.com/@robertbye?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究渲染函数的更多高级特性。

render 函数是组件的一种方法，它为我们提供了一种通过创建虚拟 DOM 节点来创建 HTML 元素的替代方法。

# 虚拟节点必须是唯一的

在传递给 VNode 函数第二个参数的数组中，VNode 必须是唯一的。

我们不能创建包含重复子元素的虚拟节点。例如，以下内容是无效的:

```
Vue.component("text-block", {
  render(createElement) {
    const dupNode = createElement("p", "hi");
    return createElement("div", [dupNode, dupNode]);
  }
});
```

…因为数组中有 2 个`dupNode`。

相反，我们应该写:

```
Vue.component("text-block", {
  render(createElement) {
    return createElement(
      "div",
      Array.apply(null, { length: 5 }).map(() => {
        return createElement("p", "hi");
      })
    );
  }
});
```

多次调用`createElement`以重复创建具有相同内容的新 VNodes。

# 用普通 JavaScript 替换模板特性

我们可以用`if`语句代替`v-if`，用`map`语句代替`v-for`。

例如，我们可以如下创建一个`person`组件:

`src/index.js:`

```
Vue.component("persons", {
  props: ["persons"],
  render(createElement) {
    if (this.persons.length) {
      return createElement(
        "ul",
        this.persons.map(p => {
          return createElement("li", p);
        })
      );
    } else {
      return createElement("p", "Persons list is empty");
    }
  }
});new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Jane", "Mary"]
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
      <persons :persons="persons"></persons>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到一个呈现的`persons`列表。

这与以下内容相同:

`src/index.js`:

```
Vue.component("persons", {
  props: ["persons"],
  template: `    
    <ul v-if='persons.length'>
      <li v-for='p of persons'>{{p}}</li>
    </ul>
    <p v-else>Persons list is empty</p>
  `
});new Vue({
  el: "#app",
  data: {
    persons: ["Joe", "Jane", "Mary"]
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
      <persons :persons="persons"></persons>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

# v 型车

渲染函数中没有与`v-model`等价的函数。我们可以实现如下等价逻辑:

`src/index.js`:

```
Vue.component("custom-input", {
  props: ["value"],
  render(createElement) {
    return createElement("input", {
      domProps: {
        value: this.value
      },
      on: {
        input: event => {
          this.$emit("input", event.target.value);
        }
      }
    });
  }
});new Vue({
  el: "#app",
  data: {
    msg: ""
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
      <custom-input v-model="msg"></custom-input>
      {{msg}}
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

在上面的代码中，我们在我们的`custom-input`组件中发出`input`事件，并获取`value`道具。这个和`v-model`一样。

因此，我们可以用`v-model`绑定到父模型，当我们在输入中输入内容时，输入的数据会显示在输入下方。

![](img/60b983bcdc546d97415f394e95d001bf.png)

Photo by [Lucas Franco](https://unsplash.com/@lucasfranco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 事件和按键修饰符

要添加`.passive`、`.capture`和`.once`事件修饰符，我们可以在事件处理程序前面加上以下前缀，将修饰符添加到我们的事件处理程序中:

*   `.passive` —前缀为`&`
*   `.capture` —前缀为`!`
*   `.once` —前缀为`~`
*   `.capture.once`或`.once.capture` —前缀为`~!`

例如，我们可以写:

```
on: {
  '!click': this.doThisInCapturingMode,
  '~keyup': this.doThisOnce,
  '~!mouseover': this.doThisOnceInCapturingMode
}
```

其他修饰符不需要前缀，因为它们在普通 JavaScript 中有对应的内容。它们是:

*   `.stop` —使用`event.stopPropagation()`
*   `.prevent` —使用`event.preventDefault()`
*   `.self` —使用`if (event.target !== event.currentTarget) return`
*   按键——例如`.enter`或`.13` —使用`if (event.keyCode !== 13) { //... }`
*   修改键—例如`.ctrl`、`.alt`、`.shift`、`.meta` —使用`if (!event.ctrlKey) { //... }`

例如，我们可以如下添加一个`keyup`事件处理程序:

```
Vue.component("custom-input", {
  props: ["value"],
  render(createElement) {
    return createElement("input", {
      domProps: {
        value: this.value
      },
      on: {
        keyup: event => {
          event.preventDefault();
          this.$emit("input", event.target.value);
        }
      }
    });
  }
});
```

然后，当释放键盘按键时，会发出`input`事件。

我们可以使用带有特殊前缀的修饰语，如下所示:

`src/index.js`:

```
Vue.component("custom-button", {
  render(createElement) {
    return createElement("button", {
      domProps: {
        innerHTML: "Click Me",
        id: "child"
      },
      on: {
        "!click": event => {
          alert(`${event.target.id} clicked`);
        }
      }
    });
  }
});new Vue({
  el: "#app",
  methods: {
    onClick() {
      alert(`parent clicked.`);
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
      <custom-button></custom-button>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

然后我们得到显示的`child clicked`警告弹出窗口，因为我们在点击时使用了`capture`修饰符。

# 结论

如果我们想用`v-if`和`v-for`写组件但是使用渲染函数，我们可以分别用`if`语句和数组的`map`方法来代替。

对于添加 Vue 事件处理程序，我们可以使用特殊的前缀为其中一些添加修饰符，使用普通的 JavaScript 为另外一些添加修饰符。

此外，我们可以在子 VNodes 的数组中添加重复的元素。

## **用简单英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，**用你的中级用户名发邮件到**[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**