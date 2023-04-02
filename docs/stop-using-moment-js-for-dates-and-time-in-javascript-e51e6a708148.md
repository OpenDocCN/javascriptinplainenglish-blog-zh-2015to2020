# 停止在 JavaScript 中使用 Moment.js 表示日期和时间

> 原文：<https://javascript.plainenglish.io/stop-using-moment-js-for-dates-and-time-in-javascript-e51e6a708148?source=collection_archive---------2----------------------->

## Luxon.js 是 Moment.js 的现代替代品

![](img/c939bcd89bdf34cb17318531406ea88e.png)

Photo by [Waldemar Brandt](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何使用过 JavaScript 的原生日期方法的人都知道它的局限性和随之而来的挫败感。对于原生日期对象，除了基本的格式化和操作之外的任何事情都很麻烦。

因此，大多数开发人员更喜欢使用其他库进行日期和时间操作。Moment.js 就是这样一个被广泛使用的库，然而，还有一个更好的库，我们中没有多少人知道或使用它。

Luxon.js 是一个 JavaScript 库，由 Moment.js 开发人员之一构建，以克服 Moment.js 包的缺点。由于它是由相同的开发人员构建的，您可能会发现两者之间有很多相似之处。

虽然我不会深入研究，因为这篇文章是面向安装和使用 Luxon 与普通 JavaScript，一些额外的使用 Luxon 的时刻列出如下

1.  卢克森是不可变的(不能突变或改变)
2.  更好的时区支持
3.  显式 API

## 装置

安装 Luxon.js 非常简单。安装和使用 Luxon.js 主要有三种方式:

1.  **手动加载-** 你可以从[这里](https://moment.github.io/luxon/global/luxon.js)安装下载完整的 Luxon.js，只需像这样`<script src=”luxon.js”></script>`包含 Luxon.js 脚本标签。
2.  **NPM-** Luxon.js 支持 Node 6+，通过运行`npm install — save luxon`你可以快速安装它。你必须在你的 JavaScript 文件中通过`const { DateTime } = require(“luxon”);`来引用它。
3.  这是我将在本文中使用的方法。你可以从[这里](https://www.jsdelivr.com/package/npm/luxon)获取最新的 CDN，并简单地将 HTML 粘贴到你的主 index.html 文件的 head 标签之间。

值得注意的是，为了支持 IE 10 或 11，您还需要在 HTML head 部分添加以下行:

```
<script src="https://cdn.polyfill.io/v3/polyfill.js?features=default,String.prototype.repeat,Array.prototype.find,Array.prototype.findIndex,Math.trunc,Math.sign,Object.is"></script>
```

## 入门指南

如前所述，我将使用 CDN 来安装 Luxon.js。因此，我需要在我的 HTML 文件中添加以下几行:

```
<script src="https://cdn.jsdelivr.net/npm/luxon@1.25.0/build/global/luxon.min.js"></script>
```

这就是我们真正需要开始的全部内容。

我们现在可以使用 Luxon 提供的所有功能和工具。为了简单起见，我不会使用外部 JS 文件，而是将代码写在我们的 HTML 文件中。

Basic DateTime object

[DateTime.local](https://moment.github.io/luxon/docs/class/src/datetime.js~DateTime.html#static-method-local) 接受任意数量的参数，一直到毫秒。本质上，这只是一个 Javascript 日期对象。

下面是我们的 HTML 文件现在的样子:

# 格式化日期时间

Luxon 提供了许多工具来使日期和时间可读，但其中两个是最重要的。如果你想格式化一个人类可读的字符串，使用`toLocaleString`:

```
var dt= luxon.DateTime().local()
dt.toLocaleString()      //=> '9/14/2017'
dt.toLocaleString(DateTime.DATETIME_MED) //=> 'September 14, 3:21 AM'
```

通过让浏览器决定不同部分的顺序以及如何给它们加上标点符号，这种方法在不同的地区都适用。

但是，如果您想让另一个程序读取该字符串，或者如果您想将它存储在数据库中，您可能想使用`toISO`:

```
dt.toISO() //=> '2017-09-14T03:21:47.070-04:00'
```

还支持自定义格式。官方文件提供了进一步[格式化](https://moment.github.io/luxon/docs/manual/formatting)的细节。

## 数学运算

Luxon 使日期和时间的数学运算变得非常容易。扣除或增加天数和小时数是 Luxon.js 支持的一些操作。

下面是演示加法和减法运算的代码片段。

```
var DateTime= luxon.DateTime()
var dt = DateTime.local();
dt.plus({hours:45, minutes: 23});
dt.minus({days: 9});
dt.startOf('day');
dt.endOf('hour');
```

如您所见，方法`minus`和`plus`需要一个 JSON 对象作为参数。

## 国际的

Luxon 的主要亮点之一是它对 Intl 功能的支持。一些最重要的是格式化:

```
var dt = DateTime.local();
var f = {month: 'long', day: 'numeric'};
dt.setLocale('fr').toLocaleString(f)      //=> '14 septembre'
dt.setLocale('en-GB').toLocaleString(f)   //=> '14 September'
dt.setLocale('en-US').toLocaleString(f)  //=> 'September 14'
```

Luxon 的 Info 类可以用来列出不同地区的日期和时间:

```
Info.months('long', {locale: 'fr'}) //=> [ 'janvier', 'février', 'mars', 'avril', ... ]
```

# 期间

很多时候我们需要存储一个会话或一个故事的持续时间。Luxon 提供了一个 Duration 类，可以用来以一种非常容易理解的方式表示时间。您可以像这样创建它们:

```
var dur = Duration.fromObject({hours: 2, minutes: 7});
```

此外，您可以使用这些 duration 对象添加或减去 DateTime 对象，如下所示。

```
dt.plus(dur);
```

Luxon 默认提供各种 getters。这使得访问存储在这些 Duration 对象中的数据变得非常容易。下面列出了一些 getters:

```
dur.hours   //=> 2
dur.minutes //=> 7
dur.seconds //=> 0dur.as('seconds') //=> 7620
dur.toObject()    //=> { hours: 2, minutes: 7 }
dur.toISO()       //=> 'PT2H7M'
```

我已经陈述了一些主要的和最常用的功能。你可以在他们的官方[持续时间 API 文档](https://moment.github.io/luxon/docs/class/src/duration.js~Duration.html)中找到关于这个主题的所有资源和更多信息。

# 日历

Luxon 对非标准日历也有极好的支持。

根据他们的[官方文档](https://moment.github.io/luxon/docs/manual/tour.html)，Luxon 完全支持公历和 ISO 周历。通俗地说，Luxon 可以解析这些日历中指定的日期，使用这些日历将日期格式化为字符串，使用这些日历的单位转换日期，等等。

例如，这是 Luxon 直接使用 ISO 日历:

```
DateTime.fromISO('2017-W23-3').plus({ weeks: 1, days: 2 }).toISOWeekDate(); //=>  '2017-W24-5'
```

虽然，值得一提的是，使用非支架日历是一个非常罕见的场景，但仍然很高兴知道 Luxon 支持广泛的日历标准。

# 时区

Luxon 支持不同的时区。像往常一样，官方文件有整整[大节](https://moment.github.io/luxon/docs/manual/zones)关于它。用户可以从全球任何地方访问您的数据，因此必须以当地时区显示时间，以增强用户体验。

您可以在特定区域创建日期时间，并更改它们的区域:

```
DateTime.fromObject({zone: 'America/Los_Angeles'}) // now, but expressed in LA's local time
DateTime.local().setZone('America/Los_Angeles') // same
```

Luxon 还直接支持 UTC:

```
DateTime.utc(2017, 5, 15);
DateTime.utc();
DateTime.local().toUTC();
DateTime.utc().toLocal();
```

## 有效性检查

我最喜欢的 Luxon 特性之一是它验证日期和时间的能力。在使用日历时，得到类似“9 月 45 日”的输入是很常见的。然而，Luxon 承认这些错误，并相应地标记它们。

它提供了一个`isValid`方法来检查日期是否有效。

```
var dt = DateTime.fromObject({ month: 2, day: 400 }); 
dt.isValid //=> false
dt.toString(); //=> 'Invalid DateTime'
```

转换无效的 DateTime 对象会返回一个明确的错误“无效的 DateTime ”,这在向用户显示错误日志时非常方便。

## 结论

js 是一个令人兴奋而简单的日期时间库。它无疑克服了 JavaScript 的原生 DateTime 对象的所有缺点，同时为常见的日常用例(如时区和持续时间)提供了重要的支持。

Luxon.js 可以被认为是 Moment.js 的现代时尚替代品，它借鉴了 Moment.js 的许多想法和概念，并建立在它的基础上。

然而，尽管人们会发现这两个包之间有许多相似之处，但 Luxon 也有一些微妙的不同，比如 Luxon 方法经常将 option 对象作为它们的最后一个参数。你可以在他们的官方网站上找到差异的完整列表。

至于未来的支持，开发者已经[声称会无限期支持。但是即使是当前版本的 Luxon 也是 Moment.js 的成熟替代品。](https://moment.github.io/luxon/docs/manual/why.html#future-plans:~:text=I%20plan%20to%20support%20it%20indefinitely)

在你的下一个项目中使用 Luxon over Moment.js 绝对值得。