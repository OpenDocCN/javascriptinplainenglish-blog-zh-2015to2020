# Vue 3 —渲染函数事件和插件

> 原文：<https://javascript.plainenglish.io/vue-3-render-functions-events-and-plugins-afddb194ec73?source=collection_archive---------10----------------------->

![](img/8dfc03a30866ed2544f53e8b8286a946.png)

Photo by [Jakob Dalbjörn](https://unsplash.com/@jakobdalbjorn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在这篇文章中，我们将看看如何用 Vue 3 创建渲染函数和插件。

# 事件修饰符等效项

事件修饰符有以下等效项。

它们是:

*   `.stop` — `event.stopPropagation()`
*   `.prevent`——`event.preventDefault()`
*   `.self`——`if (event.target !== event.currentTarget) return`
*   `.enter`或`.13` — `if (event.keyCode !== 13) return`
*   `.ctrl`、`.alt`、`.shift`或`.meta` — `if (!event.ctrlKey) return`

例如，我们可以这样使用它:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
  </head>
  <body>
    <div id="app">
      <custom-input></custom-input>
    </div>
    <script>
      const app = Vue.createApp({}); app.component("custom-input", {
        render() {
          return Vue.h("input", {
            onKeyUp: event => {
              if (event.target !== event.currentTarget) return;
              if (!event.shiftKey || event.keyCode !== 23) return;
              event.stopPropagation();
              event.preventDefault();
              //...
            }
          });
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

我们可以调用普通的 JavaScript 事件方法并检查它们的属性来添加等价的修饰符。

# 时间

`this.$slots`允许我们向我们的 Vue 应用添加插槽。

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
      <custom-heading :level="1">foo</custom-heading>
    </div>
    <script>
      const app = Vue.createApp({});
      app.component("custom-heading", {
        render() {
          const { h } = Vue;
          return h(`h1`, {}, this.$slots.default());
        },
        props: {
          level: {
            type: Number,
            required: true
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

添加或自定义内部带有插槽的 h1 元素。

`this.$slots.default`是默认插槽。

# JSX

如果我们安装了这个[巴别塔插件](https://github.com/vuejs/jsx)，我们也可以在渲染函数中使用 JSX。

这样，我们可以在渲染函数中使用更方便的 JSX 语法。

例如，我们可以写:

```
import AnchoredHeading from './AnchoredHeading.vue'app.component("custom-heading", {
  render() {
    return (
      <AnchoredHeading level={1}>
        <span>Hello</span> world!
      </AnchoredHeading>
      );
    }
});
```

在我们的渲染函数中编写 JSX 而不是普通的 JavaScript。

# 插件

插件是一个可以重用的自包含代码。

他们在 Vue 中添加了全局级别的功能。

他们可以添加任何东西，包括全局方法或属性。

它还可以包括全球资产和混合资产。

实例方法也可以添加插件。

我们可以通过用`install`方法导出一个对象来添加插件:

```
export default {
  install: (app, options) => {
    //...
  }
}
```

然后，我们可以通过编写以下内容来添加我们自己的全局属性:

```
export default {
  install: (app, options) => {
    app.config.globalProperties.$foo = (key) => {
      return 'foo';
    }
  }
}
```

我们也可以在一个插件中注入其他插件。

例如，我们可以写:

```
export default {
  install: (app, options) => {
    app.config.globalProperties.$foo = (key) => {
      return 'foo';
    } app.provide('i18n', options);
  }
}
```

我们可以用`app.directive`和`app.mixin`方法将全局混合和指令直接添加到插件代码中:

```
export default {
  install: (app, options) => {
    app.config.globalProperties.$foo = (key) => {
      return 'foo';
    } app.provide('i18n', options) app.directive('my-directive', {
      bind (el, binding, vnode, oldVnode) {
        // some logic ...
      }
      ...
    }) app.mixin({
      created() {
        //...
      }
      //...
    })
  }
}
```

要使用插件，我们调用`app.use`方法:

```
app.use(myPlugin, options)
```

![](img/7aa5b58f320ac3ab41619f7d554c7091.png)

Photo by [WebFactory Ltd](https://unsplash.com/@webfactoryltd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

渲染函数中有相当于指令事件修饰符的东西。

如果我们添加一个插件，渲染函数也可以包含 JSX。

我们可以用各种方法调用来创建插件。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **！**