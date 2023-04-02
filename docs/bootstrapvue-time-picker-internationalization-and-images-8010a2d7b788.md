# BootstrapVue —时间选择器国际化和映像

> 原文：<https://javascript.plainenglish.io/bootstrapvue-time-picker-internationalization-and-images-8010a2d7b788?source=collection_archive---------9----------------------->

![](img/d7f9d997d797cc0e072886da058423c6.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何定制一个时间选择器和图像。

# 时间选择器国际化

我们可以更改时间选择器的标签值，使其适用于不同的地区。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" v-bind="de" show-seconds :locale="locale"></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: "",
      de: {
        labelHours: "Stunden",
        labelMinutes: "Minuten",
        labelSeconds: "Sekunden",
        labelIncrement: "Erhöhen",
        labelDecrement: "Verringern",
        labelSelected: "Ausgewählte Zeit",
        labelNoTimeSelected: "Keine Zeit ausgewählt",
        labelCloseButton: "Schließen"
      }
    };
  }
};
</script>
```

我们有`v-bind`指令，将标签作为道具绑定到`b-form-timerpicker`组件。

现在我们将看到控件的德国标签，而不是默认的英语标签。

# 小时周期

我们可以通过改变小时周期来改变小时的显示方式。

有 4 种方式显示小时。

`'h12'`显示从 1 到 12。

`'h23'`显示从 0 到 23。

`'h11'`显示 0 到 11，`'h24'`显示 1 到 24。

这些都可以作为自己的道具。

为了以 12 小时制显示小时，我们将`hour12`属性设置为`true`。

为了显示 24 小时制，我们将`hour12`设置为`false`。

根据`Intl.DateTimeFormat`返回的值，格式化的时间字符串可能是`h12`或`h23`。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-timepicker v-model="value" :hour12="true" :locale="locale"></b-form-timepicker>
    <div>Value: {{ value }}</div>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      value: ""
    };
  }
};
</script>
```

和显示从 1 到 12 的小时数，但也可以根据区域设置格式化为 0 到 11 或 1 到 12。

# 形象

我们可以添加`b-img`组件来显示响应图像。

还有一个`b-img-lazy`，只在图像显示在屏幕上时才加载。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" fluid alt="cat"></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

向组件添加图像。

`fluid`表示图像不会随其容器一起增长。

为了让它随着容器一起成长，我们可以使用`fluid-grow`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" fluid-grow alt="cat"></b-img>
  </div>
</template>
<script>
export default {
  name: "App"
};
</script>
```

# 拇指甲

要用缩略图制作图像，我们可以使用`thumbnail`道具。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" thumbnail></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

这将使图像显示为缩略图。

# 圆角

我们可以使用`rounded`道具使图像的角变圆。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" rounded></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

现在我们的图像有了圆角。

也可以为其设置一个设定值，包括`'top'`、`'right'`、`'bottom'`、`'left'`、`'circle'`或 0。

0 关闭舍入。

所以我们可以写:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" rounded="circle"></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后我们可以看到图像变圆了。

# 对齐图像

我们可以通过添加`left`、`center`和`right`道具来对齐图像。

例如，我们可以使用这些道具添加图像，如下所示:

```
<template>
  <div id="app">
    <b-img src="http://placekitten.com/200/200" right></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

然后，我们将图像与页面的右侧对齐。

# 纯色图像

我们可以把图像做成纯色。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-img blank blank-color="green" alt="green image" width='200' height='200'></b-img>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们有`blank`道具使图像空白。

`blank-color`设置颜色。

`alt`是描述性文字。

`width`是图像的宽度，`height`是图像的高度。

![](img/762c19c5f9466a6c382d4bb26bb4cf48.png)

Photo by [Brienna Scott](https://unsplash.com/@bbhjk123?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以自定义时间选择器的区域设置。

此外，我们可以用`b-img`组件添加图像。

它们可以有图像，也可以是空白的。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道，解码**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**