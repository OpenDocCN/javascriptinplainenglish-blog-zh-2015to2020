# NPM 和 Git 的自动版本控制

> 原文：<https://javascript.plainenglish.io/auto-versioning-with-npm-and-git-786c6795b926?source=collection_archive---------7----------------------->

![](img/a311c07b5f870ca3ff7dceb3846a08b4.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我花了 9 个月的时间学习 Vue.js、Vuetify 和 electronic。这间接意味着我对 NPM 和 Git 的理解比以前有了很大的提高。

我厌倦了不得不记住更新我的版本号，所以我决定寻找自动版本控制的方法。我没有找到任何一个能按我想要的方式工作的，所以我想出了一个我自己的。

简单到了*为什么我没有发现这一点？*所以，我决定写这篇小文章，这样其他人可能会发现它，而不必担心为什么网上没有提到它。这是最好的方法吗？我不知道，但对我来说效果很好。

**方法**

在我的 *package.json* 文件中，我添加了三个简单的脚本。

```
{
  ... scripts: { ...
    "push": "npm version patch && git push",
    "push-minor": "npm version minor && git push",
    "push-major": "npm version major && git push"
  }
}
```

**结果**

如果您熟悉这些命令，这是不言自明的。否则，*NPM version[patch | minor | major]*会在 *package.json* 文件中适当增加版本号。该命令自动将版本更新提交给本地 git repo，然后 git push 推送到远程 repo。

显然，如果您没有使用遥控器，您可以从这些脚本中删除 *git push* 。如果我这样做了，我可能也会将命令更改为更相关的内容。

**用途**

同样，如果你已经熟悉了 *npm* 或 *yarn* (我个人的选择)，你就已经明白如何使用这些脚本了。否则，您可以从命令行键入 *npm run [push，push-minor，push-major]* 或 *yarn [push，push-minor，push-major]。*

如果我决定提交一个 commit，并且认为它不需要新版本，我只需输入标准的 *git push* 命令。

**影响**

对我来说，这简化了版本控制。我不用考虑那么多。我只需将我之前的 *git push* 命令改为 *yarn push* ，版本补丁在推送前递增。因为补丁级别的修订对我来说是最难记住的，所以我把它设为默认。

**总结**

这个简单的方法帮助我在多个项目中保持版本有序。还有其他的自动版本控制方法，例如，git 的*预推送*或*预提交*挂钩，但是这种方法具有最低的设置成本，并且肯定满足我的需求。希望别人觉得有用。

类似的方法也可以用于其他版本工具，比如 SVN。

*更多内容看* [***说白了. io***](http://plainenglish.io/) ***。*** *报名参加我们的* [***免费每周简讯点击这里***](http://newsletter.plainenglish.io/) ***。***