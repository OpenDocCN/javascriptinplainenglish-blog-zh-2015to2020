# 常见的 Vue 问题—数据共享、模板和图像

> 原文：<https://javascript.plainenglish.io/common-vue-problems-data-sharing-templates-and-images-5a1d9c4a9a03?source=collection_archive---------9----------------------->

![](img/e56a002afaf9b174509ae4cbb61e46fc.png)

Photo by [frank mckenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 在组件之间共享数据

我们可以用 Vuex 在组件之间共享数据。

我们可以使用`mapMutation`将 Vuex 存储中的突变添加到我们的组件中。

`mapState`可以映射状态。

# 在列表中显示

如果我们想在列表中使用`v-show`，我们可以把列表项放在一个组件中。

例如，我们可以写:

```
<post v-for="post in posts" :post="post" :key="post.id"></post>
```

然后`post`可以有`v-show`代码。

这样，我们就不会在所有列表项中同时打开和关闭所有列表项。

我们还可以计算`show`属性来显示我们想要的元素。

例如，我们可以写:

```
Vue.component('post', {
  template: '#post-template',
  props: {
    post: Object,
    selectedId: Number,
  },
  computed: {
    show() {
      return this.post.id === this.selectedId
    },
  },
})
```

然后我们可以传入`selectedId`属性来设置要显示的项目的 ID。

我们可以通过编写以下代码来使用这个`post`组件:

```
<post :selected-id="selectedId" :post="post" @click="selectedId = post.id"></post>
```

# 遍历静态文件目录中的资源

我们可以通过设置基本路径来遍历静态文件目录中的资产。

然后，我们可以将基本路径连接到静态文件的相对路径。

例如，我们可以写:

```
<template lang="html">
  <div>
     <div :key="key" v-for="(img, key) in images" >
       <img :src="`imageDir${key}`" :key="key" >
     </div>
   </div>
</template><script>
export default {
  data() {
    return {
      imageDir: "/path/to/images/", 
      images: {}
    }
  },
  ...
}
```

我们在一个对象中有`images`，键有相对路径。

因此，我们可以通过将`img`值作为第一个参数并将`key`作为`v-for`的第二个参数来使用`v-for`循环遍历这些键。

然后我们可以通过访问这些键来循环访问这些键。

# 用 Vue Konva 显示图像

我们可以使用 Vue Konva 的`v-image`组件来显示图像。

例如，我们可以在模板上写下以下内容:

```
...
<v-stage ref="stage" :config="configKonva">
  <v-layer ref="layer">
    <v-image :config="configImg"></v-image>
  </v-layer>
</v-stage>
...
```

我们有了 stage 和 layer，我们可以用它们来填充带有`v-image`的图像。

然后在组件代码中，我们可以写:

```
export default {
  data() {
    return {
      img: new Image(50, 50),
      configKonva: {
        width: 200,
        height: 200
      }
    }
  },
  computed: {
    configImg() {
      return {
        x: 20,
        y: 20,
        image: this.img,
        width: 200,
        height: 200,
      }
    }
  },
  mounted() {
    this.img.src = "http://placekitten.com/200/200"
  }
}
```

我们有一个 config 对象用`x`和`y`设置图像的左上角坐标。

`image`有图像。

`width`和`height`有尺寸。

# 无法从组件访问引用

我们只能访问在 DOM 加载后运行的生命周期钩子中的引用。

因此，我们不能运行在`created`钩子中访问 refs 的代码。

相反，我们可以在`mounted`钩子中引用 refs，因为它是在 DOM 加载之后加载的。

例如，我们可以写:

```
mounted() {
  console.log(this.$refs.canvas);
},
```

然后，如果我们有一个引用为`canvas`的元素，我们可以将它作为`this.$refs`的属性来访问。

# 使用不带 HTML 标签的 v-if 和 v-else

如果我们不想在我们的`v-if`或`v-else`代码中呈现任何 HTML 元素，我们可以在这些指令中使用`tenplate`标签而不是元素标签。

例如，我们可以写:

```
<template v-if="condition">
</template>
<template v-else>
</template>
```

然后，我们在内容之外不呈现任何 HTML 元素。

# v-bind 和双花括号的区别

双花括号，写为`{{}}`，用于模板中的插值。

`v-bind`用于将数据作为道具传递给子组件。

我们不能用插值来传递数据作为道具。

例如，我们必须写:

```
<div v-bind:class="`btn btn-success hint--top ${class}`"></div>
```

或者:

```
<div>{{class}}</div>
```

![](img/7ed29887086c060c62f62b49699108b8.png)

Photo by [Peter Oslanec](https://unsplash.com/@peter_oslanec?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在任何组件之间共享数据的最佳方式是使用 Vuex。

`v-bind`与双花括号不同，它们不能互换使用。

如果我们不想呈现任何包装元素，那么我们可以使用`template`。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**