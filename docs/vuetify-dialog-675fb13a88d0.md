# 虚拟化—对话框

> 原文：<https://javascript.plainenglish.io/vuetify-dialog-675fb13a88d0?source=collection_archive---------10----------------------->

![](img/8401daf2412a01dafd2b47b6c9356e10.png)

Photo by [Mimi Thian](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 情态的

我们可以用`persistent`道具创建一个模态对话框。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-row justify="center">
          <v-dialog v-model="dialog" persistent max-width="290">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark v-bind="attrs" v-on="on">Open Dialog</v-btn>
            </template>
            <v-card>
              <v-card-title class="headline">Title</v-card-title>
              <v-card-text>Lorem ipsum.</v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="green darken-1" text @click="dialog = false">Cancel</v-btn>
                <v-btn color="green darken-1" text @click="dialog = false">OK</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
  }),
};
</script>
```

我们在`v-dialog`上安装了`persistent`支柱，使其只能通过内部按钮关闭。

# 可滚动

要使对话框可滚动，我们只需添加一个`scrollable`道具。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-row justify="center">
          <v-dialog v-model="dialog" scrollable max-width="290">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark v-bind="attrs" v-on="on">Open Dialog</v-btn>
            </template>
            <v-card>
              <v-card-title>Select Country</v-card-title>
              <v-divider></v-divider>
              <v-card-text style="height: 300px;">
                <v-radio-group v-model="country" column>
                  <v-radio :label="c" :value="c" v-for="(c, i) of countries" :key="i"></v-radio>
                </v-radio-group>
              </v-card-text>
              <v-spacer></v-spacer>
              <v-card-actions>
                <v-btn color="green darken-1" text @click="dialog = false">Cancel</v-btn>
                <v-btn color="green darken-1" text @click="dialog = false">OK</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
    country: "",
    countries: [
      "Afghanistan",
      "Albania",
      "Algeria",
      "American Samoa",
      "Andorra",
      "Angola",
      "Anguilla",
      "Antarctica",
      "Antigua and Barbuda",
      "Argentina",
      "Armenia",
      "Aruba",
      "Australia",
      "Austria",
      "Azerbaijan",
    ],
  }),
};
</script>
```

我们显示了一个溢出了`v-card-text`容器的单选按钮组。

所以我们给`v-dialog`添加了`scrollable`道具，使单选按钮组可以滚动。

# 溢出

如果模式不适合可用的窗口空间，那么容器将被滚动。

# 形式

我们可以在`v-dialog`组件中添加一个表单。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-row justify="center">
          <v-dialog v-model="dialog" persistent max-width="600px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark v-bind="attrs" v-on="on">Open Dialog</v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">User Profile</span>
              </v-card-title>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field label="first name*" required></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field label="middle name" hint="middle name"></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field label="last name*" hint="last name" persistent-hint required></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field label="Email*" required></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field label="Password*" type="password" required></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-select :items="['0-17', '18-29', '30-54', '54+']" label="Age*" required></v-select>
                    </v-col>
                  </v-row>
                </v-container>
                <small>*required field</small>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
                <v-btn color="blue darken-1" text @click="dialog = false">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    dialog: false,
  }),
};
</script>
```

在`v-dialog`中添加一个表单。

这样，当我们打开对话框时，我们将看到表单。

![](img/623e27b41c49a2e05ad16cbe9202a6d5.png)

Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Vuetify 创建一个包含各种内容和行为的对话框。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**