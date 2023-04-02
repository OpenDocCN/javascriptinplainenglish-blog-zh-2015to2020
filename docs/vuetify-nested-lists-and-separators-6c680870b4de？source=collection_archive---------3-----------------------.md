# Vuetify —嵌套列表和分隔符

> 原文：<https://javascript.plainenglish.io/vuetify-nested-lists-and-separators-6c680870b4de?source=collection_archive---------3----------------------->

![](img/abaa63597f3eb8c240c56bad9d5aabb2.png)

Photo by [Karen Ciocca](https://unsplash.com/@kciocca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 嵌套列表

我们可以用`v-list-item`组件添加嵌套列表。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card class="mx-auto" width="300">
        <v-list>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-home</v-icon>
            </v-list-item-icon> <v-list-item-title>Home</v-list-item-title>
          </v-list-item> <v-list-group prepend-icon="account_circle" value="true">
            <template v-slot:activator>
              <v-list-item-title>Users</v-list-item-title>
            </template> <v-list-group no-action sub-group value="true">
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title>Admin</v-list-item-title>
                </v-list-item-content>
              </template> <v-list-item v-for="(admin, i) in admins" :key="i" link>
                <v-list-item-title v-text="admin[0]"></v-list-item-title>
                <v-list-item-icon>
                  <v-icon v-text="admin[1]"></v-icon>
                </v-list-item-icon>
              </v-list-item>
            </v-list-group> <v-list-group sub-group no-action>
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title>Actions</v-list-item-title>
                </v-list-item-content>
              </template>
              <v-list-item v-for="(crud, i) in cruds" :key="i">
                <v-list-item-title v-text="crud[0]"></v-list-item-title>
                <v-list-item-action>
                  <v-icon v-text="crud[1]"></v-icon>
                </v-list-item-action>
              </v-list-item>
            </v-list-group>
          </v-list-group>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    admins: [
      ["Management", "people_outline"],
      ["Settings", "settings"],
    ],
    cruds: [
      ["Create", "add"],
    ],
  }),
};
</script>
```

我们在另一个`v-list-group`里面有`v-list-group`。

# 副标题和分隔符

我们可以添加标题和分隔线来分隔我们的列表。

例如，我们可以写:

```
<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="475" class="mx-auto">
        <v-toolbar color="teal" dark>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
          <v-toolbar-title>Settings</v-toolbar-title>
        </v-toolbar> <v-list two-line subheader>
          <v-subheader>General</v-subheader>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>Profile photo</v-list-item-title>
              <v-list-item-subtitle>Change profile photo</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list> <v-divider></v-divider> <v-list subheader two-line flat>
          <v-subheader>Hangout notifications</v-subheader><v-list-item-group v-model="settings" multiple>
            <v-list-item>
              <template v-slot:default="{ active }">
                <v-list-item-action>
                  <v-checkbox :input-value="active" color="primary"></v-checkbox>
                </v-list-item-action> <v-list-item-content>
                  <v-list-item-title>Notifications</v-list-item-title>
                  <v-list-item-subtitle>Allow notifications</v-list-item-subtitle>
                </v-list-item-content>
              </template>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    settings: [],
  }),
};
</script>
```

我们有了`v-subheader`组件来添加副标题。

我们有`v-divider`来添加列表之间的分隔符。

# 结论

我们可以在列表中添加副标题和分隔线。

此外，我们可以在页面中包含嵌套列表。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**