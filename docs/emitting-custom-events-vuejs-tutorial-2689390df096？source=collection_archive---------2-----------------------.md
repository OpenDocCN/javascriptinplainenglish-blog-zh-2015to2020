# 发射组件自定义事件— VueJS 教程

> 原文：<https://javascript.plainenglish.io/emitting-custom-events-vuejs-tutorial-2689390df096?source=collection_archive---------2----------------------->

![](img/e592cb72d822711558acc0b5830ccd53.png)

Photo by [Fatos Bytyqi](https://unsplash.com/@fatosi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何 Vue 应用程序的一个重要部分是充分利用 Vue 的反应能力在应用程序中传递数据。然而，当涉及到处理多个组件、继承和开发中经常出现的一系列其他问题时，有时这可能会有点混乱。

希望这篇快速的文章涵盖了所有的基础，让你顺利地成为 Vue.js 中自定义事件的大师。

# 为什么发出事件

您可能会问自己，“我什么时候需要这样做？”。我不怪你。当你不知道教程和你的项目有什么关系时，你很难去浏览它。

当使用 Vue 的最佳实践并试图创建模块化组件时，发出事件非常有用。如果您希望父组件能够从其子组件接收数据，一个很好的方法是使用 VueJS 自定义事件。

例如，假设您有一个 popup 组件，它的可见性由父组件控制。如果我们想通过点击退出按钮来关闭弹出窗口，我们可以从子进程向父进程发出一个事件，告诉它隐藏弹出窗口。

在更复杂的用例中，这项基本技能非常重要，但是一旦你掌握了基础知识，就很容易看到它如何应用到你的项目中。

# 我们该怎么做呢？

回到前面的弹出组件的例子，假设我们有以下两个组件。

这可能是一个验证表单、登录弹出窗口，真的是任何东西

```
<template>
   <div class='popup'>
      This is the popup
      <div>Close</div>
   </div>
</template>
<script>
</script> 
```

`Page.vue` —这只是本例的父容器

```
<template>
   <div>
      <popup v-show='popupOpen'></popup>
      Hello World
  </div>
</template><script>
import Popup from '.Popup.vue'export default {
   components: {
      Popup
   },
   data () {
      popupOpen: false
   }
}</script> 
```

如你所见，当`popupOpen`变量为真时，弹出窗口仅在`Page.vue`上可见——然而，如果我们想通过点击按钮来关闭弹出窗口，将该信息传回`Page.vue`可能会有点棘手，这是发出自定义事件的地方。

我们要做的就是添加几行。

在`Popup.vue` —我们要添加一个方法，在按钮被点击时调用它。该方法将发出事件。

```
<template>
   <div class='popup'>
      This is the popup
      <div @click='handleClick'>Close</div>
   </div>
</template>
<script>
export default {
   methods: {
      handleClick: function () {
         // can put fancier logic here, but we'll be simple
         this.$emit('close')
      }
   }
}
</script>
```

然后，在`Page.vue`中，我们将不得不监听这个事件并相应地处理它。这可以通过使用`v-on` 修改器修改一行来完成。

```
<popup v-show='popupOpen' v-on:close='popupOpen = false'></popup>
```

# 变得有点花哨了

当然，这个例子非常简单，甚至似乎不言自明。但是使用 VueJS 自定义事件可以实现更高级的功能。

发出 VueJS 事件的一个常见用途是创建属性之间的双向绑定。幸运的是，VueJS 知道这一点，并为此创造了一种速记方法。

直接从 Vue docs。我们有下面这个例子。

```
this.$emit('update:title', newTitle)
```

然后在父母那里，我们倾听。

```
<text-document   
   v-bind:title="doc.title"   
   v-on:update:title="doc.title = $event" >
</text-document>
```

然而，聪明的 Vue 团队添加了一个简写，将这个命令简化为一行。

```
<text-document v-bind.sync="doc"></text-document>
```

这是一个将数据传递回父组件的很好的例子，通过用第二个参数调用`this.$emit()`，它被传递回父组件，在变量名`$event`下。这很有用，因为您可以传回对象或您想要的任何其他数据。

# 进一步阅读

和往常一样，我非常喜欢使用 [VueJS 文档](https://vuejs.org/v2/guide/components-custom-events.html)——这是任何编程框架中最好的书面文档。这绝对是我查找更多细节以及代码语法和选项的来源。

# 就是这样。

和五个快速的要点——就是这样！这应该足以让你开始使用 Vue 的一个很少使用的特性。我希望这能对你有所帮助，如果你有任何问题、意见、顾虑，或者只是想打声招呼，请给我回复或者发邮件到 matt@learnvue.co

编程快乐！

如果你有兴趣了解更多关于 VueJS 的知识，[点击这里获得一份免费的 Vue 备忘单](https://expert-painter-2502.ck.page/f4a74ed5e2)，它是每个 Vue 开发者都需要知道的基本知识。