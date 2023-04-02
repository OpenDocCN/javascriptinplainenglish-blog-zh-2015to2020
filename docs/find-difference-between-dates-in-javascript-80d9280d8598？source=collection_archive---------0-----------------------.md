# 在 JavaScript 中找出日期之间的差异

> 原文：<https://javascript.plainenglish.io/find-difference-between-dates-in-javascript-80d9280d8598?source=collection_archive---------0----------------------->

## 了解如何在 JavaScript 中找出两个日期之间的差异。

![](img/d770a5b96ff60453c45c9920c3c9c6ac.png)

Image from [Lukas Blazek](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们学习在两个日期之间寻找`Number of Seconds`、`Number of Minutes`、`Number of hours`、`Number of days`、`Number of weeks`、`Number of months`、`Number of years`。

让我们创建两个日期

```
let d1 = **Date.now();**let d2 = **new Date(2019,5,22).getTime()**; //Jun 22 2019 in millisecondconsole.log(d1); //1573527098946console.log(d2); //1561141800000
```

# **两个日期之间的秒数。**

```
**1 second = 1000 milliseconds**
```

现在我们有两个毫秒级的日期`d1`和`d2`。要将`milliseconds`转换成`seconds`，我们可以将`milliseconds` 中两个日期的差值除以`1000`。

```
function secondsDiff(d1, d2) { let millisecondDiff = d2 - d1; let secDiff = Math.floor( **( d2 - d1) / 1000** ); return secDiff;}
```

# 两个日期之间的分钟数。

```
**1 minutes = 60 seconds**
```

现在我们有方法找到两个日期之间的`seconds`的数目，所以我们可以找到`seconds`的差异，然后除以`60`就会得到两个日期之间的`minutes`的数目。

```
function minutesDiff(d1, d2) { let seconds = secondsDiff(d1, d2); let minutesDiff = Math.floor( **seconds / 60** ); return minutesDiff;}
```

# 两次约会之间的小时数。

```
**1 hour = 60 minutes**
```

现在我们有方法找到两个日期之间的`minutes`的数目，所以我们可以找到`minutes` 的差异，然后除以`60`就会得到两个日期之间的`hours`的数目。

```
function hoursDiff(d1, d2) { let minutes = minutesDiff(d1, d2); let hoursDiff = Math.floor( **minutes** **/ 60** ); return hoursDiff;}
```

# 两个日期之间的天数。

```
**1 day = 24 hours** 
```

现在我们有方法找到两个日期之间的`hours` 的数目，所以我们可以找到差异，然后除以`24` 就会得到两个日期之间的`number of days` 。

```
function daysDiff(d1, d2) { let hours = hoursDiff(d1, d2); let daysDiff = Math.floor( **hours / 24** ); return daysDiff;}
```

# 两个日期之间的周数。

```
**1 week = 7 days**
```

现在我们有方法找到两个日期之间的数字`days` ，所以我们可以找到差异，然后除以`7`将得到两个日期之间的`number of weeks` 。

```
function weeksDiff(d1, d2) { let days = daysDiff(d1, d2); let weeksDiff = Math.floor( **days/ 7** ); return weeksDiff;}
```

# 两个日期之间的年数。

为了找出两个日期之间的年数，我们有内置方法`getFullYear`，用`date1`年减去`date2`年，我们将得到`yearsDiff`。

```
function yearsDiff(d1, d2) { let date1 = new Date(d1); let date2 = new Date(d2); let yearsDiff =  **date2.getFullYear() - date1.getFullYear()**; return yearsDiff;}
```

# 两个日期之间的月数。

```
**1 month != 30 days** Number of days in month is not same in all months , so we need to do it differently
```

步骤:

*   首先，我们需要找出两个日期之间的年数。
*   将两个日期之间的年数乘以 12(因为每年有 12 个月)
*   `second date`的月份号(6 月→ 5 日)与`first date`的月份号

查找两个日期之间的月数

```
function monthsDiff(d1, d2) { let date1 = new Date(d1); let date2 = new Date(d2); let years = yearsDiff(d1, d2); let months =**(years * 12)** + (**date2.getMonth() - date1.getMonth()**) ; return months;}
```

感谢阅读📖。希望你喜欢这个。如果你发现任何错别字或错误给我一个私人说明📝谢谢🙏 😊。

关注我 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

**请捐款** [**此处**](https://www.paypal.com/paypalme2/jagathishSaravanan) **。你捐款的 80%捐给了需要食物的人🥘。提前感谢。**