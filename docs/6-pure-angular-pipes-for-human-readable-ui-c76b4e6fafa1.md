# 6 个纯角度管道，用于人类可读的 UI

> 原文：<https://javascript.plainenglish.io/6-pure-angular-pipes-for-human-readable-ui-c76b4e6fafa1?source=collection_archive---------2----------------------->

## 定制角形管道，改善用户界面，促进“干”原则，并解决日常任务。

![](img/991993cdb1aacac199d3ad8af4cc3482.png)

Angular Pipe — The StrawPipe

# 用于性能和用户界面的角形管道

角形管道是将小的转换应用到 UI 的有效方式。使用管道让你的应用更加**干巴巴**(不要重复自己)。

> 它允许您分离关注点，并将 UI 转换逻辑从组件逻辑中移出。

> 纯角度管道是您可以在角度应用程序中采用的许多性能优化策略之一。默认情况下，管道是纯的。

一个角形管道接收一个输入，并通过一个`transform`函数将该输入转换成所需的输出。`Pipe`类实现了`PipeTransform`接口，可以是**纯的或不纯的。**

## 纯的

如果管道是纯的，这意味着计算结果被缓存，并且`transform`函数仅在输入改变时运行。纯管道的行为类似于记忆化。

## 肮脏的

另一方面，如果管道**不是**纯的，这将意味着管道`transform`功能在每个变化检测周期期间运行。如果您的管道具有影响转换结果的内部状态，这将非常有用。

> 重要的是要注意，即使输入值没有变化，具有`pure: false`的管道也会在变化检测周期**运行。意识到这种行为导致的性能瓶颈非常重要。**

 [## 有角的

### Angular 是一个构建移动和桌面 web 应用程序的平台。加入数百万开发者的社区…

angular.io](https://angular.io/guide/pipes) 

让我们开始吧…

# 1) URL 友好字符串——将`string`转换成一个 slug

## ToSlugPipe

当实现由于用户输入、类别和其他人类可读数据而生成的动态 URL 时，您通常会希望通过一个函数来处理字符串，从而生成一个 URL 安全版本。

> **URL 不应该使用下划线、空格或任何其他字符来分隔单词。**

[](https://moz.com/learn/seo/url) [## URL 结构[2020 SEO 最佳实践]

### 尽可能保持 URL 的简单、相关、吸引人和准确是获得用户和搜索的关键…

moz.com](https://moz.com/learn/seo/url) 

> 最好这个方法是 **DRY，**以便 URL 遵循相同的约定。这是弯管的完美用例。

管道将一个 URL 转换成一个 slug。它去除了 URL 中不允许或不相关的任何字符，包括在用作 URL 路径/段时可能会导致问题的字符串。`toSlug`使用正则表达式(Regex)去除不符合 URL 的字符。

**`**toSlug**`**做什么简单来说:****

*   **转换为**小写****
*   **用`-`替换**空格****
*   **替换**特殊**字符**
*   **将 **&** 替换为`and`**
*   **删除所有**非单词**字符**
*   **用单个`—`替换多个`**--**`**
*   **从**开始**修剪`—`**
*   **从**端**修剪`—`**

**ToSlugPipe — Transform string to URL slug**

## ****奖励—让您的网址独一无二****

**可选的`id`参数将**将传递的`id`值的** **一半**附加到废料的末尾。**

**如果你有机会通过这种转换生成相似或重复的碎片，这是很有用的**。它确保了 slugs 是唯一的，从而使你的应用程序的 URL 是唯一的。****

> **可以理解的是，通过提高可发现性和相关性，这对 SEO 有很大的影响。**

****在一个中等 URL 中可以看到一个示例输出:**[https://it next . io/smoother-UX-on-page-load-with-angular-resolvers-e 063 ef 035124](https://itnext.io/smoother-ux-on-page-load-with-angular-resolvers-e063ef035124)**

# **2)显示与其值(数字)相关联的枚举键(字符串)**

> **对于检索要在 UI 中显示的类别名称非常有用。**

## **CategoryAsStringPipe**

**`CategoryAsString`管道获取 TypeScript **enum** `Category`并检索与 enum 值相关联的键。**

**如果你已经在你的应用中为类别使用了一个**枚举**，这是很有用的。这个管道允许您将枚举值作为一个`number`(类别类型)传递给它，它返回与它关联的**标签**，标签的类型为`string`。**

> **这节省了大量重复的代码，因为没有这个管道，您必须将`Category`导入到在 HTML 模板中显示`Category`名称的每个组件中**

**CategoryAsStringPipe — Get enum key from the value**

**例如，`Category`枚举可能看起来像…**

```
enum Category { All = 0
    Fitness // 1
    Music // 2
    Entertainment // 3
    ... }
```

**有了这个枚举，如果你通过了类别`Music`比如…**

```
{{ Category.Music | categoryAsString }}
```

**或者更真实地来自真实数据…**

```
{{item.category | categoryAsString}}
```

**它将返回**字符串** `Music`**

## **奖励—在 URL 中使用类别关键字**

**使类别 URL 符合`toSlug`管道。**

**在这里，我们使用`categoryAsString`管道将`number`类型的类别转换为`string`键，然后使用`toSlug`将结果转换为 slug。**

**这将产生`sports-fitness`而不是`Sports and Fitness`。更加 URL 友好。**

**Category To Slug — URL compliant categories**

# **3)用省略号截断溢出的文本**

## **椭圆形**

**有时，您会有溢出的文本，您正在努力防止溢出。也许你想严格控制从一个`string`显示的字符数。或者你可能只想展示一定数量的角色作为**预告片。****

**无论您的具体使用情况如何，`ellipsis`管道会将`string`修剪成不超过预定义最大长度的字符串。最大长度作为参数传递给转换函数。**

**当接收到比最大长度更长的字符串时，它会在末尾追加一个`...`。如果不是，那么它返回未修改的`string`。**

**EllipsisPipe — Truncate Text With Ellipsis**

**要使用这个，你**提供一个数字给`ellipsis`管道的最大参数**。生成的用户简介不会超过 50 个字符+ `...`。**

> **如果该值不长于最大值，则不会发生省略号。**

**Ellipsis Use Case — HTML**

## ****奖励——提供一个动态参数来代替省略号****

**这允许你添加一个**定制的动态替换**到削减的`string`。**

**原来`ellipsis`管道的区别是**可选** `**append**` **自变量。**参数`append`有一个默认值，即省略符号`...`。**

**让我们给它起一个更合适的名字: **TruncatePipe****

**TruncatePipe — Truncate Text**

**要使用这个，你需要**提供你自己的** `**string**` **来附加你想要的，而不是** `**...**` **作为右边的第二个参数。****

**Truncate Use Case — HTML**

**这使得我们省略管道更加“干燥”(不要重复)，因为所有的截断都可以由一个管道来处理。**

# **4)将一个数(1)转换成它的等效值(1st)**

## **普通吸管**

**当基于数字数据创建动态**摘要**、**描述**和**标签**时，您必须将数字转换为人类可读的文本。**

**这通常需要复杂的代码来将统计数据转换成更可读的形式，**一种更少机器人化、更人性化的形式**。**

****根据数据和图表自动生成报告**是常见的做法，需要这种格式。**

**此外，**自动化交易电子邮件**需要将数据转换成人类可读的形式。**

****例如:****

> **“你刚刚雇佣了第 32 名员工”**

**听起来比...**

> **“你刚刚雇佣了 32 号员工”或“你刚刚雇佣了 32 号员工”**

**OrdinalSuffix — Convert Number to Ordinal**

**使用`ordinalSuffix` …**

**OrdinalSuffix Use Case — HTML**

# **5)截止时间—(天、月、小时、SECS)直到某个日期**

## **时间管道**

****依赖关系:****

*   **日期-fns**

**[](https://date-fns.org/) [## 现代 JavaScript 日期实用程序库

### date-fns 提供了最全面、最简单、最一致的工具集，用于在

date-fns.org](https://date-fns.org/) 

这个管道接受两个日期(**一个是今天的日期**)，并以人类可读的形式返回它们之间的差异。

您需要从 **date-fns、**导入`formatDistanceToNow`，这将是完成大部分格式化工作的函数。

它采用了一个`**options**`参数，我们可以在这里指定是否要在返回值中包含秒。

> 可用的选项有`includeSeconds`、`locale`和`addSuffix`。

> 为了使用这个管道，您只需要提供未来的日期…date——DNS 处理开始日期(现在是日期时间)

TimeUntilPipe — Get time until a future date

如果您有需要显示的功能，这个管道特别有用:

*   离**事件开始还有多长时间**
*   离你**开始工作还有多长时间**

> 它本质上是一个**倒计时。**

你可以像这样使用它…

TimeUntil — Use Case

# 6)花费的时间/差异—两个日期之间的差异

## 数据差异管道

与前面的管道`timeUntil`类似，这个管道**将两个日期作为输入，并以人类可读的格式返回两者之间的差异**。

> 如果您想要显示开始日期和结束日期之间的时间，这个管道非常有用。

示例输出:`23 hrs 59 mins 30 secs`

诸如**行程时间或两个目的地之间的行程时间**的功能。

> 我们将使用另一个 **date-fns 函数**来检索人类可读的(天、小时、分钟、SECS)差异。

**这根管子有两个部分:**

*   将`fromDate`和`toDate`作为参数传递给`transform()`函数。
*   然后，`formatDistance()`函数返回两者之间的差， **PLUS 将差格式化为人类可读的** `**string**` **。**

DateDiffPipe — Time Difference in hours, minutes, seconds

使用它就像将`fromDate`作为第二个参数**(右边的参数)传递一样简单。**

DateDiff Use Case — HTML

## 奖金-获得严格模式的差异

使用与`dateDiff`相同的逻辑，您可以在我们的`transform()`函数中从 date-fns 调用`formatDistanceStrict()`函数。

**这将改变返回的差异字符串中使用的语言**。`formatDistance`和`formatDistanceStrict`函数的区别在于，后面的**函数不会使用‘大约 5 个月’之类的近似语言。**

DateDiff Strict

你也可以强制一个单位，如**选项**参数中的`’second’ | ‘minute’ | ‘hour’ | ‘day’ | ‘month’ | ‘year’`。

> 注:**还有一个严格版的** `***formatDistanceToNow***` **函数**叫做`*formatDistanceToNowStrict*`。因此您可以在我们的`*timeUntil*`管道中实现严格模式。

要使用它，您只需为`strict`参数提供一个`true`值来启用严格模式。

## 就这样……我们现在有了 6 个更有用的管道，让我们的开发生活变得更简单了。

> 感谢阅读！
> 
> 有什么问题吗？请在评论中告诉我。

[](https://www.npmjs.com/package/date-fns) [## 日期-fns

### 现代 JavaScript 日期实用程序库

www.npmjs.com](https://www.npmjs.com/package/date-fns) 

# **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！******