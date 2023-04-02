# 用于处理事件、添加进度条和截断文本的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-handling-events-adding-progress-bars-and-truncate-text-56409a44f71c?source=collection_archive---------8----------------------->

![](img/e734b2118eb179b225dbcf795f060095.png)

Photo by [frank mckenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看处理事件、添加实用方法、进度条和截断文本的最佳包。

# vue 事件

vue-events 是一个发送和接收事件的简单事件。

要安装它，我们运行:

```
npm i vue-events
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueEvents from "vue-events";
Vue.use(VueEvents);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div></div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      eventData: { foo: "bar" }
    };
  },
  created() {
    this.$events.fire("event", this.eventData);
    this.$events.emit("event", this.eventData);
    this.$events.$emit("event", this.eventData);
  },
  mounted() {
    this.$events.on("event", eventData => console.log(eventData));
  }, beforeDestroy() {
    this.$events.$off("event");
    this.$events.off("event");
    this.$events.remove("event");
  }
};
</script>
```

我们注册了`VueEvents`插件。

然后我们用第二个参数中的一些数据发送带有`fire`、`emit`或`$emit`的事件。

我们用`on`听事件。

我们用`$off`、`off`或`remove`清除事件监听器。

# vue-下划线

我们可以使用 vue-下划线给 Vue 应用添加下划线。

为了使用它，我们运行:

```
npm i vue-underscore
```

来安装它。

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import underscore from "vue-underscore";
Vue.use(underscore);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div></div>
</template>

<script>
export default {
  mounted() {
    const arr = [{ id: 1 }, { id: 2 }];
    const found = this.$_.findWhere(arr, { id: 1 });
    console.log(found);
  }
};
</script>
```

一旦我们注册了插件，我们就可以使用`this.$_`属性来访问它的方法。

我们也可以直接访问图书馆:

```
<template>
  <div></div>
</template>

<script>
import { _ } from "vue-underscore";export default {
  mounted() {
    const arr = [{ id: 1 }, { id: 2 }];
    const found = _.findWhere(arr, { id: 1 });
    console.log(found);
  }
};
</script>
```

# Vue 屏蔽输入

Vue 屏蔽输入是 Vue 应用程序的屏蔽输入组件。

要安装它，我们运行:

```
npm i vue-masked-input
```

然后我们可以通过写来使用它:

```
<template>
  <div>
    <masked-input v-model="date" mask="11/11/1111" placeholder="dd/mm/yyyy"/>
  </div>
</template>

<script>
import MaskedInput from "vue-masked-input";export default {
  data() {
    return {
      date: ""
    };
  },
  components: {
    MaskedInput
  }
};
</script>
```

我们注册并使用了`masked-input`组件。

它可以用`v-model`将输入值绑定到一个状态。

此外，输入格式受到`mask`属性的限制。

它必须是相同的格式。

除了数字之外，掩码还可以包含字母或其他字符。

# vue-顶部-进度

vue-top-progress 是 vue 应用程序的进度条组件。

要安装它，我们运行:

```
npm i vue-top-progress
```

然后我们写道:

```
<template>
  <div>
    <vue-topprogress ref="topProgress"></vue-topprogress>
  </div>
</template>

<script>
import { vueTopprogress } from "vue-top-progress";export default {
  mounted() {
    this.$refs.topProgress.start(); setTimeout(() => {
      this.$refs.topProgress.done();
    }, 2000);
  }, components: {
    vueTopprogress
  }
};
</script>
```

去使用它。

我们添加了`vue-topprogress`组件。

ref 被设置为`topProgress`，所以我们可以调用`start`来显示进度条。

我们调用`done`让它消失。

颜色和速度可以改变。

# Vue 线夹

Vue Line Clamp 是一个让我们截断文本的指令。

要安装它，我们运行:

```
npm i vue-line-clamp
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import lineClamp from "vue-line-clamp";Vue.use(lineClamp, {});
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <p
      v-line-clamp:20="2"
    >Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse sit amet tortor vulputate, faucibus nulla eu, fringilla ligula. Nunc et aliquet justo. Nulla sit amet risus eu metus volutpat tincidunt. Pellentesque vehicula, erat eu dignissim maximus, diam leo egestas massa, non tincidunt arcu quam placerat eros. Nullam at nunc id ante cursus dignissim non ac libero. Praesent posuere, velit ut varius feugiat, arcu enim sollicitudin odio, eu sagittis dolor massa eget urna. Pellentesque in faucibus arcu, non dignissim arcu. Integer porta sodales tortor sed cursus. Suspendisse at finibus urna. Sed id venenatis ex. Nunc quis dictum velit, a hendrerit enim. Phasellus interdum, tellus quis congue fringilla, tortor sem maximus ante, vel tempus lorem risus nec est. Proin ullamcorper non felis sed gravida. In feugiat laoreet tellus, eget dictum lectus laoreet in. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Vestibulum in blandit metus.</p>
  </div>
</template>

<script>
export default {};
</script>
```

我们注册了`lineClamp`插件。

然后我们使用`v-line-clamp`指令来截断文本。

![](img/535f13bb2e2b394b32e840b4ec3b929f.png)

Photo by [Adrian Pereira](https://unsplash.com/@adrianluisp10?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

vue-events 让我们发出并监听自定义事件。

vue-下划线让我们可以在应用程序中加入下划线。

vue-top-progress 让我们显示一个进度条。

Vue Line Clamp 让我们截断文本。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**