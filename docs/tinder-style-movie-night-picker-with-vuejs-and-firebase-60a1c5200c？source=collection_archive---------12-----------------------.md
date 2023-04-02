# 火绒风格的电影夜采摘与 VueJs，Vuetify，VuexFire，和 Firebase

> 原文：<https://javascript.plainenglish.io/tinder-style-movie-night-picker-with-vuejs-and-firebase-60a1c5200c?source=collection_archive---------12----------------------->

## 第 4 部分—电影

![](img/e59d3a6be75c61ed982099a040200d18.png)

Image by [stokpic](https://pixabay.com/users/stokpic-692575/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2545676) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2545676)

在第三部分中，我们致力于添加合作伙伴。现在，让我们把重点放在电影的展示和投票上。为此，我们将使用[电影数据库](https://www.themoviedb.org/)。这个 API 完全可以免费使用**，只要你正确地给数据源**赋予属性。

要获得 API 密钥，请前往[https://www.themoviedb.org/](https://www.themoviedb.org/)并创建一个帐户。然后点击右上角的您的帐户。接下来，单击下拉菜单中的设置，并选择左侧的 API 选项。然后你必须申请一个 API 密匙，这是即时的。

## 发现 API 端点

我们将用来显示电影的这个特定端点就是 discover 端点。这将允许我们以可预测的方式向用户展示随机电影。因此，请阅读此端点的[文档](https://developers.themoviedb.org/3/discover/movie-discover)来熟悉一下。在那里，您会发现一个开发人员友好的界面来创建确切的端点。

在我们的使用中，我们将使用缺省端点并稍作修改来返回成人电影。我将使用文档接口来调整生成该 URL 的查询参数:

```
[https://api.themoviedb.org/3/discover/movie?api_key=<YOUR_API_KEY>&language=en-US&sort_by=popularity.desc&include_adult=true&include_video=false&page=1](https://api.themoviedb.org/3/discover/movie?api_key=c9b1dcef3fc31d3fd438f59ca9214638&language=en-US&sort_by=popularity.desc&include_adult=true&include_video=false&page=1)
```

现在我们有了端点，当用户登陆主页时，我们将使用 Axios 调用它。由于 API 一次只返回 10 部电影，我们还需要跟踪结果的索引，并进行新的调用来获取下一页的结果。现在，我们将使用我们的方法来发出请求。

## Home.vue

在 src/views/Home.vue 中，在 options API 中制作一个数据道具，并添加一个名为 movies 的数组。这将保留我们请求的电影。我们还将添加一个 currentMovie 和 currentIndex 属性，这样我们就知道屏幕上当前播放的是什么电影。最后，我们将创建一个 isLoading 属性，在获取电影时显示一个覆盖图。

```
*data*: () => ({
 isLoading: false,
 movies: [],
 currentMovie: {},
 currentIndex: 0
})
```

接下来，我们将创建从数据库获取电影的方法。在脚本的顶部标记 import axios:

```
*import* axios *from* 'axios'
```

然后，在选项 API 中创建一个方法属性，并添加以下函数:

fetchMovies

这样用户就不会一遍又一遍地看同样的电影，我们也会跟踪他们最后一次浏览的页面。为此，我们需要更新我们的 Vuex 用户模块。因此，请转到 src/store/modules/user.js。我们将添加一个名为 movieApiPage 的新状态属性，一个用于更新它的新变体，以及一个我们可以调度的操作。您的用户模块应该如下所示:

Vuex user module

现在，当用户登录时，如果我们在数据库中找到该属性，我们需要将它传递给商店。在 src/views/SignIn.vue 中找到我们设置用户数据并添加 movieApiPage 的商店分派。

```
this*.$store.dispatch*('user/setUserData', {
 id: *dbUser.*id,
 name: *userData.*name,
 email: *userData.*email,
 partnerId: *userData.*partnerId || '',
 **movieApiPage: *userData.*movieApiPage || 1**
})
```

之后，我们将创建一些计算属性，使我们的代码更容易编写:

Home Computed props

接下来，我们需要拇指向上和拇指向下点击的逻辑。因为它们都需要增加当前电影/索引，我们将添加一个 increment 方法。在这个方法中，我们必须检查电影是否是当前页面的最后一部。如果是，我们将需要获取更多内容，并将页码保存在用户的集合中。

incrementCurrentIndex

现在，我们可以实现拇指向上和向下逻辑:

thumbsUp() & thumbsDown()

我们需要做的最后一件事是在页面加载时填充我们的电影。为此，我们将使用创建的方法并触发 fetchMovies 方法:

Home created()

现在我们已经完成了所有的核心逻辑，我们可以构建我们的模板了。我们会有一张卡片，上面有电影海报、标题、描述和竖起/放下的拇指。还有，别忘了电影数据库的属性。

Home.vue template

# 视频教程

Video Tutorial

# 结论

现在我们有了电影和匹配逻辑，我们可以开始专注于包装应用程序。在[第 5 部分](https://diligentdev.medium.com/tinder-style-movie-night-picker-with-vuejs-and-firebase-d62d6c39feb9)中，我们将着重于显示匹配。这也给了我们一个机会来完善我们忽略的任何错误或事情。当它出来的时候，请确保你跟随我得到更新！