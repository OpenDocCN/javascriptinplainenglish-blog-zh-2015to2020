# 前端开发人员可以做得更好的事情

> 原文：<https://javascript.plainenglish.io/things-that-front-end-developers-can-do-to-get-better-a6be36c22550?source=collection_archive---------5----------------------->

![](img/f5846080aff7f8ba9398fa0dbc9d44c9.png)

Photo by [Erik Lucatero](https://unsplash.com/@erik_lucatero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

开发人员很容易犯错误，但如果我们事先知道这些错误，我们就可以防止它们。

在本文中，我们将看看作为前端开发人员我们可以做得更好的事情。

# 不抽象元素

我们应该将 CSS 选择器提取到独立的块中，这样，如果同一个类中的元素可能在当前父元素之外，我们就可以使用相同的样式。

例如，如果我们有下面的 HTML:

```
<div class='parent'>
  <p class='paragraph'>
    paragraph
  </p>
</div>
```

然后，代替编写下面的 CSS:

```
.parent .paragraph {
  font-weight: bold;
}
```

我们应该改为写:

```
.paragraph {
  font-weight: bold;
}
```

现在，我们可以用相同的粗体样式对段落类中的任何内容进行样式化。

# 不使用 Flexbox 或 CSS 网格

Flexbox 和 grid 是 CSS 的两个最好的特性。它使布局比使用浮动更容易，并且比使用表格布局领先很多。

因此，我们应该利用它们。Flexbox 的工作原理是通过添加`display: flex`属性来创建一个我们想要使用 flexbox 的容器。

然后我们可以用`justify-content`属性来安排它们。我们可以将容器内的物品放在两端、并排或居中。

我们可以通过 flexbox 改变许多选项。

网格是伟大的布局，否则反应。如果我们想用 CSS 网格做一个布局。我们只需指定用于布局的类，然后在 CSS 网格的值中对它们进行排序，以便它们按照在值中的排列方式对齐。

例如，我们可以像这样编写 CSS:

```
.a {
  grid-area: header;
}.b {
  grid-area: main;
}.c {
  grid-area: sidebar;
}.item-d {
  grid-area: footer;
}.container {
  display: grid;
  grid-template-columns: 30px;x;
  grid-template-rows: auto;
  grid-template-areas:
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

然后我们用`grid-template-areas`属性的值指定`header`、`main`、`sidebar`和`footer`元素在网格中的位置。

`grid-area`指定哪些元素是`header`、`main`、`sidebar`或`footer`。

# HTML 中的大写字母

我们不必在 HTML 代码中显式地写大写字母。相反，我们可以对一个元素使用 `text-transform: uppercase;`属性。然后我们将显示大写字母，而不是之前的大小写。

![](img/92e1304b7435e36c422e2ffe39027b46.png)

Photo by [Lucrezia Carnelos](https://unsplash.com/@ciabattespugnose?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 学会计划

如果我们必须开发任何有价值的东西，计划总是很重要的。所以要学会规划，这样才能思考到所有可能发生的问题，并据此进行规划。

我们真的只需要分解问题，这样我们就可以轻松地完成任务。

小事情更容易完成，所以我们应该这样做。

# 保存我们自己的代码样本

保存我们自己的代码以便我们以后可以使用是一个好主意。如果它们有用，我们就应该保留它们。

那我们就不用再去摸索怎么做一件事了。

# 知道什么时候说不

我们不必事事都答应。有些事做不到，不如说不。

与其让某人失望，不如现在说不。

过于随和意味着我们贬低自己。如果某件事不可行或者不好，那么我们应该对他们说不。

# 保持健康和活跃

健康和活跃是开发人员工作的重要组成部分。否则，我们会紧张，疲惫，筋疲力尽。

这意味着我们不会做好我们的工作。因此，我们应该致力于保持健康和活跃。

经常休息，这样我们就不必不停地思考我们的工作。一旦充好电，我们就可以继续工作了。

像散步、去健身房、骑自行车等运动。都是锻炼身体的好方法。

# 结论

为了成为更好的前端开发人员，我们可以做很多事情。

我们应该学习 flexbox 和 grid，这将使布局比没有它们容易得多。

此外，我们应该学会如何与他人相处。如果某事不好或不可行，我们可以说不。

休息一下，保持健康和活跃是很重要的。这样，我们就可以在休息后为工作做好准备。

# **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**