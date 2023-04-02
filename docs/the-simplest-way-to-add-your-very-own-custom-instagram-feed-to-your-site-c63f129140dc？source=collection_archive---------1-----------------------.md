# 如何将 Instagram feed 添加到您的网站📷

> 原文：<https://javascript.plainenglish.io/the-simplest-way-to-add-your-very-own-custom-instagram-feed-to-your-site-c63f129140dc?source=collection_archive---------1----------------------->

![](img/e145fc51f1b997aa16357731f9af3c46.png)

Don’t worry, you can make the feed look way better than this!

要使 Instagram API 正常工作，需要以下位:

**1—insta feed . js(**[](https://github.com/stevenschobert/instafeed.js/)****)****

****2 —您的 instagram 账户用户 ID(**[**)https://smash balloon . com/insta gram-feed/find-insta gram-user-ID/**](https://smashballoon.com/instagram-feed/find-instagram-user-id/)**)****

****3 —一个 API 密钥(稍后将详细介绍)****

****4 —激活提要的神奇 JS 代码(稍后将详细介绍)****

**让我们仔细检查每一点，并解释你需要做什么。**

# **1.instafeed.js**

**这是一个简单地从 github 链接添加 instafeed.js 文件的例子:[https://github.com/stevenschobert/instafeed.js/](https://github.com/stevenschobert/instafeed.js/)没有必要篡改它，它只是简单地工作。这是一段神奇的 JS 代码，你必须(稍微)摆弄一下才能让它工作。**

# **2.您的 Instagram 帐户用户 ID**

**为此，只需访问[https://smash balloon . com/instagram-feed/find-insta gram-user-id/](https://smashballoon.com/instagram-feed/find-instagram-user-id/)并输入您的 insta gram 帐户名称。如果这没有意义，这里有一个例子:我的 instagram 帐户可以位于 https://[www.instagram.com/sunilsandhu](www.instagram.com/sunilsandhu)因此，我的帐户名称是 sunilsandhu 将它放入 smashballoon 链接，它会给你你的用户 id——它很长，但请确保你复制了整个该死的东西，因为你一会儿会需要它。**

# **3.API 密钥**

**这可能有点尴尬，但遵循这一点，你会得到它的工作:**

1.  **首先，进入 https://[www.instagram.com/developer/register/](www.instagram.com/developer/register/)——它会要求你登录(如果你还没有的话)；**
2.  **然后你应该被带到 https://[www.instagram.com/developer/](www.instagram.com/developer/)——你会看到一个按钮，上面写着“注册你的应用程序”。按下那个按钮。去吧，我敢！**
3.  **你现在应该在 https://[www.instagram.com/developer/clients/manage/](www.instagram.com/developer/clients/manage/)上——你能看到右上角写着“注册新客户”吗？单击该按钮。**
4.  **你现在在 https://[www.instagram.com/developer/clients/register/](www.instagram.com/developer/clients/register/)上——填写那个讨厌的表格！**
5.  **一旦你填好了，你会看到一些细节。这个很长的客户端 ID 很重要，因为我们以后会用到它。**

# **4.激活提要的神奇 JS 代码**

1.  **一旦你想好了 Instagram feed 在你的站点上的位置，给这个元素一个 instafeed 的 id。这有意义吗？我当然希望如此。如果没有，我在我的网站上放了下面的代码来实现它:`<div id="instafeed"></div>`**
2.  **首先要做的第二件事是:确保在代码中包含了 instafeed.js 文件。这将需要在我们下一点将要讨论的代码之上(但不需要在我们在第 0 点中制作的 div 标签之上)。**
3.  **有很多方法可以实现特定类型的提要，无论您希望它获取您自己的提要，还是搜索特定的标签。你可以在 readme 中找到更多关于这样做的内容，可以在这里找到[https://github . com/stevenschobert/insta feed . js/blob/master/readme . MD](https://github.com/stevenschobert/instafeed.js/blob/master/README.md)总之，下面是我们如何实现一个提要:**

```
var feed = new Instafeed({
            get: 'user',
            userId: '5729620331',
            template: '<a href="{{link}}"><img class="insta-image" src="{{image}}" /></a>',
            accessToken: 'superduperlongcodethatwillsithere'
        });
        feed.run();
```

**让我们分解每一行来帮助理解它。**

1.  **`var feed = new Instafeed({`创建 Instafeed 的新实例，并将其包装到一个变量中**
2.  **`get: 'user',`这决定了我们想要的饲料类型。在本例中，我们选择了获取自己的提要**
3.  **`userId: '5729620331',`这是您之前从 https://smash balloon . com/insta gram-feed/find-insta gram-user-ID/获得的 ID**
4.  **`template: '<a href="{{link}}"><img class="insta-image" src="{{image}}" /></a>',` 这基本上就是 JS 添加到你的页面上的东西，对于 API 输入的每一张图片。你会在我的上面看到我添加了一个名为 insta-image 的类，它应用了我制作的 CSS 样式**
5.  **`accessToken: 'superduperlongcodethatwillsithere'`这是在 http://instagram.pixelunion.net/生成的——在访问本网站之前，您必须登录您想要验证的 Instagram 账户，因为它会要求您在继续之前授权应用程序**
6.  **`});`这些是 var feed 新实例的结束标记**
7.  **这将执行一切**

**这就是你想要在你的网站上获得实时 instagram feed 所需要做的一切！**

**这篇文章是由我工作的创意公司 [**Serosensa 创意公司**](https://www.serosensa.com) **-** 赞助的，他们允许我花一些时间为所有可爱的读者写这篇文章。你可以在他们的网站上查看更多他们的作品:[https://www.serosensa.com](https://www.serosensa.com)**