# JavaScript 中音调符号的去除并不明显

> 原文：<https://javascript.plainenglish.io/not-so-obvious-removal-of-diacritics-in-javascript-explained-and-done-right-52f4aeb3c85?source=collection_archive---------3----------------------->

## 解释清楚，做得对！

![](img/bbbab6fc99628470add15052bb51d73c.png)

# 快速介绍

拉丁字母在全世界被广泛使用。它是包括英语在内的许多欧洲语言的基础。事实上，英语字母表基本上与 ISO 基本拉丁字母相同，由 26 个字母组成:T0。

然而，所有其他非英语、拉丁字母都有一些特殊字母*。比如德语:`ä, ö, ü, ß`波兰语:`ą, ć, ę, ł, ń, ó, ś, ź, ż`或者葡萄牙语:`ã, õ, á, é, í, ó, ú, â, ê, ô, à, ç`。*

*基本上，这些是重音字母，音调符号。事实上，如果我们深入研究语言理论，我们会发现有趣的事实，比如:德语中的夏普 s 字符`ß`有时被认为是一个字母或连字。哇哦！？因此，德语单词 street: `Straße`也可以写成`Strasse`——两种拼法都是正确的。*

*另一个小事实:波兰语`ł`(带笔画的 l)是一个字母，而不是音调符号！如果你只是说:WAAAT——你感到困惑和惊讶——我也是！今天，当我试图用`l`代替`ł`时，我发现了这一点——事实上，这也是我决定一劳永逸地定义并解决这个问题的原因之一！*

*好了，以上是对重音字母和连字的简单介绍。我甚至不想经历所有的特例，语言和字母的古怪。基本上，我们想要实现的是一种将特殊字母映射到其最接近的 ISO basic 拉丁语对等词的方法。就这么简单。我们开始吧。*

# *不那么干净的代码*

*干净的代码，谁不喜欢干净的代码？与其使用一些模块或长映射函数，不如让我们看看 [ECMA 脚本 2015](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) 规范，并使用一些超级智能的两行程序。现在已经是 2020 年了，还会有什么问题呢？这个问题并不新鲜。对吗？不对！*

*在 ES6 中我们可以使用`String.prototype.normalize()`功能:*

```
*const str = "Crème Brulée";
const normalized = str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
console.log(normalized); // Outputs: Creme Brulee*
```

*到目前为止，一切顺利。两个字符:`è`和`é`都被替换为`e`。*

*让我们检查一下`straße` (ger)。街道)或`łokieć` (pol。肘部)。以上代码将分别产生:`straße`和`łokiec`—`ß`未被`ss`代替，`ł`未被`l`代替，只是`ć`如预期的那样被`c`代替。*

*不幸的是，ES6 不是这里的解决方案。*

# *不要重新发明轮子*

*我们的下一个解决方案是简单地映射特殊字母。这听起来是一个合理的解决方案。然而，在 2020 年，你会想象这个问题很久以前就已经解决了，不是吗？*

*事实上是。因此，不用担心德语、波兰语、葡萄牙语、法语和许多其他字母，不用重新发明轮子和编写替换发音符号函数，我们只需要寻找一个为我们这样做的 npm 模块。*

# *NPM*

*实际上，在我们开始之前，让我们定义一个通用的测试字符串:*

```
*const str = "Iлｔèｒｎåｔïｏｎɑｌíƶａｔï߀ԉ ą ć ę ł ń ó ś ź ż ä ö ü ß"*
```

*我在谷歌上发现的第一个模块是 [**发音符号**](https://www.npmjs.com/package/diacritics) 。它的用法很简单:*

```
*const removeDiacritics = require('diacritics').remove;
console.log(removeDiacritics(str));// OUTPUTS:
// Internationalizati0n a c e l n o s z z a o u ss*
```

*让我们注意两件事:波兰语和德语字母被正确替换了！国际化这个词末尾的第二个字符实际上是`0`而不是`o`。不伟大，不可怕。*

*接下来，搜索列表上是 [**发音符号**](https://www.npmjs.com/package/diacritic) :*

```
*const Diacritics = require('diacritic');
console.log(Diacritics.clean(str));// OUTPUTS:
// Internationalization a c e l n o s z z a o u ss*
```

***我们有赢家吗？**看起来是这样，不过我们再检查几个包裹吧。*

*第三位是 [**去除重音**](https://www.npmjs.com/package/remove-accents\) :*

```
*const removeAccents = require('remove-accents');
console.log(removeAccents(str));// OUTPUTS:
// Iлｔeｒｎaｔiｏｎɑｌiƶａｔi߀ԉ a c e l n o s z z a o u ß*
```

*这个看起来只转换音调符号，而不改变字母。*

*最后，我们来试试 [**归一化-发音符号**](https://www.npmjs.com/package/normalize-diacritics) :*

```
*const { normalize } = require('normalize-diacritics');
console.log(await normalize(str));// OUTPUTS:
// Iлternationɑlizati0ԉ a c e l n o s z z a o u s*
```

*与其他包相比，这个包做得最差——尤其是，我预计`ß`将被`ss`取代。*

# *结论*

*在经历了几种用拉丁字母替换特殊字母的方法后，我建议使用 [**发音符号**](https://www.npmjs.com/package/diacritic) 包装。这一个按照我期望的方式正确地替换了字母。*