# BootstrapVue —微调按钮

> 原文：<https://javascript.plainenglish.io/bootstrapvue-spin-button-ddada4204b6f?source=collection_archive---------4----------------------->

![](img/a5fef75c195b555af6bb3ad5b66c7bc0.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在这篇文章中，我们将看看如何添加一个旋转按钮。

# 旋转按钮

微调按钮是一种数值范围表单控件。

从 Bootstrap 2.5.0 开始就有了。

我们可以将它包含在`b-form-spinbutton`组件中。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton v-model="value" min="1" max="100"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

我们用`min`和`max`道具设置最小和最大允许值。

`v-model`将输入值绑定到`value`。

然后我们看到一个表单控件，左边有一个减号按钮，右边有一个加号按钮。

输入的数字将显示在中间。

我们还显示了 p 元素中`value`的值。

# 步骤

`step`道具让我们改变减号和加号按钮的增量。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton v-model="value" step="0.5" min="1" max="100"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

然后减号和加号按钮分别递减和递增 0.5。

# 数字换行

默认情况下，当我们单击加号按钮并且它到达`max`值时，如果我们再次单击它，它什么也不做。

减号按钮具有相同的递减值行为。

我们不是什么都不做，而是添加了`wrap`道具，这样数字就被包装起来了:

```
<template>
  <div id="app">
    <b-form-spinbutton wrap v-model="value" min="1" max="5"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

现在，当我们单击加号按钮，值超过最大值时，它将返回到 1。

同样，如果我们递减到最小值，我们将回到 5。

# 大小

我们可以用`size`道具改变它的大小。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton size="sm" v-model="value" min="1" max="5"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

那么输入会特别小。

我们也可以将`size`设置为`'lg'`使其变得特别大。

# 在一条直线上的

`inline`使输入内联。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton inline v-model="value" min="1" max="5"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

# 垂直的

我们可以使用`vertical`道具使布局垂直:

```
<template>
  <div id="app">
    <b-form-spinbutton vertical v-model="value" min="1" max="5"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

# 宽度

我们可以用`w-25`或`w-50`这样的实用类来控制宽度。

# 数字格式和区域设置

我们可以用自己的方式格式化数字。

要设置语言环境，我们可以使用`locale`属性。

例如，我们写道:

```
<template>
  <div id="app">
    <b-form-spinbutton locale='fr-ca' v-model="value" step="0.1" min="1" max="5"></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0
    };
  }
};
</script>
```

然后我们看到法裔加拿大人对十进制数的表示。

此外，我们可以使用`formatter-fn`属性运行一个函数，按照我们的方式格式化显示的数字。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-spinbutton
      :formatter-fn="fruitFormatter"
      v-model="value"
      wrap
      min="0"
      :max="fruits.length - 1"
    ></b-form-spinbutton>
    <p>Value: {{ value }}</p>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    return {
      value: 0,
      fruits: ["apple", "orange", "grape"]
    };
  },
  methods: {
    fruitFormatter(value) {
      return this.fruits[value];
    }
  }
};
</script>
```

我们将`max`设置为`fruits.length — 1`，将`min`设置为 0，并添加`wrap`道具，这样我们就可以循环遍历`fruitFormatter`中数组的索引。

`value`是输入的数字，是索引。

所以我们看到了显示的`fruits`条目。

![](img/ef03d37a65dc39e9b8f55bed4c369547.png)

Photo by [Katerina Kerdi](https://unsplash.com/@katekerdi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用微调按钮做很多事情，它让我们显示一个带有增量和减量按钮的数字输入。

可以对输入进行格式化，也可以设置语言环境。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**