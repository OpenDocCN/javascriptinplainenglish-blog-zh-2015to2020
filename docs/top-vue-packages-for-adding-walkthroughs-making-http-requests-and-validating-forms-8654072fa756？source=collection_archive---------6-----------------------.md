# 用于添加演练、发出 HTTP 请求和验证表单的顶级 Vue 包

> 原文：<https://javascript.plainenglish.io/top-vue-packages-for-adding-walkthroughs-making-http-requests-and-validating-forms-8654072fa756?source=collection_archive---------6----------------------->

![](img/818a9f3fb4b054fd1d8eaea0160425e3.png)

Photo by [Jack Young](https://unsplash.com/@jackyoung?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将研究如何创建添加演练、发出 HTTP 请求和验证表单的最佳包。

# vue-旅游

我们可以使用 vue-tour 为我们的应用程序创建演练。

为了使用它，我们写:

```
npm i vue-tour
```

然后，我们可以通过编写以下内容来注册组件:

```
import Vue from "vue";
import App from "./App.vue";
import VueTour from "vue-tour";require("vue-tour/dist/vue-tour.css");
Vue.use(VueTour);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

我们也进口了这些款式。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <div id="v-step-0">step 0.</div>
    <div class="v-step-1">step 1</div>
    <div data-v-step="2">step 2</div>
    <v-tour name="myTour" :steps="steps"></v-tour>
  </div>
</template>

<script>
export default {
  data() {
    return {
      steps: [
        {
          target: "#v-step-0",
          header: {
            title: "get started"
          },
          content: `hello <strong>world</strong>!`
        },
        {
          target: ".v-step-1",
          content: "step 1"
        },
        {
          target: '[data-v-step="2"]',
          content: "Step 2",
          params: {
            placement: "top"
          }
        }
      ]
    };
  },
  mounted() {
    this.$tours["myTour"].start();
  }
};
</script>
```

模板中有步骤列表。

然后我们有了`steps`数组，其中的步骤指向页面上的元素。

这样，步骤就显示在目标元素附近。

在每个步骤中，我们可以设置标题、内容和步骤的位置。

然后，为了开始这个旅程，我们像以前一样使用页面底部的`start`方法。

# vue-资源

vue-resource 是一个 HTTP 客户端，是一个 vue 插件。

要安装它，我们运行:

```
npm i vue-resource
```

然后我们可以通过写来使用它:

```
import Vue from "vue";
import App from "./App.vue";
const VueResource = require("vue-resource");Vue.use(VueResource);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

注册插件。

现在我们可以访问组件中的`$http`属性:

```
<template>
  <div>{{data.name}}</div>
</template>

<script>
export default {
  data() {
    return {
      data: {}
    };
  },
  async mounted() {
    const { body } = await this.$http.get("https://api.agify.io/?name=michael");
    this.data = body;
  }
};
</script>
```

它是基于承诺的，所以我们可以使用异步和等待。

我们可以通过编写以下代码来设置在整个应用程序中使用的通用选项:

```
Vue.http.options.root = '/root';
Vue.http.headers.common['Authorization'] = 'Basic YXBpOnBhc3N3b3Jk';
```

我们为所有请求设置了根 URL 和随处使用的`Authorization`头。

# vue 形式

vue-form 是一个用于 vue 应用程序的表单验证库。

我们可以通过安装包来使用它:

```
npm i vue-form
```

然后我们可以通过写来使用它:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import VueForm from "vue-form";Vue.use(VueForm);Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <div>
    <vue-form :state="formState" [@submit](http://twitter.com/submit).prevent="onSubmit">
      <validate tag="label">
        <span>Name *</span>
        <input v-model="model.name" required name="name"><field-messages name="name">
          <div>Success!</div>
          <div slot="required">name is required</div>
        </field-messages>
      </validate>
    </vue-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      model: {
        name: ""
      },
      formState: {}
    };
  },
  methods: {
    onSubmit() {
      if (this.formState.$invalid) {
        return;
      }
    }
  }
};
</script>
```

我们在`main.js`中注册了组件。

然后在组件中，我们使用了`vue-form`组件。

`state`属性被设置为`formState`对象，当表单状态改变时，它将被更新。

`submit.prevent`具有调用了`e.preventDefault`的提交处理程序。

`v-model`绑定到模型。

`field-messages`组件显示表单验证消息。

`required`表示需要。

在`onSubmit`方法中，我们检查`formState.$invalid`来检查表单是否无效。

如果无效，我们不会继续提交。

其他表单状态属性包括`$name`与字段的名称属性值。

`$valid`查看表单或字段是否有效。

`$focused`检查表单或字段是否被聚焦。

`$dirty`查看表单或字段是否被操作过。

它还带有其他验证器，如电子邮件、URL、号码、最小和最大长度等等。

![](img/f8cb52c597d1edcdc33c6e65dad3aae7.png)

Photo by [Heather Lo](https://unsplash.com/@llhheather?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过 vue-tour 包向我们的 Vue 应用程序添加漫游。

vue-resource 是一个基于承诺的 HTTP 客户端，是为 vue 应用程序开发的。

vue-form 是一个有用的 vue 应用程序的表单验证插件。

## **用简单英语写的 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**