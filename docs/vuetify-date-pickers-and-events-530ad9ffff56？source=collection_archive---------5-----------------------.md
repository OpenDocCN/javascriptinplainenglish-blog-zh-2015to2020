# Vuetify —日期选取器和事件

> 原文：<https://javascript.plainenglish.io/vuetify-date-pickers-and-events-530ad9ffff56?source=collection_archive---------5----------------------->

![](img/75f59c494dda994e01c21e8ae3aae0ca.png)

Photo by [Noiseporn](https://unsplash.com/@noiseporn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 日期选择器—多个

如果我们添加了`multiple`道具，我们可以选择多个日期。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row justify="center">
      <v-date-picker v-model="dates" multiple></v-date-picker>
    </v-row>
    <v-row justify="center">
      <p>{{dates}}</p>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dates: ["2020-09-15", "2020-09-20"],
  }),
};
</script>
```

展示一个日期选择器，让我们选择多个日期。

`dates`是一个日期字符串数组。

# 日期选择器-范围

我们可以添加`range`道具来设置射程。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row justify="center">
      <v-date-picker v-model="dates" range></v-date-picker>
    </v-row>
    <v-row justify="center">
      <p>{{dates}}</p>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    dates: ["2020-09-15"],
  }),
};
</script>
```

现在我们可以选择一系列日期。

# 日期选择器—生日选择器

我们可以通过重组日期范围并在选择日期后关闭日期选择器来制作一个生日选择器。

例如，我们可以写:

```
<template>
  <v-menu
    ref="menu"
    v-model="menu"
    :close-on-content-click="false"
    transition="scale-transition"
    offset-y
    min-width="290px"
  >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        v-model="date"
        label="Birthday date"
        prepend-icon="event"
        readonly
        v-bind="attrs"
        v-on="on"
      ></v-text-field>
    </template>
    <v-date-picker
      ref="picker"
      v-model="date"
      :max="new Date().toISOString().substr(0, 10)"
      min="1950-01-01"
      @change="save"
    ></v-date-picker>
  </v-menu>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: null,
    menu: false,
  }),
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = "YEAR"));
    },
  },
  methods: {
    save(date) {
      this.$refs.menu.save(date);
    },
  },
};
</script>
```

如果我们打开菜单到`'YEAR'`，我们设置日期选择器来改变模式。

这样，我们必须先选择年份，然后选择月份和日期。

一旦日期改变，就会调用`save`方法将值保存到`v-model`的日期，即`date`。

# 日期选取器事件

我们可以在日期选择器中显示事件。

例如，我们可以写:

```
<template>
  <v-row justify="space-between">
    <div>
      <div class="subheading">Defined by array</div>
      <v-date-picker v-model="date1" :events="arrayEvents" event-color="green lighten-1"></v-date-picker>
    </div>
    <div>
      <div class="subheading">Defined by function</div>
      <v-date-picker
        v-model="date2"
        :event-color="date => date[9] % 2 ? 'red' : 'yellow'"
        :events="functionEvents"
      ></v-date-picker>
    </div>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    arrayEvents: null,
    date1: new Date().toISOString().substr(0, 10),
    date2: new Date().toISOString().substr(0, 10),
  }), mounted() {
    this.arrayEvents = [...Array(6)].map(() => {
      const day = Math.floor(Math.random() * 30);
      const d = new Date();
      d.setDate(day);
      return d.toISOString().substr(0, 10);
    });
  }, methods: {
    functionEvents(date) {
      const [, , day] = date.split("-");
      if ([1, 2, 3].includes(parseInt(day, 10))) return true;
      if ([10, 11, 12].includes(parseInt(day, 10))) return ["red", "#00f"];
      return false;
    },
  },
};
</script>
```

在第一个日期选择器中，我们将`events`属性设置为`arrayEvents`数组。

这是一个包含一些日期的数组。

我们还可以将`events`设置为返回布尔值的函数。

我们可以返回一个函数，用一个布尔值或者一个点的颜色字符串。

# 结论

我们可以添加日期选择器来显示事件，并动态改变模式。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**