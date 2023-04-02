# 您需要尝试 3 个更棒但更小的 JavaScript 库

> 原文：<https://javascript.plainenglish.io/3-more-awesome-but-small-javascript-libraries-you-need-to-try-2f756ef9a2b?source=collection_archive---------0----------------------->

## 非常感谢你们——这是承诺给你们的第二部分

![](img/8fd99610f550695295b90f498d222672.png)

Here is the second part with more cool libraries!

首先，非常感谢您对本系列第一部分的精彩反馈——我收到了许多消息，这篇文章可能会对您有所帮助，您也了解了一些很酷的新 JS 库。
如果你没有检查第一部分，你可以这样做:

[](https://medium.com/javascript-in-plain-english/3-awesome-but-small-javascript-libraries-you-need-to-try-d43df5c0b5dd) [## 你需要尝试 3 个很棒但很小的 JavaScript 库

### 我将向您展示 3 个不太为人所知但非常强大的小型 JavaScript 库，它们将使您的生活变得更加轻松

medium.com](https://medium.com/javascript-in-plain-english/3-awesome-but-small-javascript-libraries-you-need-to-try-d43df5c0b5dd) 

当然，在第二部分，我也想这样做。
本文将向您展示 3 个不太知名但非常强大的小型 JavaScript 库，它们将让您的生活(以及您的客户的生活)变得轻松，而不会让您的 web 应用程序变得不必要的繁重。

# 1.)使用 xtype.js 进行可靠的数据验证

你有没有遇到过这样的问题，你想在 JavaScript 中检查类型，但是 Vanilla JS 的 typeof 函数给你的结果不可靠？

## 如果你不知道我的意思，那么看看这一团乱

```
typeof(new Date())                     *// object*typeof([‘Max’, ‘Carl’, ‘Tom’])         *// object*typeof({ name: ‘Max’, age: 20 })       *// object*typeof(/ab+c/i)                        *// object*typeof(new Error(‘Error’))             *// object*
```

但是使用 **xtype.js** ，有一个解决方案:
你可以通过 NPM 安装它，以便与 React 或 Node.js 一起使用

```
npm install xtype
```

或者当你[下载](http://xtype.js.org/getit/download)它的时候在浏览器中使用它。但是首先，让我们看看如何解决这个问题:

```
xtype.type(new Date())                 *// date*xtype.type([‘Max’, ‘Carl’, ‘Tom’])     *// array*xtype.type({ name: ‘Max’, age: 20 })   *// object*xtype.type(/ab+c/i)                    *// regexp*xtype.type(new Error(‘Error’))         *// error*
```

酷吧。情况越来越好。因为通过不使用**类型**函数，而是直接传递值，我们还可以获得关于我们的值的更多细节。

```
xtype.type(-1.1)                       // number// more details please:xtype(-1.1)                            // negative_floatxtype(1)                               // positive_integerxtype('')                              // empty_string xtype('  ')                            // whitespacextype(["Max", "Carl"])                 // multi_elem_arrayxtype(["Max"])                         // single_elem_array
```

## 这难道不令人惊讶吗？

而且只有 3kB 大小。它在浏览器和 Node.js 中也能工作:)
提示:在
[官方文档](https://xtype.js.org/)中有更多的内容可以发现。

# 2.)用 Mousetrap.js 处理浏览器中的键盘快捷键

尤其是在构建更复杂的 UI 时，为用户提供键盘快捷键是很有帮助的，甚至是必要的。当然，这在视频游戏或类似游戏中也是可以想象的。无论如何，这是给你的只有 2.2kB 的捕鼠器

获取代码，比如从一个 CDN:[https://Craig . global . SSL . fastly . net/js/mouse trap/mouse trap . min . js？a4098](https://craig.global.ssl.fastly.net/js/mousetrap/mousetrap.min.js?a4098)
导入到你网站的<头>里。
**或:**
*npm 安装捕鼠器
从‘捕鼠器’导入捕鼠器*

## 让我们来看几个例子

绑定单个键:

```
Mousetrap.bind(‘4’, function() {
  console.log(‘you pressed 4’)
})
```

**重要提示:**被定义或按下的是大写还是小写字母是有区别的。比如捕鼠器，x 不等于 x。

绑定一个单键，按住**键，当**键出现时触发该功能。(因为**按键**的设置)

```
Mousetrap.bind(‘x’, function() {
  console.log(‘You pressed x and let it come up’)
},‘keyup’)
```

当 a、b、c 键按顺序按下**时触发。必须遵守指定的顺序。**

```
Mousetrap.bind(‘a b c’, function() { 
  console.log(‘You pressed a, then b & after that you pressed c’)
})
```

创建组合键

```
Mousetrap.bind(‘command+shift+k’, function(e) {
  console.log(‘you pressed command, shift & k’)
})
```

应该以相同方式反应的几个组合键可以用一个数组传递

```
Mousetrap.bind([‘a+d’, ‘w+s’], function(e) {
  console.log(‘you pressed a & d OR w & s’)
})
```

更多信息，请查看官方文档:)

# 3.)用 Countable.js 分析用户输入

使用 Countable.js，您可以实时统计用户在文本字段中输入的内容。不管是关于数字符、单词还是段落。

先说好消息:有一个[现场演示](https://sacha.me/Countable/#demo)可用。但是让我们用它来建造我们自己的东西

从[https://github.com/RadLikeWhoa/Countable/archive/master.zip](https://github.com/RadLikeWhoa/Countable/archive/master.zip)下载库或者通过 npm 安装:
NPM 安装可数

```
<textarea id=”text”></textarea><script>
  Countable.count(document.getElementById(‘text’), counter =>
    console.log(counter)
  )
</script>
```

现在您应该看到:

```
{paragraphs: 0, sentences: 0, words: 0, characters: 0, all: 0}
```

使用 **Countable.count** 函数，我们可以获得文本字段的当前状态。但是如果我们现在改变其中的任何内容，该函数将不会被再次调用。

文本字段内容的每次变化的触发可以通过 **Countable.on** 函数实现

```
Countable.on(document.getElementById(‘text’), counter =>
  console.log(counter)
)
```

每当我们现在更改输入，就会出现一个新的 console.log

我觉得一点也不难，也不复杂。
因此 gzip&只缩小了 **1kB** 大:)

## 加入我的邮件，接收你感兴趣的一切

## **关于我，作者:)**

嗨！非常感谢您的阅读，我叫路易斯，是一名来自德国的 18 岁学生。我热爱 Web 开发，包括后端和前端。我最喜欢的技术是 React、Vue、React Native 和 node . js。
请务必关注我，了解更多与这些相关的内容，并随时查看我的 IG @ Louis . jsx&@ codingcultureshop

给我一些掌声和反馈——这不花任何钱:)
**祝你有美好的一天！**