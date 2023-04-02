# 火绒风格的电影夜采摘与 VueJs，Vuetify，VuexFire，和 Firebase

> 原文：<https://javascript.plainenglish.io/tinder-style-movie-night-picker-with-vuejs-and-firebase-d62d6c39feb9?source=collection_archive---------12----------------------->

## 第 5 部分——火柴和应用抛光

![](img/29194706d3d7566ffb0bf19200b94e85.png)

Image by [David Mark](https://pixabay.com/users/12019-12019/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1936633) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1936633)

在第四部分中，我们让电影流入我们的应用程序，并允许用户上下滑动。我们还检查了他们的搭档是否有匹配的电影。我们需要做的最后一件事是在另一个页面上显示这些匹配，并稍微润色一下应用程序。

# 火柴和真空火焰

为了显示比赛，我们将使用 VuexFire。这将为我们的 matches 页面提供实时功能，而无需编写额外的代码。为此，转到 src/store/modules/user.js。

```
*import* { firestoreAction } *from* "vuexfire";
*import* { db } *from* "../../main";
```

然后添加一个名为 matches 的新状态属性，并将其设置为一个空数组:

```
const *state* ={
 *id*: *null*,
 *name*: *null*,
 *email*: *null*,
 *partnerId*: *null*,
 *movieApiPage*:1,
 ***matches*:[],**
};
```

接下来，创建一个名为 bindMatchesRef 的新操作:

```
*bindMatchesRef*: *firestoreAction*((*context*)=>{
 *return context.bindFirestoreRef*(
  "matches",
 db
 *.collection*("users")
 *.doc*(*context.state.id*)
 *.collection*("matches")
 );
}),
```

然后，转到 src/store/index.js。在那里，我们需要在顶部添加一个新的导入:

```
*import* { vuexfireMutations } *from* "vuexfire";
```

现在我们将把它添加到带有对象析构的基本存储变异中:

```
mutations: {
 ...vuexfireMutations,
}
```

我们需要做的最后一件事是调用我们在 Vuex 用户模块中添加的新动作。为此，请访问 src/views/Home.vue，从我们创建的 vue 生命周期中调用操作:

```
*created*() {
 **this*.$store.dispatch*("user/bindMatchesRef");**
 this*.fetchMovies*(this*.*userMovieApiPage);
}
```

现在我们已经设置好了，我们的比赛将在我们即将创建的比赛页面上实时更新。

# 匹配页面

现在要显示匹配项，请转到 src/views 并创建一个名为 Matches.vue 的新文件。在这里，我们将显示用户的匹配项和每部电影的详细信息。我们唯一需要的是访问 Firestore 中的 matches 集合。

因为 VuexFire 会负责将数据与我们的商店同步，所以我们需要做的就是创建一个计算属性来引用它:

```
computed: {
  *matches*() {
   *return* this*.$store.state.user.*matches;
  },
},
```

我们还将创建一个方法 prop to 和一个名为 getMovieImage 的方法来获取比赛的海报图像:

```
methods: {
 *getMovieImage*(posterPath) {
  *return* posterPath ?
  `https://image.tmdb.org/t/p/w500/${posterPath}` : 
  "";
 },
}
```

为了显示匹配，我们将遍历它们并为每一个创建一张卡片。卡片内有标题、描述和图片。您的最终 Matches.vue 文件应该如下所示:

# 航行

我们的应用程序现在有相当多的页面，但用户没有办法在不知道路径的情况下导航到这些页面。为了解决这个问题，我们将创建一个包含所有页面的导航抽屉。为此，我们将把*虚拟应用工具栏*移到它自己的组件中，并添加一个*虚拟导航抽屉。*

在 src 中创建一个名为 components 的新文件夹。然后，在这个文件夹中创建一个名为 AppBar.vue 的文件，接下来，在 src/App.vue 中导入并注册这个组件，复制 v-app-bar，并用导入的组件替换它。

回到 AppBar.vue，我们将为应用程序中的每个页面添加一个带有 *v 列表项*的 *v 导航抽屉*。我们需要添加一个选项 API 数据属性和一个抽屉属性来控制抽屉的显示和隐藏。最后，我们将添加一个 *v-app-bar-nav-icon* 来切换我们的抽屉属性。只有当用户登录时才会显示。

完成所有这些之后，您的 AppBar.vue 文件应该如下所示:

AppBar.vue

# 注销用户

## 注销按钮

我们要实现的最后一件事是为用户提供注销功能。为此，我们还将编辑 AppBar.vue 组件。就在我们的电影《拾夜者》片名之后，添加一个 *v-spacer* 和 *v-btn* 。我们将给出 logout 的按钮文本，绑定到稍后将创建的方法的 click 事件，并且仅在用户登录时显示按钮。

```
<v-app-bar *app* *color*="primary" *dark*>
 <v-app-bar-nav-icon @*click*.*stop*="drawer = !drawer" *v-if*="userId">
 </v-app-bar-nav-icon>
  <div *class*="d-flex align-center">
   <h2>Movie Night Picker</h2>
  </div>
 <v-spacer></v-spacer>

 **<v-btn *text* *v-if*="userId" @*click*="logoutUser">Logout</v-btn>**
</v-app-bar>
```

## Vuex

在我们的 Vuex 商店中，我们需要在注销时清除用户的状态。为此，我们需要在用户模块中有一个新的变化和动作:

```
//action
*clearUserData*(*context*){
 *context.commit*("CLEAR_USER_DATA");
}//mutation
*CLEAR_USER_DATA*:(*state*)=>{
 *state.id* = *null*;
 *state.name* = *null*;
 *state.email* = *null*;
 *state.partnerId* = *null*;
 *state.movieApiPage* =1;
}
```

## 注销方法

现在，回到 AppBar.vue 文件内部，我们需要创建注销用户的方法。在脚本标记的顶部，导入 firebase:

```
*import* firebase *from* 'firebase';
```

然后，编写注销用户的函数，清除用户的状态，并重定向回我们的登录页面。

```
methods: {
 *async* *logoutUser*() {
  *await* *firebase.auth*()*.signOut*();
  this*.$store.dispatch*("user/clearUserData");
  this*.$router.replace*("/");
 },
}
```

# 视频教程

Video Tutorial

# 结论

这就是火绒风格的电影《夜盗》的结尾。你可以对应用程序做很多改进，如果你做了，一定要分享它们。如果您有任何意见、问题或顾虑，请务必留下回复。下次见，编码快乐！