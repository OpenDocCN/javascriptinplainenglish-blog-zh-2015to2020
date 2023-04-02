# 用 Vue-矩在 Vue 应用程序中显示时间

> 原文：<https://javascript.plainenglish.io/displaying-time-with-vue-moment-635ce1e57db5?source=collection_archive---------8----------------------->

![](img/3f98a28e96081f0da0c2135f0dd00093.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用程序框架，我们可以用它来开发交互式前端应用程序。

在本文中，我们将看看一个简单的 Vue 包，它用 Vue-矩包在模板中显示时间。

# vue-矩

Vue 矩为我们提供了许多过滤器来显示时间。

要使用它，我们首先通过运行以下命令安装它:

```
npm i vue-moment
```

然后我们可以通过书写来使用它:

```
import Vue from "vue";
import App from "./App.vue";Vue.config.productionTip = false;Vue.use(require("vue-moment"));new Vue({
  render: h => h(App)
}).$mount("#app");
```

然后，我们可以通过编写以下内容来使用内置过滤器:

```
<template>
  <div id="app">
    <span>{{ new Date() | moment("dddd, MMMM Do YYYY") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们只需要使用`moment`过滤器，就可以得到显示的日期。

此外，我们可以传递选项，根据一个以上的模式来解析日期。

例如，我们可以写:

```
<template>
  <div id="app">
    <span>{{ [ '01.01.20', ["MM.DD.YY", "MM-DD-YY", "MM-DD-YYYY"] ] | moment("dddd, MMMM Do YYYY") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

由于我们的日期字符串与第一个模式匹配，因此将使用过滤器对其进行解析和格式化。

我们也可以用它来计算从现在开始的时间。

例如，我们可以写:

```
<template>
  <div id="app">
    <span>{{ new Date(2020, 10, 1) | moment("from", "now") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们得到一个字符串，像是:

```
in 5 months
```

显示在屏幕上。

`moment(“from”, “now”)`代码计算从现在到该日期的时间。

我们也可以写`moment(“from”)`来分类。

此外，我们还可以通过以下方式计算给定日期的日期:

```
<template>
  <div id="app">
    <span>{{ new Date(2020, 10, 1) | moment("from", "1990-01-01") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们得到:

```
in 31 years
```

显示。

为了在时间跨度中隐藏“in”或“ago ”,我们可以输入`true`:

```
<template>
  <div id="app">
    <span>{{ new Date(2020, 10, 1) | moment("from", "1990-01-01", true) }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在我们得到了:

```
31 years
```

在屏幕上。

它还可以显示日历日期。

例如，我们可以写:

```
<template>
  <div id="app">
    <span>{{ new Date(2020, 10, 1) | moment("calendar") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们得到:

```
11/01/2020
```

我们还可以显示从日期派生的内容。

例如，我们可以写:

```
<template>
  <div id="app">
    <span>{{ new Date() | moment('add', '6 days', 'calendar', null, { nextWeek: '[Happens in a week]' }) }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们在当前日期基础上再增加 6 天。

我们会在下周用`'Happens in a week'`替换任何一个日期。

因此，这就是我们得到的。

我们也可以用`subtract`减去天数:

```
<template>
  <div id="app">
    <span>{{ new Date() | moment("subtract", "6 hours") }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

此外，我们还可以通过`duration`过滤器显示持续时间:

```
<template>
  <div id="app">
    <span>{{ 3600000 | duration('humanize') }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

它可以是 ISO8601 格式的字符串。包含数字和单位的数字或数组。

所以我们也可以写:

```
<template>
  <div id="app">
    <span>{{ [30, 'days'] | duration('humanize', true) }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

它采用了`'humanize'`以外的选项，我们可以写:

```
<template>
  <div id="app">
    <span>{{ [30, 'days'] | duration('asDays') }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们可以将多个过滤器链接在一起。

所以我们可以写道:

```
<template>
  <div id="app">
    <span>{{ [7, 'minutes'] | duration('subtract', 120000) | duration('humanize', true) }}</span>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们从 5 分钟减去 120000 毫秒，并以人性化的形式显示出来。

所以我们显示`in 5 minutes`。

![](img/f2bd278332ae6f172c25f38ab0d048f4.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-moment 包允许我们以各种方式显示时间跨度。

我们可以直接从内置的过滤器计算时间。

它还支持多种格式的时间和日期格式。

## 简单英语中的 JavaScript

你知道我们有四个出版物和一个 YouTube 频道吗？全部在 [**找到。io**](https://plainenglish.io/) 和 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**