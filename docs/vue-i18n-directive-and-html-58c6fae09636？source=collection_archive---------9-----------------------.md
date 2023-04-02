# vue-i18n —指令和 HTML

> 原文：<https://javascript.plainenglish.io/vue-i18n-directive-and-html-58c6fae09636?source=collection_archive---------9----------------------->

![](img/3221736829b467b8f3164fd0bb8ec531.png)

Photo by [YUCAR FotoGrafik](https://unsplash.com/@yucar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 vue-i18n 创建多语言 Vue 应用程序。

# 自定义指令本地化

我们可以使用`v-t`指令来获得翻译。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    hello: "hi"
  },
  fr: {
    hello: "bonjour"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <p v-t="'hello'"></p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只需将`v-t`指令放入模板中，就可以得到翻译。

于是`bonjour`就显示在屏幕上。

我们也可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    hello: "hi {name}"
  },
  fr: {
    hello: "bonjour {name}"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <p v-t="{ path: 'hello', args: { name: nickName } }"></p>
  </div>
</template><script>
export default {
  name: "App",
  computed: {
    nickName() {
      return "foo";
    }
  }
};
</script>
```

插值平移。

`path`有消息的路径，`args`有动态参数。

现在我们让`bonjour foo`显示在屏幕上。

# 与过渡一起使用

我们可以在转换中使用`v-t`指令。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    hello: "hi"
  },
  fr: {
    hello: "bonjour"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <button [@click](http://twitter.com/click)="toggle = !toggle">Toggle</button>
    <transition name="fade">
      <p v-t="'hello'" v-if="toggle"></p>
    </transition>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      toggle: true
    };
  }
};
</script><style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
```

由于有了`transition`组件，我们得到了一个过渡效果。

# $t 与 v-t

`$t`更加灵活，可以在组件代码和模板中使用。

然而，它在每次重新渲染时运行，这意味着它更慢。

`v-t`比`$t`有更好的性能，因为有缓存。

也可用于`vue-i18n-extensions`的预翻译。

然而，它不像`$t`那样灵活，而且很复杂。

# 分量插值

要用 HTML 添加翻译，我们可能希望将它们放在我们的标签之间:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    hello: "hi",
    world: "world"
  },
  fr: {
    hello: "bonjour",
    world: "monde"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <p>
      {{ $t('hello') }}
      <b>{{ $t('world') }}</b>
    </p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

但是，这太不灵活了。

我们可以使用`i18n`组件来避免这种情况。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    hello: "hi {0}",
    world: "world"
  },
  fr: {
    hello: "bonjour {0}",
    world: "monde"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <i18n path="hello" tag="p" for="world">
      <b>{{ $t('world') }}</b>
    </i18n>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们用`{0}`占位符将`i18n`中的内容插入到标签中。

然后我们看到第二个词是粗体的。

![](img/998788a051630183b493328e7efd43ab.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以将 HTML 格式的文本放在`i18n`组件中。

同样，我们可以使用`v-t`指令作为`$t`的替代。

它速度更快，并且可以处理过渡。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**