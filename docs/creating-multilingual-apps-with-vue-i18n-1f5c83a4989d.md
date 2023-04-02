# 使用 vue-i18n 创建多语言应用

> 原文：<https://javascript.plainenglish.io/creating-multilingual-apps-with-vue-i18n-1f5c83a4989d?source=collection_archive---------8----------------------->

![](img/2488e8fb8ab68701154d62b95eb39b8a.png)

Photo by [Ignacio Brosa](https://unsplash.com/@ignaciobrosa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将了解如何使用 vue-i18n 创建多语言 Vue 应用程序。

# 入门指南

首先，我们通过编写以下内容来安装该软件包:

```
npm i vue-i18n
```

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);const messages = {
  en: {
    message: {
      hello: "hello"
    }
  },
  fr: {
    message: {
      hello: "bonjour"
    }
  }
};const i18n = new VueI18n({
  locale: "fr",
  messages
});Vue.config.productionTip = false;
new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

我们可以通过书写来显示译文:

```
<template>
  <div id="app">
    <p>{{ $t("message.hello") }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

# 格式化

我们可以通过写下以下内容在我们的信息中插入表达:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);const messages = {
  en: {
    message: {
      hello: "{msg} world"
    }
  },
  fr: {
    message: {
      hello: "bonjour"
    }
  }
};const i18n = new VueI18n({
  locale: "en",
  messages
});Vue.config.productionTip = false;
new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

在我们的组件中，我们可以写:

```
<template>
  <div id="app">
    <p>{{ $t('message.hello', { msg: 'hello' })  }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们让`hello world`显示在屏幕上。

# 列表格式

我们可以用 vue-i18n 格式化列表。

例如，我们可以写:

```
const messages = {
  en: {
    message: {
      hello: "{0} world"
    }
  },
  fr: {
    message: {
      hello: "bonjour"
    }
  }
};
```

我们可以写:

```
<template>
  <div id="app">
    <p>{{ $t('message.hello', ['hi']) }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

来显示`hello world`。

我们也可以使用类似数组的对象来代替数组:

```
<template>
  <div id="app">
    <p>{{ $t('message.hello', {'0': 'hi'}) }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们得到了同样的结果。

# Ruby on Rails 消息格式

占位符也可以用 Ruby on Rails 格式编写。

例如，我们可以写:

```
const messages = {
  en: {
    message: {
      hello: "%{msg} world"
    }
  },
  fr: {
    message: {
      hello: "bonjour"
    }
  }
};
```

我们写道:

```
<template>
  <div id="app">
    <p>{{ $t('message.hello', {msg: 'hi'}) }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

# 自定义格式化程序

我们可以添加一个自定义格式化程序类:

```
class CustomFormatter {
  constructor (options) {
    // ...
  } interpolate (message, values) {
    return ['resolved message string']
  }
}
```

然后我们可以在传递给`VueI18n`构造函数的消息中设置它:

```
const i18n = new VueI18n({
  locale: 'en',
  formatter: new CustomFormatter(),
  messages: {
    en: {
      // ...
    },
    // ...
  }
})
```

我们只需设置传递给构造函数的对象的`formatter`属性。

# 多元化

vue-18n 支持多元化。

为了将单个和多个单词添加到`messages`，我们将单词添加到由`|`字符分隔的字符串中。

例如，我们可以写:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);const messages = {
  en: {
    apple: "apple | apples",
    orange: "no oranges | one orange | {count} oranges"
  }
};const i18n = new VueI18n({
  locale: "en",
  messages
});Vue.config.productionTip = false;
new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

然后，我们在组件中编写以下内容:

```
<template>
  <div id="app">
    <p>{{ $tc('apple', 1) }}</p>
    <p>{{ $tc('apple', 2) }}</p> <p>{{ $tc('orange', 0) }}</p>
    <p>{{ $tc('orange', 1) }}</p>
    <p>{{ $tc('orange', 5, { count: 5 }) }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们得到了:

```
appleapplesno orangesone orange5 oranges
```

在屏幕上。

对于常规翻译，我们使用了`$tc`函数而不是`$t`函数。

占位符在大括号中。

# 日期时间本地化

日期和时间也可以本地化，我们只需传入一个带有我们想要显示的日期格式的对象。

为此，我们写道:

```
import Vue from "vue";
import App from "./App.vue";
import VueI18n from "vue-i18n";Vue.use(VueI18n);const dateTimeFormats = {
  en: {
    short: {
      year: "numeric",
      month: "short",
      day: "numeric"
    },
    long: {
      year: "numeric",
      month: "short",
      day: "numeric",
      weekday: "short",
      hour: "numeric",
      minute: "numeric"
    }
  }
};const i18n = new VueI18n({
  locale: "en",
  dateTimeFormats
});Vue.config.productionTip = false;
new Vue({
  i18n,
  render: h => h(App)
}).$mount("#app");
```

那么我们可以如下使用`$d`功能:

```
<template>
  <div id="app">
    <p>{{ $d(new Date(), 'short') }}</p>
    <p>{{ $d(new Date(), 'long', 'en') }}</p>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们传入`'long'`和`'short'`格式字符串和可选的语言。

![](img/7eb11db5e2254a16ae5c71645a976919.png)

Photo by [Pagie Page](https://unsplash.com/@pagie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 vue-i18n 来格式化本地化我们的应用程序。

可以为不同的地区添加文本、单复数单词和日期。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**