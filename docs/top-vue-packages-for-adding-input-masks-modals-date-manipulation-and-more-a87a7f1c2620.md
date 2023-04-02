# 用于添加输入掩码、模态、日期操作等的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-input-masks-modals-date-manipulation-and-more-a87a7f1c2620?source=collection_archive---------10----------------------->

![](img/c17cef406841f10d661cb10692ec587d.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加输入掩码、模态、操作日期和自动完成下拉菜单的最佳包。

# Vue IMask 插件

Vue IMask Plugin 是一个 Vue 插件，它在我们的应用程序中添加了一个输入掩码。

我们通过运行以下命令来安装它:

```
npm i vue-imask
```

然后我们写道:

```
<template>
  <imask-input
    v-model="numberModel"
    :mask="Number"
    radix="."
    @accept="onAccept"
    placeholder="Enter number"
  />
</template>

<script>
import { IMaskComponent } from "vue-imask";export default {
  data() {
    return {
      numberModel: "",
      onAccept(value) {
        console.log(value);
      }
    };
  },
  components: {
    "imask-input": IMaskComponent
  }
};
</script>
```

添加带有掩码的输入。

当输入有效时，它发出`accept`事件。

`mask`设置格式。

在我们的例子中，它是一个数字。

`v-model`将值绑定到模型状态。

`placeholder`是占位符。

`radix`是小数点分隔符。

@trevoreyre/autocomplete-vue 是 vue 应用程序的一个易于使用的自动建议组件。

为了使用它，我们运行:

```
npm i @trevoreyre/autocomplete-vue
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import Autocomplete from "@trevoreyre/autocomplete-vue";
import "@trevoreyre/autocomplete-vue/dist/style.css";Vue.use(Autocomplete);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <autocomplete :search="search"></autocomplete>
  </div>
</template><script>
export default {
  data() {
    return {
      fruits: ["apple", "orange", "grape", "banana"]
    };
  },
  methods: {
    search(input) {
      if (input.length < 1) {
        return [];
      }
      return this.fruits.filter(f => {
        return f.toLowerCase().startsWith(input.toLowerCase());
      });
    }
  }
};
</script>
```

去使用它。

我们注册插件并导入 CSS。

然后我们添加`autocomplete`组件来添加自动完成输入。

`search` prop 采用一个函数，让我们从一个输入值中搜索一个项目。

我们将`search`方法作为值传递。

`input`有输入值。

它返回匹配项的数组。

该函数还可以返回一个承诺。

我们也可以谴责回调，改变基类，并设置默认值。

# vue 日期 fns

vue-date-fns 是 vue-date-fns 库的一个 vue 包装器。

要使用它，我们首先通过运行以下命令来安装它:

```
npm i vue-date-fns
```

然后我们可以通过写来使用它:

```
<template>
  <div id="app">
    <div>{{ new Date() | date }}</div>
  </div>
</template><script>
import { dateFilter } from "vue-date-fns";export default {
  filters: {
    date: dateFilter
  }
};
</script>
```

我们注册并使用过滤器。

`date`将日期格式化为日期字符串。

我们也可以使用`$date`方法来操作日期。

它还带有地区数据，因此我们可以切换到不同的地区。

我们还可以更改日期格式:

```
<template>
  <div id="app">
    <div>{{ new Date() | date('dd MMMM yyyy') }}</div>
  </div>
</template><script>
import { dateFilter } from "vue-date-fns";export default {
  filters: {
    date: dateFilter
  }
};
</script>
```

我们只是向过滤函数传递一个参数。

# Vue 我的下拉组件

Vue 我的下拉组件是一个易于使用的下拉组件。

为了使用它，我们运行:

```
npm i vue-my-dropdown
```

来安装它。

然后我们写道:

```
<template>
  <div id="app">
    <dropdown :visible="visible">
      <span class="link" @click="visible = !visible">click here</span>
      <div slot="dropdown" class="dialog">hello</div>
    </dropdown>
  </div>
</template><script>
import dropdown from "vue-my-dropdown";export default {
  components: { dropdown },
  data() {
    return {
      visible: false
    };
  }
};
</script>
```

去使用它。

它没有任何样式，所以我们必须自己添加。

我们填充`dropdown`槽的内容来填充下拉列表。

我们可以设置位置，添加动画，等等。

# Vuedals

Vuedals 是 Vue 应用的模态组件。

为了使用它，我们运行:

```
npm i vuedals
```

来安装它。

然后我们可以写:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import { default as Vuedals } from "vuedals";Vue.use(Vuedals);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div id="app">
    <button @click="showModal()">show modal</button>
    <vuedal></vuedal>
  </div>
</template><script>
import {
  default as Vuedals,
  Component as Vuedal,
  Bus as VuedalsBus
} from "vuedals";export default {
  components: {
    Vuedal
  },
  methods: {
    showModal() {
      VuedalsBus.$emit("new", {
        name: "modal",
        component: {
          name: "modal",
          template: `
            <div>
              <p>modal</p>
            </div>
          `
        }
      });
    }
  }
};
</script>
```

去使用它。

我们调用`Vuedals.$emit`方法来打开模态。

内容由一个组件填充。

![](img/23bb2fa341e56006aa72e940cf043c1b.png)

Photo by [Raphael](https://unsplash.com/@raphrecs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Vue IMask 插件是一个简单的输入屏蔽组件。

@trevoreyre/autocomplete-vue 是 vue 应用程序的自动完成组件。

vue-date-fns 是 vue 应用程序的日期-fns 的包装器。

Vue my dropdown 组件是一个我们可以自定义的下拉列表。

Vuedals 是一个简单的模态。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**