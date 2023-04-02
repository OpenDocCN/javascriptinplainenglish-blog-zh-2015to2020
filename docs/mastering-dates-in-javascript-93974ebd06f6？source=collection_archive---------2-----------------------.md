# 在 JavaScript 中掌握日期

> 原文：<https://javascript.plainenglish.io/mastering-dates-in-javascript-93974ebd06f6?source=collection_archive---------2----------------------->

## 如何不因日期对象的不足而沮丧

![](img/527312b4b6ae5d0c7b3b9403de34f3ca.png)

Photo by [Jess Bailey](https://unsplash.com/@jessbaileydesigns?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](/s/photos/day-and-time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 语言最不令人满意的方面之一是日期对象的管理，尤其是考虑到没有经验的程序员不太习惯处理日期。

我为什么这么说？在我看来，最根本的缺失是有效的方法:

1.  格式化日期
2.  将表示日期的字符串转换为 date 对象
3.  执行*日期运算，*例如计算两个日期之间的距离或添加小时、天、月等。，到给定的日期

在接下来的几节中，我将展示如何用不必要的复杂解决方案来弥补 JavaScript 中的这些缺点，以及相反，一组奇妙的库如何让您直接获得相同的结果。

# JavaScript 中的日期是什么

JavaScript Date 是一个内置对象，内部包含从 1970 年 1 月 1 日开始的毫秒数，而外部则显示为格式优美的字符串，具体取决于环境。例如，语句:

```
**let d = new Date;**
```

创建一个包含当前日期和时间的 Date 对象。当我写这篇文章时，2020 年 6 月 21 日下午 2 点 27 分，相应的*日期*包含一个类似 1592828905046 的整数。我们可以用`*.valueOf*`显示这个内部值:

```
**new Date().valueOf()** is equal to **1592828905046**
```

相同的值(在相同的瞬间)可以通过以下方式获得:

```
**Date.now()**
```

如果我们用 *console.log()* 或 *document.write()* 检查一个日期对象，我们会得到一个人类可读的字符串:

```
**Mon Jun 22 2020 14:26:59 GMT+0200 (Central Europe daylight saving time)**  [Chrome, Firefox]or**Mon Jun 22 2020 14:26:59 GMT+0200 (CEST)**   [Safari]
```

# 用 date 对象方法设置日期格式

过去 30 年里发明的大多数编程语言都提供了适当的功能来按照你需要的方式 ***格式化*** 一个日期，例如在网页上显示。 **JavaScript 没有**。

例如，假设我们有一个*日期*对象 ***日期*** ，我们需要以如下方式将它显示为文本:

```
1) **June 22nd, 2020**2) **22/06/2020**3) **22.06.20 13.00**4) **Tue,** **Jun 22, 1.00 pm**
```

仅使用 ES5 JavaScript(既不使用任何库，也不使用 ***Intl*** 对象，参见下一节)，情况很糟糕:

1.  ***“2020 年 6 月 22 日”——***不可行:没有办法自己从 JavaScript 中获取完整的月份名称(没有 **Intl** )。
2.  **“22/06/2020”——**我能找到的最短的解决方案是:

```
let d = mydate.toISOString(); // returns 2020–06–20T13:00:00.000Z
d = d.substr(0,10).split('-').reverse().join('/');
```

3.**“22 . 06 . 20 13.00”——**一个(恐怖的)解决方案是:

```
let d = mydate.toISOString();  
d = d.substr(8,2)+’.’+d.substr(5,2)+’.’+d.substr(2,2)+' '+d.substr(11,5).replace(':','.') ;
```

4. **" *，*******6 月 22 日下午 1 点"***——太复杂而不值得引用:我们可以从我们得到的东西上切下各种碎片。 *toString()* 或 *toLocaleString()* 或*。toUTCString()* 并将它们重新组合在一起。*

# *用整数格式化日期。DateTimeFormat 对象*

*意识到这些缺点并寻找日期、数字等的区域表示问题的更广泛的答案。，ECMA International 在 2015 年推出了一个特定的国际化 API，其中包括这些问题的解决方案，以及许多其他功能。*

***不幸的是**，找到的解决方案远不是一个简单的解决方案。首先，ECMA International 没有选择用强大的新方法来扩充 Date 对象(这是一个愚蠢的想法吗？)，但他们发明了一种新的 ***Intl。*datetime format**对象，从而生成一个 ***。格式化*可用于格式化日期的**方法。其结果容易受到批评。*

*让我们看看我的意思。*

```
*1) **June 22nd, 2020***
```

*我们仍然不能直接得到我们想要的，因为。DateTimeFormat 没有提供生成序数后缀的选项。我们能得到的最佳近似值是这样的:*

```
*new Intl.DateTimeFormat(‘en-US’,{ year: ‘numeric’, month: ‘long’, day: ‘numeric’ }).format(mydate)*
```

*这就给出了 2020 年 6 月 22 日。至少可以说，这不是一个非常紧凑的解决方案。*

*顺便说一下， *Intl API* **提供了一种在 *Intl 中产生序数后缀的方法。PluralRules* 对象，但是没有办法在 *Intl 中获得相同的效果。DateTimeFormat.format* 方法。***

*第二个例子可以用类似的方式**精确地**解决:*

```
*2) **22/06/2020**new Intl.DateTimeFormat('it-IT',{ day: '2-digit', month: '2-digit', year: 'numeric' }).format(mydate)*
```

*对于第三种情况，没有简洁的解决方案，因为我们需要一个非标准分隔符，并且我们需要删除日期后的逗号:*

```
*3) **22.06.20 13.00**let d = new Intl.DateTimeFormat('en-GB',{ day: '2-digit', month: '2-digit', year: '2-digit', hour:'2-digit', minute:'2-digit', hour12:true }).format(mydate);d = d.replace(/\//g,'.').replace(/:/,'.').replace(',','')*
```

*我们的第四个例子可以用 Intl 正确地实现。日期时间格式:*

```
*4) **Mon,** **Jun 22, 1.00 pm**new Intl.DateTimeFormat('en-US',{ weekday:'short', month: 'short', day: 'numeric', hour: 'numeric', minute:'2-digit', hour12:true }).format(mydate)*
```

*然而，看着上面的表情，你不感到绝望吗？是不是显得没必要的啰嗦？*

# *解析日期*

*让我们继续程序员处理的第二个最常见的日期操作:解析表示日期的字符串，以获得相应的日期对象。*

*的确，日期“类”有一个“.parse”方法应该做到这一点:*

```
***Date.parse(‘2012–07–06’)** gives **1341532800000**and**Date.parse(‘July 6, 2012’)** gives **1341525600000***
```

****等等，等等*** ！但是这些日期不是一样的吗？我们得到不同的数字，怎么回事？让我们把它们格式化回来:*

```
***new Date(Date.parse(‘2012-07-06’)) : Fri Jul 06 2012 02:00:00 GMT:+0200****new Date(Date.parse(‘Jul 6, 2012’)) : Fri Jul 06 2012 00:00:00 GMT:+0200***
```

*你看出区别了吗？前者的 ***小时*** 是下午 2 点，后者的 ***午夜*** 。JavaScript 引擎解释两个日期，这两个日期对一个人来说是相等的，就好像它们是不同的一样:第一个日期被解释为 GMT 时区的午夜，然后转换为我的时区(CEST)，也就是凌晨 2:00。第二个日期被解释为我的时区的午夜。*

*我在 Chrome、Firefox 和 Safari 上测试过它们:它们都同意这种解释，这意味着它们可能遵循了 ECMA 的官方定义。
***那不是废话*** 吗？*

*更糟糕的是，各种 JavaScript 引擎在如何解释字符串的问题上并不总是意见一致。例如:*

```
 ***Date.parse(‘2012-07-06 14:20:34’)** 
                        *gives*          **1341584434000** *in Chrome and Firefox*
                    **NaN** *in Safari**
```

*Safari 是一个纯粹主义者，只识别日期和时间之间带有“T”分隔符的日期时间，例如**2012–07–06t 14:20:34。***

*除此之外， ***Date.parse*** 方法只能解释非常少的字符串类型: ***ISO*** 格式(我们的第一个例子)、 ***长日期*** (第二个例子)和 ***短日期*** (例如 07/06/2020)。没有办法通知。关于日期的组成部分如何放置在字符串中的解析函数。*

# *日期算法*

*在处理日期时，一个常见的需要是计算两个日期之间的时间间隔或者计算什么日期是*小时、天、月等。 ***后*** 或 ***前*** 给定日期。**

**对于原生 JavaScript 来说，这很简单，对与日期相对应的数值进行算术运算，即表示自 1970 年 1 月 1 日以来的*毫秒数的整数。***

***举个例子，***

1.  ***计算日期 2 和日期 1 之间的差异 ***小时*** :***

```
**Math.round( ( date2.valueOf() — date1.valueOf() ) / (60*60*1000) );**
```

**2.计算两周后的日期:**

```
**new Date(Date.now() + 2*7*24*3600*1000);**
```

**3.以天数计算我的年龄:**

```
**Math.floor((Date.now()-Date.parse(‘1956–07–13’)) / (24*3600*1000));**
```

**这并不困难，但是值得注意的是缺乏任何直接的和用户友好的方法来做同样的事情。**

**出现的问题通常会更复杂:**

**4.找到下一个星期一**

```
**let now = Date.now(); 
let today = now — now%(24*3600*1000);   // round to current day 
let weekDay = new Date(today).getDay(); // Sunday=0, Monday=1 etc. 
let nextMonday = new Date( today + (1 + (7-weekDay)%7)*24*3600*1000 );**
```

# **图书馆尽职尽责**

**幸运的是，有一些 JavaScript 库可以解决 JavaScript 对日期操作支持不足所带来的所有问题。而且我要说，任何一个像样的 JavaScript 程序员都必须使用其中的一个，而不是依赖于原生的 JavaScript ***Date*** 和 ***Intl*** 对象。**

**我将展示我们如何使用以下库之一出色地解决上面列出的所有示例:**

*   **[**moment.js**](https://momentjs.com/) ，最知名最全的库，支持多种浏览器。它包括对国际化的全面支持。**
*   **[**day.js**](https://day.js.org/en/) ，moment.js 的快速轻量替代方案(所有国际化支持都保存在单独的文件中)**
*   **[**luxon**](https://moment.github.io/luxon/) **，**最新的库，由 moment.js 相同的组织支持，使用了不同的方法，与 moment.js 相比有优点和缺点(参见[Luxon 为什么存在？](https://moment.github.io/luxon/docs/manual/why.html))**

# **用 moment.js 和朋友格式化日期**

**下面是我们给出的四个格式化示例的工作原理。**

****moment . js*和* day.js****

**假如 ***mydate*** 是用***【mydate】****或****dayjs(mydate)***获得的包装对象，我们的格式化问题就很容易解决了:****

```
**1) **June 22nd, 2020             mydate.format('MMMM Do, YYYY')**2) **22/06/2020                  mydate.format('DD/MM/YYYY')**3) **22.06.20 13.00              mydate.format('DD.MM.YY HH.mm')**4) **Tue,** **Jun 22, 1.00 pm        mydate.format('ddd, MMM D, h.mm a')****
```

**很简单，不是吗？格式选项的完整列表可在[这里](https://momentjs.com/docs/#/displaying/)和[这里](https://day.js.org/docs/en/display/format)获得。**

****卢克森****

**一旦用创建了日期时间对象**

```
**let mydate = luxon.DateTime.fromISO('2020-06-22')**
```

**我们的四个例子可以(几乎)这样解决:**

```
**1) **June 22nd, 2020             mydate.toFormat('MMMM d, y')** Note: no support for the n-th suffix2) **22/06/2020                  mydate.toFormat('dd/MM/y')**3) **22.06.20 13.00              mydate.toFormat('dd.MM.yy HH.mm')**4) **Tue,** **Jun 22, 1.00 pm        mydate.toFormat('EEE, MMM d, h.mm a')** Note: AM/PM is always uppercase**
```

**Luxon 中可用的格式选项不多的原因是它依赖于 ***Intl*** 对象，并且继承了它们的所有优点和缺点。的完整规格。 *toFormat* 选项可用[此处](https://moment.github.io/luxon/docs/manual/formatting.html#toformat)。**

# **解析与 moment.js 和朋友的约会**

****Moment.js 和 dayjs****

```
****moment('2012–07–06')
moment('2012–07–06', 'YYYY-MM-DD')
moment('Jul 6, 2020', 'MMM D, YYYY')****dayjs('2012–07–06')
dayjs('2012–07–06', 'YYYY-MM-DD')
dayjs('Jul 6, 2020', 'MMM D, YYYY')**all give**Fri Jul 06 2012 00:00:00 GMT+0200****
```

**这正是我们可以从*解析*函数中期望得到的东西。此处[此处](https://momentjs.com/docs/#/parsing/)和[此处](https://day.js.org/docs/en/parse/string-format)提供完整规格。**

**请注意，moment.js 和 day.js 都将缺少的时间部分解释为当前时区的午夜。如果想用 UTC 解析或显示日期时间，可以用`moment.utc()`和`dayjs.utc()`代替`moment()`和`dayjs()`。**

****卢克森****

```
****luxon.DateTime.fromISO('2012-07-06')
luxon.DateTime.fromFormat('2012-07-06','y-MM-dd');
luxon.DateTime.fromFormat('Jul 6, 2012','MMM d, y')**all give**2012-07-06T00:00:00.000+02:00****
```

**与 moment.js 和 day.js 一样，当没有给出附加选项时，日期被解释为当前时区的午夜。Luxon 解析的完整规范可从[这里](https://moment.github.io/luxon/docs/manual/parsing.html)获得。**

# **与 moment.js 和朋友的约会算法**

**下面是我们给出的四个计算日期的例子。**

****Moment.js 和 day.js****

```
**1\. compute the difference in hours between date2 and date1:                                        -> **moment(date2).diff(date1,’hours’)** -> **dayjs(date2).diff(date1,’hours’)**2\. compute the Date that is two weeks in the future since now:
-> **moment().add(2,'weeks')** -> **dayjs().add(2,'weeks')**3\. compute my age in days:
-> **moment().diff('1956-07-13','days')** -> **dayjs().diff('1956-07-13','days')**4\. find the next Monday
-> **moment().startOf('isoWeek').add(1, 'week')** -> **dayjs().startOf('isoWeek').add(1, 'week')****
```

**最后一个需要一些解释:*。startOf('isoWeek')* 获取给定周期的开始日期；在我们的示例中，我们需要符合 ISO 8601 标准的本周开始，即星期一。**

**全套操作方法在[这里](https://momentjs.com/docs/#/manipulating/)和[这里](https://day.js.org/docs/en/manipulate/manipulate)解释**

****卢克森****

```
**1\. compute the difference in hours between date2 and date1:                                        -> **date2.diff(date1).as('hours')**2\. compute the Date that is two weeks in the future since now:
-> **luxon.DateTime.local().plus({weeks: 2})**3\. compute my age in days:
-> **luxon.DateTime.local().diff( luxon.DateTime.fromISO('1956-07-13') ).as('days')**4\. find the next Monday
-> **luxon.DateTime.local().set({weekday: 1}).plus({weeks: 1})****
```

**完整规格可在[此处](https://moment.github.io/luxon/docs/manual/math.html)获得。**

# **结论**

**不清楚为什么 ECMA 从 2011 年起就没有着手 JavaScript 日期对象规范，尽管它的功能缺陷是显而易见的。幸运的是，一些第三方库可以让我们避免很多麻烦。**

**可能现在最好的选择是 *day.js* ，因为它比 *moment.js* 更轻，比 *luxon* 更紧凑。与 *moment.js 相比，*还有一个额外的优势，那就是它的 **dayjs** wrapper 对象是**不可变的**，也就是说，不能通过应用*之类的方法无意中改变它。增加*或*。减去*。**