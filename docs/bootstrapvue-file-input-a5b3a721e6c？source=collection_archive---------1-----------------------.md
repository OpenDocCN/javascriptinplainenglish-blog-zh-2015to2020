# BootstrapVue —文件输入

> 原文：<https://javascript.plainenglish.io/bootstrapvue-file-input-a5b3a721e6c?source=collection_archive---------1----------------------->

![](img/ed2e102bcd88d2aaaa24fd5316ea0c88.png)

Photo by [Kouji Tsuru](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

在本文中，我们将了解如何使用 BootstrapVue 添加文件输入。

# 文件上传

我们可以使用`b-form-file`组件添加一个文件上传小部件。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-file
      v-model="file"
      :state="Boolean(file)"
      placeholder="choose file..."
      drop-placeholder="drop file..."
    ></b-form-file>
    <p>{{ file ? file.name : '' }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data(){
    return {
      file: undefined
    }
  }
};
</script>
```

`b-form-file`组件让我们添加一个文件输入。

`v-model`获取选中的文件对象并将其绑定到`file`属性。

`state`用于设置验证状态。

`placeholder`是为常规文件输入显示的占位符。

`drop-placeholder`是拖放文件时显示的占位符。

然后我们在它下面显示文件名。

我们可以使用`multiple`道具让用户添加多个文件。

默认情况下，丢弃模式是启用的。

我们可以添加`no-drop`道具来禁用掉文件。

# 限制文件类型

我们可以限制可以上传的文件类型。

为此，我们只需设置`accept`道具。

例如，我们可以写:

```
<b-form-file accept="image/jpeg, image/png, image/gif"></b-form-file>
```

或者:

```
<b-form-file accept=".jpg, .png, .gif"></b-form-file>
```

我们可以设置文件的 mime 类型或文件扩展名。

# 胶料

文件上传框的大小也可以改变。

`size`道具让我们改变大小。`'sm'`代表小尺寸，`'lg'`代表大尺寸。

否则，我们得到默认的大小。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-file v-model="file" :state="Boolean(file)" size="sm"></b-form-file>
    <p>{{ file ? file.name : '' }}</p>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      file: undefined
    };
  }
};
</script>
```

# 文件名格式化程序

我们可以按照自己喜欢的方式格式化所选文件的文件名。

为此，我们可以创建一个函数。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-file multiple :file-name-formatter="formatNames"></b-form-file>
    <p>{{ file ? file.name : '' }}</p>
  </div>
</template><script>
export default {
  name: "App",
  methods: {
    formatNames(files) {
      if (files.length === 1) {
        return files[0].name;
      } else {
        return `${files.length} files chosen`;
      }
    }
  }
};
</script>
```

`files`参数是一个带有所选文件的类似数组的对象。

因此，我们可以检查`length`属性并返回我们想要显示的字符串。

# 用作用域槽格式化文件名

为了在选择文件时显示文本之外的内容，我们可以在槽中添加项目。

为此，我们写道:

```
<template>
  <div id="app">
    <b-form-file multiple>
      <template slot="file-name" slot-scope="{ names }">
        <b-badge>chosen file: {{ names[0] }}</b-badge>
        <b-badge v-if="names.length > 1" variant="dark" class="ml-1">{{ names.length }} files chosen</b-badge>
      </template>
    </b-form-file>
  </div>
</template><script>
export default {
  name: "App"
};
</script>
```

我们用自己的内容填充`file-name`槽。

`names`数组让我们访问文件名。

# 自（动）调焦装置

我们可以添加`autofocus`属性，使文件输入在加载或显示在`keep-alive`组件中时成为焦点。

# 清除文件选择

我们可以通过将文件对象设置为`null`或`undefined`来清除文件选择。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-form-file v-model="file"></b-form-file>
    <b-button @click="file = undefined">Reset file input</b-button>
  </div>
</template><script>
export default {
  data() {
    return {
      file: undefined
    };
  }
};
</script>
```

我们只需将`file`设置为`undefined`来清除输入。

同样，我们可以通过为文件输入设置一个 ref 来清除它，然后对它调用`reset`。

我们可以写:

```
<template>
  <div id="app">
    <b-form-file v-model="file" ref="file-input" class="mb-2"></b-form-file>
    <b-button @click="clearFiles">Reset file input</b-button>
  </div>
</template><script>
export default {
  data() {
    return {
      file: undefined
    };
  },
  methods: {
    clearFiles() {
      this.$refs["file-input"].reset();
    }
  }
};
</script>
```

这与通过清除模型来重置是一样的。

![](img/ae569a71630263c1b2b8f082616243ff.png)

Photo by [Victor Malyushev](https://unsplash.com/@malyushev?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

BootstrapVue 带有一个文件输入组件。

我们可以用它做所有我们能想到的事情，比如放下文件、选择文件和清除文件选择。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道，解码**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**