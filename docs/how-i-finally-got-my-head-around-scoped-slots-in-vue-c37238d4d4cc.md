# 我最终是如何理解 Vue 中的作用域插槽的

> 原文：<https://javascript.plainenglish.io/how-i-finally-got-my-head-around-scoped-slots-in-vue-c37238d4d4cc?source=collection_archive---------0----------------------->

![](img/4102ebea6c828473d7c306a50c062c73.png)

Vue 是一个前端框架，用于构建 web 应用程序，其设计方式可以让开发人员很快变得高效。关于这个框架的各个方面都有大量的信息，而且这个社区每天都在增长。如果你正在读这篇文章，很可能你已经知道了。

虽然启动和运行起来既快又简单，但该框架的一些元素更复杂、更强大，需要更多的脑力(至少对我来说)才能理解。其中一个领域是插槽，以及相关但功能有些不同的作用域插槽。我花了一段时间才理解老虎机是如何工作的，所以当我理解后，我认为值得分享一下我对老虎机的看法，以防对其他人有所帮助。

# 插槽和命名插槽

常规插槽是父组件向标准 Props 机制之外的子组件发送一些信息的一种方式。我发现这有助于我将这种方法与常规 HTML 元素联系起来。

例如，以 HTML 标签为例。

```
<a href=”/sometarget">This is a link</a>
```

如果这是 Vue，而是你的组件，那么你将把文本“这是一个链接”发送到“a”组件，它将把它呈现为一个超链接，并把“这是一个链接”作为该链接的文本。

让我们定义一个子组件来展示这是如何工作的:

```
<template>
  <div>
    *<slot></slot>
  </div>* </template>
```

然后从父节点开始，我们这样做:

```
<template>
  <div>
    *<child-component>This is from outside</child-component>
  </div>
</template>*
```

正如您所料，我们在屏幕上看到的是“这是从外面来的”，但是是由子组件呈现的。

我们还可以在子组件中添加默认信息，以防像这样什么都没有传入:

```
<template>
  <div>
    <slot>Some default message</slot>
  </div>
</template>
```

如果我们像这样创建子组件:

```
<child-component>
</child-component>
```

我们看到屏幕上显示“一些默认消息”。

命名槽与常规槽非常相似，只是在目标组件中可以有多个发送文本的位置。

让我们更新子组件，以包含一些命名的插槽

```
<template>
  <div>
    <slot>Some default message</slot>
    <br/>
    <slot *name*="top"></slot>
    <br/>
    <slot *name*="bottom"></slot>
  </div>
</template>
```

这里，我们在子组件中有三个插槽。两个有名字——顶部和底部。

让我们更新父组件来利用这一点。

```
<child-component *v-slot:top*>
Hello there!
</child-component>
```

注意—我们在这里使用新的 Vue 2.6 符号来指定我们想要定位的插槽:` v-slot:theName '

你希望在屏幕上看到什么？如果你说“你好，托普！”你只说对了一部分。

因为我们没有为未命名的槽提供任何值，所以我们也获得了默认值。所以我们实际上看到的是:

一些默认消息
你好！

在幕后，未命名的插槽被称为“默认”，因此您也可以使用:

```
<child-component *v-slot:default*>
Hello There!
</child-component>
```

我们只会看到:

你好。

因为我们现在提供了默认/未命名插槽的值，而命名插槽“top”或“bottom”都没有默认值。

你发送的内容不一定只是文本，也可以是其他组件或 HTML。您正在发送供显示的内容。

# 作用域插槽

![](img/e03302b66fbb4a926b6f4903635f44f7.png)

我认为吃角子老虎机和命名吃角子老虎机是相对简单的，一旦你玩了一会儿，你就可以理解了。另一方面，作用域插槽虽然共享相同的名称，但却有些不同。

我倾向于认为作用域插槽有点像投影仪(或我的欧洲朋友的投影仪)。原因如下。

子组件中的作用域槽可以使用槽来提供用于在父组件中呈现的数据。这就像有人拿着投影仪站在你的子组件中，将一些图像投射到你的父组件的墙上。

这里有一个例子。在子组件中，我们像这样设置一个插槽:

```
<template>
  <div>
    <slot *name*="top" *:myUser*="user"></slot>
    <br/>
    <slot *name*="bottom"></slot>
    <br/>
  </div>
</template><script>data() {
  *return* {
    user: "Ross"
  }
}</script>
```

请注意，名为“top”的插槽现在有一个名为“myUser”的属性，我们将它绑定到“User”中包含的一个反应数据值。

在我们的父组件中，我们像这样设置子组件:

```
<div>
   <child-component *v-slot:top*="slotProps">{{ slotProps }}</child-component>
</div>
```

我们在屏幕上看到的是:

{ "myUser": "Ross" }

用投影仪做类比，我们的子组件通过 myUser 对象将其用户字符串的值传送给父组件。它在父对象中投影到的墙壁称为“slotProps”。

我知道这不是一个完美的类比，但当我第一次发现发生了什么时，它帮助我以这种方式思考这个问题。

Vue 文档非常优秀，我也看到了很多其他关于作用域插槽如何在线工作的描述，但是很多人似乎采用了将父类中的所有或部分属性命名为子类中的属性的方法，对我来说，这很难理解到底发生了什么。

通过在父类中使用 ES6 析构，我们还可以通过编写以下代码将用户明确拉出 slotProps(您可以随意称呼它):

```
<child-component *v-slot:top*="{myUser}">{{ myUser }}</child-component>
```

或者甚至在父代中给它起一个新名字:

```
<child-component *v-slot:top*="{myUser: aFancyName}">{{ aFancyName }}</child-component>
```

都只是 ES6 的破坏，和 Vue 没什么关系。

如果你是从 Vue 和吃角子老虎机开始你的旅程，希望这能助你一臂之力，揭开一些更棘手的部分。

## **简明英语团队的一份说明**

你知道我们有四种出版物吗？给他们一个关注来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****