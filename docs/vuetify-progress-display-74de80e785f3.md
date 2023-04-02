# 虚拟化—进度显示

> 原文：<https://javascript.plainenglish.io/vuetify-progress-display-74de80e785f3?source=collection_archive---------11----------------------->

![](img/9059c05ea81866cf47eb0d043e6fd43c.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 进度通报

我们可以用`v-progress-circular`组件添加一个循环进度显示。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-progress-circular indeterminate color="primary"></v-progress-circular>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

`indeterminate`道具让它永远显示。

而`color`道具设置了颜色。

# 尺寸和宽度

`size`和`width`道具让我们改变圆形进度显示的大小和宽度:

```
<template>
  <div class="text-center">
    <v-progress-circular indeterminate color="red" :width="3"></v-progress-circular>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

`width`改变宽度。

`size`以像素为单位改变循环进度显示的半径:

```
<template>
  <div class="text-center">
    <v-progress-circular indeterminate color="red" :size="50"></v-progress-circular>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({}),
};
</script>
```

# 辐状的

`rotate`道具让我们定制`v-progress-circular`组件的来源。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-progress-circular
      :rotate="360"
      :size="100"
      :width="15"
      :value="value"
      color="teal"
    >{{ value }}</v-progress-circular>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      interval: {},
      value: 0,
    };
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  mounted() {
    this.interval = setInterval(() => {
      if (this.value === 100) {
        return (this.value = 0);
      }
      this.value += 10;
    }, 1000);
  },
};
</script>
```

在圆圈内显示`value`，并根据`value`设置进度条的填充颜色。

# 线性进度

除了循环进度显示之外，Vuetify 还附带了`v-progress-linear`组件，以一行显示进度。

例如，我们可以添加一个:

```
<template>
  <div class="text-center">
    <v-progress-linear v-model="valueDeterminate" color="deep-purple accent-4"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      valueDeterminate: 50,
    };
  },
};
</script>
```

`v-model`设置进度值。

`color`有条形的颜色。

# 不定杆

我们可以添加`indeterminate`道具，让它永远保持动画效果:

```
<template>
  <div class="text-center">
    <v-progress-linear indeterminate color="yellow darken-2"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {};
  },
};
</script>
```

# 缓冲器

我们可以添加一个缓冲状态来同时表示两个值。

例如，我们可以写:

```
<template>
  <div class="text-center">
    <v-progress-linear v-model="value" :buffer-value="bufferValue"></v-progress-linear>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      value: 10,
      bufferValue: 20,
      interval: 0,
    };
  }, watch: {
    value(val) {
      if (val < 100) return; this.value = 0;
      this.bufferValue = 10;
      this.startBuffer();
    },
  }, mounted() {
    this.startBuffer();
  }, beforeDestroy() {
    clearInterval(this.interval);
  }, methods: {
    startBuffer() {
      clearInterval(this.interval); this.interval = setInterval(() => {
        this.value += Math.random() * (15 - 5) + 5;
        this.bufferValue += Math.random() * (15 - 5) + 6;
      }, 2000);
    },
  },
};
</script>
```

我们有高于`value`值的`bufferValue`。

`v-model`有进度值，`buffer-value`有第二个值，大于`v-model`的值。

# 结论

我们可以用 Vuetify 的组件添加一个圆形或线性的进度显示。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**