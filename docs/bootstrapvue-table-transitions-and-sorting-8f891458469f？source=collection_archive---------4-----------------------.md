# BootstrapVue —表格转换和排序

> 原文：<https://javascript.plainenglish.io/bootstrapvue-table-transitions-and-sorting-8f891458469f?source=collection_archive---------4----------------------->

![](img/b2993999c971d67bfa1ff3b81c2920f4.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了制作好看的 Vue 应用，我们需要设计组件的样式。

为了使我们的生活更容易，我们可以使用内置样式的组件。

我们看看如何定制表格内容，包括过渡和排序。

# 表体转换

我们可以向表体添加表体过渡。

为此，我们可以在对象中添加`tbody-transition-prois`来添加`transition-group`属性。

`tbody-transition-handlers` kets 让我们从一个对象中添加`transition-groip`事件处理程序。

`primary-lkey`是一个字符串，指定用作唯一行键的字段。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table primary-key="id" :tbody-transition-props="transProps" :items="items" :fields="fields"></b-table>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      fields: [
        { key: "id", sortable: true },
        { key: "firstName", sortable: true },
        { key: "lastName", sortable: true }
      ],
      transProps: {
        name: "flip-list"
      },
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script><style>
table .flip-list-move {
  transition: transform 1s;
}
</style>
```

我们在对表格行排序时添加了过渡效果。

为此，我们添加了设置为`transProps`对象的`tbody-transition-props`道具。

该对象有一个过渡名称。

该名称应该与 CSS 转换的前缀类相匹配。

因此，在`style`标签中，我们有了以`flip-list`开头的`flip-list-move`类。

`primary-key`也被设置为我们表中的唯一键列，即`id`。

我们用每个`fields`数组条目中的`sortable`属性使其可排序。

现在，当我们单击每个字段上的排序按钮时，我们会看到一个淡入淡出效果。

# 整理

BootstrapVue 表是可排序的。

我们可以指定整理桌子的道具。

要指定要排序的列，我们可以使用`sort-by`属性。

可以通过将`sort-desc`设置为`true`或`false`来设置方向。

通过这种方式，我们可以分别从最高到最低对值进行排序，反之亦然。

设置`foot-clone`时，页脚标题也允许点击排序。

即使我们有自定义页脚也是如此。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table sort-by="id" sortDesc :items="items" :fields="fields"></b-table>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      fields: [
        { key: "id", sortable: true },
        { key: "firstName", sortable: true },
        { key: "lastName", sortable: true }
      ],
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  }
};
</script>
```

我们添加了带有我们想要排序的字段名称的`sort-by`属性。

设置为`id`按`id`排序。

让我们按降序对该列进行排序。

因此，我们按照`id`列对行进行降序排序。

# 自定义排序图标

我们可以定制 CSS 来改变排序图标。

# 排序比较

我们可以将条目与`sort-compare`属性集的比较方式改为一个函数，让我们自定义排序。

例如，我们可以写:

```
<template>
  <div id="app">
    <b-table sort-compare='sortCompare' :items="items" :fields="fields"></b-table>
  </div>
</template><script>
export default {
  name: "App",
  data() {
    return {
      fields: [
        { key: "id", sortable: true },
        { key: "firstName", sortable: true },
        { key: "lastName", sortable: true }
      ],
      items: [
        { id: 1, firstName: "alex", lastName: "green" },
        {
          id: 2,
          firstName: "may",
          lastName: "smith"
        },
        { id: 3, firstName: "james", lastName: "jones" }
      ]
    };
  },
  methods: {
    sortCompare(
      aRow,
      bRow,
      key,
      sortDesc,
      formatter,
      compareOptions,
      compareLocale
    ) {
      return aRow[key].localeCompare(bRow[key], compareLocale, compareOptions);
    }
  }
};
</script>
```

我们给出了我们设置为`sort-compare`的值的`sortCompare`。

在方法中，我们可以将`compareLocale`和`compareOptions`传递给字符串的`localeCompare`方法来对它们进行排序。

![](img/97b85204b8401d955525162eb9bc9dd0.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在操作行时添加表体过渡来增加效果。

此外，我们可以使用内置的 props 或定制的排序函数按照自己的方式对列进行排序。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**