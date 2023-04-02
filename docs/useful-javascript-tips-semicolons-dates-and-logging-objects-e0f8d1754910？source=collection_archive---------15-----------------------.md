# 有用的 JavaScript 技巧—分号、日期和日志对象

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-semicolons-dates-and-logging-objects-e0f8d1754910?source=collection_archive---------15----------------------->

![](img/4290cfb6e89f173527241364dc8c2dcb.png)

Photo by [Kosuke Noma](https://unsplash.com/@kkk7799?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 分号

JavaScript 中可以自动插入分号。

它在中断当前行的代码后的下一行插入分号。

另外，当下一行以`}`开头时，会插入一个分号。

当到达源代码文件的末尾时，将添加分号。

分号也插在`break`关键字之后。

它也被添加在`throw`和`continue`关键词之后。

如果我们自己不添加，那么解释器会为我们插入。

# 使用 Moment.js 轻松操作日期

为了使日期操作更容易，我们可以使用 moment.js

我们可以通过添加脚本标签或安装包来安装它。

我们可以写:

```
<script src="https://unpkg.com/moment" />
```

或者:

```
npm install moment
```

如果我们使用这个包，我们可以写:

```
import moment from 'moment'
```

要获得当前日期和时间，我们可以写:

```
const date = moment();
```

要解析日期，我们可以写:

```
const date = moment(string);
```

它采用不同的格式代码。它们是:

*   `YYYY` — 4 位数年份
*   `YY` —两位数年份
*   `M` —不含 0 的 2 位数月份数字
*   `MM` — 2 位数字的月份号
*   `MMM` — 3 个字母的月份名称
*   `MMMM` —完整的月份名称
*   `dddd` —全天名称
*   `gggg` — 4 位数周年
*   `gg` — 2 位数周年
*   `w` —一年中没有前导零的一周
*   `ww` —带有前导零的一年中的某周
*   `e` —从 0 开始的一周中的某一天
*   `D` —无前导 0 的 2 位数日数
*   `DD` —两位数的日数
*   `Do` —带序数的天数
*   `T` —时间部分的开始
*   `HH`—0 到 23 的两位数小时
*   `H` —从 0 到 23 的两位数小时，不带前导 0
*   `kk` —从 1 到 24 的两位数小时
*   `k` —从 1 到 24 的两位数小时，不带前导 0
*   `a/A` —上午或下午
*   `hh` —两位数小时(12 小时制)
*   `mm` —两位数分钟
*   `ss` — 2 位数秒
*   `s` — 2 位数秒，不带前导 0
*   `S` — 1 位数毫秒
*   `SS` — 2 位毫秒
*   `SSS` — 3 位数毫秒
*   `Z` —时区
*   `x` —以毫秒为单位的 UNIX 时间戳

例如，我们可以写下日期格式:

```
moment().format("YYYY-MM-DD")
```

Moment.js 附带了几个常量。

它们是:

*   `moment.HTML5_FMT.DATETIME_LOCAL`—— `‘YYYY-MM-DDTHH:mm’`
*   `moment.HTML5_FMT.DATETIME_LOCAL_SECONDS` — `‘YYYY-MM-DDTHH:mm:ss’`
*   `moment.HTML5_FMT.DATETIME_LOCAL_MS`—— `‘YYYY-MM-DDTHH:mm:ss.SSS’`
*   `moment.HTML5_FMT.DATE`—— `‘YYYY-MM-DD’`
*   `moment.HTML5_FMT.TIME` — `‘HH:mm’`
*   `moment.HTML5_FMT.TIME_SECONDS` — `‘HH:mm:ss’`
*   `moment.HTML5_FMT.TIME_MS`—— `‘HH:mm:ss.SSS’`
*   `moment.HTML5_FMT.WEEK` — `‘YYYY-[W]WW’`
*   `moment.HTML5_FMT.MONTH`——`‘YYYY-MM’`

我们可以用`isValid`方法验证一个日期。

例如，我们可以写:

```
moment('2020-13-32').isValid()
```

返回`false`，或者:

```
moment('2020-01-23').isValid()
```

返回`true`。

# 获取相对时间字符串

我们可以使用`fromNow`方法从当前日期和时间获得相对值。

例如，我们可以写:

```
moment("2020-11-23").fromNow()
```

并得到`‘in 6 months’`。

我们可以将`true`传递到`fromNow`，那么它就显示了差异，而不涉及未来或过去。

所以如果我们写:

```
moment("2020-11-23").fromNow(true)
```

然后我们得到`‘6 months’`。

# 操纵日期

我们可以使用 Moment.js `add`和`subtract`方法来增加或减少日期的数量。

例如，我们可以写:

```
moment('2020-11-23').add(1, 'days');
moment('2020-11-23').subtract(1, 'days');
```

第二个参数可以有以下值:

*   `years`
*   `quarters`
*   `months`
*   `weeks`
*   `days`
*   `hours`
*   `minutes`
*   `seconds`
*   `milliseconds`

# 检查 JavaScript 对象

JavaScript 中有多种检查对象的方法。

我们可以使用`console.log`方法记录一个表达式并获取它的返回值。

例如，我们可以写:

```
console.log(dog)
```

还有一个`console.dir`方法可以让我们检查任何对象。

例如，我们可以写:

```
console.dir(dog)
```

`JSON.stringify`是让我们考察一个物体的另一种方式。

它将返回一个字符串。

例如，假设我们有:

```
const dog = {
  color: 'white',
  breed: 'poodle'
}
```

我们可以写:

```
console.log(JSON.stringify(dog))
```

这将创建一个未格式化的字符串。

然后我们得到:

```
{"color":"white","breed":"poodle"}
```

记录在案。

为了更好地打印字符串，我们可以再传入两个参数。

我们写道:

```
console.log(JSON.stringify(dog, null, 2))
```

我们得到了:

```
{
  "color": "white",
  "breed": "poodle"
}
```

![](img/d58ed2cbed6eae8ff6d08ce9d4ed573e.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Moment.js 使得日期操作比使用`Date`构造函数更容易。

我们可以用`console`和`JSON.stringify`来检查表达式值。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**