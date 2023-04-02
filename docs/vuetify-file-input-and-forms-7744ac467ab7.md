# 文件输入和表格

> 原文：<https://javascript.plainenglish.io/vuetify-file-input-and-forms-7744ac467ab7?source=collection_archive---------8----------------------->

![](img/f0ca31c897a70b0df78352693290f04e.png)

Photo by [Scott Blake](https://unsplash.com/@sunburned_surveyor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 文件输入的自定义图标

我们可以用`prepend-icon`道具改变图标。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input label="File input" filled prepend-icon="mdi-camera"></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

我们有了`prepend-icon`道具来添加我们想要显示的图标。

# 密集文件输入

`dense`道具可以让我们缩短文件输入。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input label="File input" dense></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
};
</script>
```

我们用`dense`道具让它变小。

# 文件输入选择槽

我们可以填充`selection`槽来定制输入选择的外观。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input
          v-model="files"
          placeholder="Upload your documents"
          label="File input"
          multiple
          prepend-icon="mdi-paperclip"
        >
          <template v-slot:selection="{ text }">
            <v-chip small label color="primary">{{ text }}</v-chip>
          </template>
        </v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    files: [],
  }),
};
</script>
```

我们用`v-model`来设置存储所选文件的状态。

在`selection`槽中，我们有`v-chip`组件来显示芯片所选择的文件名。

# 文件输入验证

`rules` prop 让我们设置验证所选文件的规则。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-file-input
          :rules="rules"
          accept="image/png, image/jpeg, image/bmp"
          placeholder="Pick an image"
          prepend-icon="mdi-camera"
          label="Avatar"
        ></v-file-input>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    rules: [
      (value) =>
        !value ||
        value.size < 2000000 ||
        "file is too big",
    ],
  }),
};
</script>
```

我们有一个带有验证文件功能的`rules`数组。

`value`有我们要验证的文件。

如果选择的文件无效，我们会返回一条错误消息。

# 形式

我们可以用 Vuetify 添加表单。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-container>
          <v-row justify="space-between">
            <v-col cols="12" md="4">
              <v-form ref="form">
                <v-text-field v-model="model" :counter="max" :rules="rules" label="First name"></v-text-field>
              </v-form>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    max: 10,
    model: "Foobar",
  }), computed: {
    rules() {
      const rules = []; if (this.max) {
        const rule = (v) =>
          (v || "").length <= this.max ||
          `A maximum of ${this.max} characters is allowed`; rules.push(rule);
      } return rules;
    },
  }, watch: {
    match: "validateField",
    max: "validateField",
    model: "validateField",
  }, methods: {
    validateField() {
      this.$refs.form.validate();
    },
  },
};
</script>
```

我们有`rules` computed 属性来根据输入计算规则。

我们用`validateField`方法验证这个表单。

我们获取表单的 ref 并对其调用`validate`。

`counter`道具设置角色的最大数量。

`rules`道具有验证规则。

![](img/1dc0b9c50c78181cc417a95a62123ac7.png)

Photo by [Patrick Campanale](https://unsplash.com/@patrickcampanale?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 添加文件输入和表单。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**