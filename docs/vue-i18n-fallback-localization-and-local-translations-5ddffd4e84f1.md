# vue-i18n —回退本地化和本地翻译

> 原文：<https://javascript.plainenglish.io/vue-i18n-fallback-localization-and-local-translations-5ddffd4e84f1?source=collection_archive---------4----------------------->

![](img/8135931091723eecc12a61ac47d4b1f4.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 vue-i18n 创建多语言 Vue 应用程序。

# 后备本地化

如果字符串不可用，我们可以设置一个`fallbackLocale`。

vue-18n 具有隐式回退区域设置。

一个更具体的区域设置会退回到同类的更一般的区域设置。

例如，`de-DE`会退回到`de`。

我们还可以使用`fallbackLocale`属性显式地指定后备语言环境。

例如，我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    foo: "hello"
  },
  fr: {}
};const i18n = new VueI18n({
  messages,
  locale: "fr",
  fallbackLocale: "en"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

然后我们写道:

```
<template>
  <div id="app">
    <p>{{ $t('foo') }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们看到显示了`hello`，因为我们的备用区域是`en`。

`fallbackLocale`也可以是一个数组，所以我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    foo: "hello"
  },
  fr: {},
  ja: {}
};const i18n = new VueI18n({
  messages,
  locale: "fr",
  fallbackLocale: ["fr", "en"]
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

它也可以是具有后备区域列表的对象。

键是区域设置，值是它的后备。

例如，我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    foo: "hello"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr",
  fallbackLocale: {
    "de-CH": ["fr", "it"],
    default: ["en", "da"]
  }
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

然后`de-CH`回落到`fr`或`it`。如果这些都不可用，则返回到`en`或`da`。

# 后退插值

后备区域可以有自己的插值。

我们必须把插值放在键里。

例如，我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    "hello {name}": "hello {name}"
  }
};const i18n = new VueI18n({
  messages,
  locale: "fr"
});new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

然后在我们的模板中，我们写道:

```
<template>
  <div id="app">
    <p>{{ $t('hello {name}', { name: 'james' }) }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

# 基于组件的本地化

我们可以只在一个组件中定位。

例如，我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);
Vue.config.productionTip = false;const messages = {
  en: {
    "hello {name}": "hello {name}"
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
    <p>{{ $t('message.welcome') }}</p>
  </div>
</template><script>
export default {
  name: "App",
  i18n: {
    messages: {
      en: { message: { welcome: "welcome" } },
      fr: { message: { welcome: "bienvenu" } }
    }
  }
};
</script>
```

我们在组件中添加了一个`i18n`属性来提供本地翻译。

`messages`属性有翻译。

然后我们像往常一样定义翻译。

由于`locale`被设置为`'fr'`，我们在屏幕上得到`bienvenu`。

这些翻译可以保存在另一个模块中并导入。

例如，我们可以写:

`messages.js`

```
export default {
  en: { message: { welcome: "welcome" } },
  fr: { message: { welcome: "bienvenu" } }
};
```

`App.vue`

```
<template>
  <div id="app">
    <p>{{ $t('message.welcome') }}</p>
  </div>
</template><script>
import messages from "./messages";
export default {
  name: "App",
  i18n: {
    messages
  }
};
</script>
```

我们可以使用`parent.$t`来获得函数组件的翻译。

例如，我们可以写:

```
<div>{{ parent.$t('message.hello') }}</div>
```

在函数组件中获取翻译。

![](img/dc0f83754e1d2c6555e46f84fc5c329a.png)

Photo by [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过各种方式获得翻译。

一种方法是从后备位置获取它们。

我们可以将它们定义为对象、数组或字符串。

此外，我们可以有一些组件专有的翻译。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**