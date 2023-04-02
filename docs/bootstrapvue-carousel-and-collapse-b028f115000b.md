# BootstrapVue —旋转和折叠

> 原文：<https://javascript.plainenglish.io/bootstrapvue-carousel-and-collapse-b028f115000b?source=collection_archive---------7----------------------->

![](img/f7ff3b60378a94b044473436a5310490.png)

Photo by [sept commercial](https://unsplash.com/@septdoigt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将看看如何添加一个旋转木马和折叠组件到一个 Vue 应用程序中。

# 旋转木马

轮播是一种幻灯片放映，用于循环播放一系列内容。

它可以包括图像、文本或自定义标记。

BootstrapVue 的转盘支持上一个/下一个控件和指示器。

我们可以通过使用`b-carousel`组件来添加一个。

要使用它，我们可以写:

```
<template>
  <div id="app">
    <b-carousel      
      v-model="slide"
      :interval="5000"
      controls
      indicators
      background="#ababab"
      img-width="1024"
      img-height="480"      
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <b-carousel-slide caption="cat" text="cat" img-src="https://placekitten.com/g/600/200"></b-carousel-slide><b-carousel-slide img-src="https://picsum.photos/1024/480/?image=54">
        <h1>hello</h1>
      </b-carousel-slide>
    </b-carousel><p class="mt-4">
      Slide #: {{ slide }}
      <br>
      Sliding: {{ sliding }}
    </p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      slide: 0,
      sliding: null
    };
  },
  methods: {
    onSlideStart(slide) {
      this.sliding = true;
    },
    onSlideEnd(slide) {
      this.sliding = false;
    }
  }
};
</script>
```

我们在上面的组件中有一个旋转木马。

`interval`表示我们每 5 秒钟循环一次代码。

`slide`是我们所在幻灯片的索引。

`controls`表示我们显示控件

`indicators`表示我们用破折号来表示我们在哪个幻灯片上。

`background`是显示的背景色。

`img-width`是以像素为单位的幻灯片图像宽度。

`img-height`是以像素为单位的幻灯片图像高度。

`sliding-start`是滑块开始滑动时运行的处理程序。

`sliding-end`是滑块完成滑动时运行的处理程序。

处理程序有`slide`参数来获取幻灯片索引。

`v-model`设置为`slide`以设置`slide`状态，其值为滑动索引。

# 交叉渐变动画

我们可以用`b-carousel`组件上的`fade`道具添加交叉渐变动画。

# 禁用动画

要禁用动画，我们可以使用`b-carousel`组件上的`no-animation`道具。

# 幻灯片包装

添加到`b-carousel`的`no-wrap`支柱将禁止传送带返回第一个载玻片。

# 倒塌

折叠组件是一个包含可以打开和关闭的内容的框。

为了使用它，我们使用带有`b-collapse`组件的`v-b-toggle`指令。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-toggle.collapse-1 variant="primary">toggle</b-button>
    <b-collapse id="collapse-1" class="mt-2">
      <b-card>
        <p class="card-text">collapse content</p>
      </b-card>
    </b-collapse>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有一个`b-button`,它有`v-b-toggle`指令和`collapse-1`修饰符来指示它正在打开和关闭 ID 为`collapse-1`的`b-collapse`组件。

然后我们可以看到`b-collapse`组件被切换。

我们可以通过添加`b-collapse`和一个带有`v-b-toggle`的组件来将折叠组件嵌套在另一个组件中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-toggle.collapse-1 variant="primary">toggle</b-button>
    <b-collapse id="collapse-1" class="mt-2">
      <b-card>
        <p class="card-text">collapse content</p>
        <b-button v-b-toggle.collapse-1-inner size="sm">toggle inner</b-button>
        <b-collapse id="collapse-1-inner" class="mt-2">
          <b-card>inner content</b-card>
        </b-collapse>
      </b-card>
    </b-collapse>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有:

```
<b-button v-b-toggle.collapse-1-inner size="sm">toggle inner</b-button>
<b-collapse id="collapse-1-inner" class="mt-2">
  <b-card>inner content</b-card>
</b-collapse>
```

这是内部塌陷组件。

我们可以用同样的方式打开和关闭它。

我们只需要确保我们不会使用与外部相同的 id。

# v 型车

我们可以通过用`v-model`绑定一个状态来得到折叠组件的折叠状态。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-button v-b-toggle.collapse-1 variant="primary">toggle</b-button>
    <b-collapse id="collapse-1" class="mt-2" v-model="visible">
      <b-card>
        <p class="card-text">collapse content</p>
      </b-card>
    </b-collapse>
    <p>{{visible}}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      visible: false
    };
  }
};
</script>
```

我们添加了`v-model`来绑定到`visible`状态，这样我们就可以将可见性状态设置为该属性的值。

如果折叠被展开，则`visible`将显示`true`，否则显示`false`。

![](img/c8e5ab46861c609ef27868d220ba138e.png)

Photo by [Kira auf der Heide](https://unsplash.com/@kadh?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 BoostrapVue 创建一个旋转木马。可以更改幻灯片选项和控件。

折叠是一个我们可以打开和关闭的组件。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过[**plain English . io**](https://plainenglish.io/)找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**