# 如何使用 Vue 过渡改善用户体验

> 原文：<https://javascript.plainenglish.io/how-you-can-use-vue-transitions-right-now-a-beginners-tutorial-71860a555d46?source=collection_archive---------9----------------------->

![](img/f264d27cfc279d6c18de805fb548d287.png)

将转场添加到您的 Vue 应用程序是让您的项目感觉更专业的一种简单方法。通过改善用户体验，你可以让更多的人留在你的网站上，提高你的转化率。

只需要一点点准备就能得到丰厚的回报。

在本指南中，我们将介绍您需要了解的关于 Vue 过渡的知识，从最基本的开箱即用到创建自定义过渡。

准备好开始学习所有关于 Vue 转换的知识了吗？我也是。让我们开始吧。

# 为什么还要使用过渡

虽然大多数人认为过渡只是装饰，一个设计良好的过渡可以…

*   抓住并引导用户的注意力
*   强调重要信息
*   在你的网站上建议一个自然的流程
*   引导用户浏览你的页面
*   帮助创建更专业的品牌形象

所有这些都有助于改善你的网站的用户体验，提高你的转化率。对所有人都是双赢。

好的。现在我们知道了过渡对你的网站非常有益，让我们学习如何在 Vue 中实现它们。

# 创建 Vue 过渡

为了[适应各种各样的开发人员](https://learnvue.co/2019/12/how-vue3-is-designed-for-both-hobby-devs-and-large-projects/)，VueJS 提供了几种方法来实现过渡:

*   CSS 过渡/动画样式
*   Javascript 挂钩对 DOM 进行编辑
*   集成第三方 CSS/JS 库

老实说，这每一个的难度都取决于你现有的知识。如果你有更多的 HTML/CSS 经验，你会喜欢使用过渡/动画样式。如果您刚从 React 过来，或者只是有更多的 Javascript 经验，手动编辑 DOM 是一个不错的选择。

现在，我们将集中使用 CSS 来制作单个元素的动画。但是不要担心，我们稍后将进入更好的东西(多元素、动态组件、作品)。

# 了解过渡组件

transition 元素是一个包装器，帮助您向元素添加转换功能。本质上，它设置了不同的挂钩，并向变化的元素添加了类，这样我们就可以在转换的不同阶段对它们进行样式化。

有 6 个不同的过渡类(3 个用于进入，3 个用于离开)。

1.  **v-enter / v-leave** :过渡的开始状态；过渡开始后移除
2.  **v-进入-激活/v-离开-激活**:转换的激活状态
3.  **v 进/ v 出**:过渡结束状态

注意:当你给你的转换一个名字属性时，这些是默认的名字。类的格式是 *name* -enter， *name* -enter-active，等等。

**让我们看看添加转场的简单方法。**

格式化你的模板代码真的很容易。只需选择您想要转换的元素，并将其包装在一个<transition>组件中。类似这样的。</transition>

```
<template>
  <div>
    <button [@click](http://twitter.com/click)='visible = !visible'>Toggle Div</button>
    <transition name="fade">
      <div v-if="visible">
        Hello World
      </div>
    </transition>
  </div>
</template><script>
export default {
  data () {
    return {
      visible: true
    }
  }
}
</script>
```

简单对吗？

现在，我们只需要添加一些样式来让过渡工作起来。

让我们使用 Vue 文档中的一些样式示例。

```
<style lang="scss">
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
```

这是做什么的？它实际上非常直观，因为它加入了相似状态的类。

这些样式表示当过渡处于活动状态时，将过渡添加到不透明度属性，以便它平滑移动。

然后，它将进入转换的开始状态和离开转换的结束状态设置为零(也称为不可见)。此外，请记住，当元素既没有 fade-enter 类也没有 fade-leave-to 类时，不透明度将被设置为默认值 1。

那里。

您已经向 Vue 应用添加了转场。现在，让我们来看看如何让它变得更加复杂和详细。

除了使用 CSS 过渡，还可以使用 CSS 动画。

只要您能够使用正确的类名，您就可以随心所欲地设计这些组件。

让我们继续学习一些使用 Vue 转场的更高级的技术。

# 变得有点复杂

虽然我们刚刚构建的转换元素很好地概述了组件是如何工作的，但是我们在现实世界中经常会遇到更复杂的用例。

幸运的是，像大多数 Vue 一样，模板非常灵活，可以适应大多数项目。让我们来看看一些不同的情况。

# 让您的组件在加载时转换

愚蠢的简单。就像这样把属性 appear 添加到你的转换元素中。

```
<transition name="fade" appear>
      <div v-if="visible">
        Hello World
      </div>
</transition>
```

# 在多个元素之间转换

同样，这非常简单——假设你有两个 div，它们像这样交替出现。

```
<transition name="fade" appear>
      <div v-if="visible">
        Option A
      </div>
      <div v-else>
        Option B
      </div>
</transition>
```

您所要做的就是将它们包装在一个过渡元素中，然后 BAM —您的过渡样式将对两者都有效。

有几件事需要注意:

当 Vue 在两个元素之间转换时，有时两个元素都可见，并且会转换进/出。如果你想要一个平滑的效果，你可能想要绝对地将它们放置在彼此之上。

如果你的元素有相同的标签，Vue 会尝试优化，只替换元素的内容。根据文档，如果你在多个元素之间转换，添加一个键总是**T2 的最佳实践。**

# 动态组件之间的转换

这甚至比在[多个组件](https://learnvue.co/2019/12/an-overview-of-vue-keep-alive/)之间转换更容易。您所要做的就是将您的动态组件包装在一个转换元素中。它的行为就像基本用例一样！

您的模板代码可能如下所示。

```
<transition name="fade" appear>
      <component :is='componentType' />
</transition>
```

# 创建可重用的转换组件

在 Vue 工作时，一个很好的习惯是尝试设计可重用的组件。

这对于转换来说很容易做到——我们真正要做的就是在根中放一个转换元素，并插入一个[组件槽](https://learnvue.co/2019/12/using-component-slots-in-vuejs%e2%80%8a-%e2%80%8aan-overview/),这样我们就可以添加更多的内容。

它看起来有点像这样。

```
<template>
  <transition name="fade" appear>
    <slot></slot>
  </transition>
</template>
```

现在，不用担心为每个组件添加过渡风格、名称和所有东西，您只需使用这个组件就可以了。

# 最后

在 Vue 中工作时，转场是一种非常简单的方法，可以给你的网站添加更多的风格和个性。

在大多数情况下，这只是几行模板代码和一些 CSS 样式，您就可以开始了。

过渡是让您的项目感觉更专业并改善用户体验的好方法。通过改善用户体验，你可以让更多的人留在你的网站上，提高你的转化率。

编码快乐！

[如果你有兴趣了解更多关于 Vue 3 的知识，下载我的免费的 Vue 3 备忘单，里面有基本的知识，比如组合 API、Vue 3 模板语法和事件处理。](https://learnvue.co/vue-3-essentials-cheatsheet/)

【https://learnvue.co】原载于 2020 年 1 月 3 日[](https://learnvue.co/2020/01/how-you-can-use-vue-transitions-right-now)**。**