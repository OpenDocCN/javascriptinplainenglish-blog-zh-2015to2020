# 用 JavaScript 显示可读日期的基础知识

> 原文：<https://javascript.plainenglish.io/basics-of-showing-readable-dates-with-javascript-92235ee6ac89?source=collection_archive---------15----------------------->

![](img/a6182cdd40115ea267d1df36b0bb6441.png)

Photo by [Harrison Broadbent](https://unsplash.com/@harrisonbroadbent?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 转换一个 ISO 日期很容易，但是人类的心理因素会影响你显示日期的方式

在 [Epiloge](http://www.epiloge.com) 我们使用 MongoDB 作为我们的通用数据库。在大多数文档中，我们至少有一个包含日期的字段。它通常是一个`created`字段或一个`updated`字段。有时候我们两者都需要。

这些日期采用 ISO 格式，例如*2020–04–08t 16:00:00 z*，即 2020 年 4 月 8 日下午 4 点。点击阅读更多关于[ISO 日期格式的信息。](https://www.cryptosys.net/pki/manpki/pki_iso8601datetime.html)

到目前为止一切顺利。在大多数情况下，你想用你的日期来分类你的文件。或者您只想确保知道文档最后一次更新是什么时候——例如，更新您的 sitemap.xml。

您不必向用户显示您在数据库中使用的日期。但是，如果您确实想向用户显示 ISO 日期，那么 helper 方法中的最佳策略是什么呢？在本文中，我提供了一种无需插件就能工作的普通 JavaScript 方法。

# 为您的网站创建一个全球可访问的助手方法来转换您的 ISO 日期

可读的 ISO 日期不一定需要使用插件。插件经常会耗尽你部署的应用程序的空间。

只需几行 JavaScript，你就可以编写一个助手函数来转换你的 ISO 日期，并让你根据你想在应用程序中显示日期的位置来微调它。这种灵活性的第二部分可能是编写自己的 Javascript helper 方法来将 ISO 日期转换为可读文本的更重要的原因。

```
$_HelperFunctions_setOlderDate: function(oldDate) {

   var tempDate = new Date(oldDate);
   var oldTime = Math.round(tempDate.getTime() / 1000);
   var shownDate = ''; var months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

   var year = tempDate.getFullYear(); 
   var month   = tempDate.getMonth();
   var day     = tempDate.getDate();                 
   shownDate = months[month] + ' ' + day + ', ' + year;    

   return showDate;
}
```

这个 helper 方法接受一个 ISO 日期，并使用`new Date(oldDate)`创建一个 date 类型的新对象。

然后，您可以使用缩写的月份数组(一月、二月等。)或者如果您愿意，也可以使用月份的全名，并通过使用函数`getFullYear()`、`getMonth()`和`getDate()`组合成一个可读的字符串。

注意，getMonth()返回一个从 0 到 11 的整数，可以用来从数组中选择月份。

![](img/b5635e1eb931d5c776a891ee455776b7.png)

Screenshot from [Epiloge](http://www.epiloge.com) showing dates in a list — modified based on how old the date is

# 修改最近日期的助手方法

有些情况下，向用户显示发布或更新内容的日期，如*2020 年 4 月 8 日*并不是首选方式。

在 Epiloge 的情况下，我们有文章和项目，但也有评论，可以在几分钟或几小时前发布。在这种情况下，你可能想切换到一种读者格式，说明帖子或评论是多久前发表的…或者如果你不想这样做，只需写“今天”而不是当前日期。

```
$_HelperFunctions_setOlderDate: function(oldDate) {

   var d = new Date(); 
   var currentTime = Math.round(d.getTime() / 1000);   
   var tempDate = new Date(oldDate);
   var oldTime = Math.round(tempDate.getTime() / 1000);
   var showTimeAgo = false; var shownDate = ''; var months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
   if ((currentTime - oldTime) < (86400*7)) showTimeAgo = true;

   var year    = tempDate.getFullYear(); 
   var month   = tempDate.getMonth();
   var day     = tempDate.getDate();                 
   shownDate = months[month] + ' ' + day + ', ' + year;    

   return [showTimeAgo, shownDate];
}
```

这个修改后的 helper 方法查看日期是否不超过 7 天。如果是这样，它返回一个变量`showTimeAgo`,向我们表明我们不想显示 2020 年 4 月 8 日这样的日期，而是显示一串 5 天前的*。*

*在前端使用 Vue.js 的 Epiloge，我们使用一个名为 [VueTimeAgo](https://www.npmjs.com/package/vue-timeago) 的便捷插件来显示不超过 7 天的日期。*

*您可以通过安装 VueTimeAgo 并将其添加到您的 Vue 应用程序中的 main.js 来添加它。如果你想支持英语以外的语言，使用这样的插件是非常明智的。*

```
*//in console, install vue-timeago
npm i vue-timeago//in your main.js, add VueTimeago
import VueTimeago from 'vue-timeago'
Vue.use(VueTimeago, {  
 locale: undefined, // Default locale  
});*
```

*然而，如果你不想使用任何插件(或者找不到适合你的前端框架的插件)或者只是想保持你的灵活性，你可以写你自己的 JavaScript 代码。*

*用户不一定需要比 a .最近一小时发布的，B 小时前发布的，c .一天前发布的或 D 天前发布的更多的信息。*

*您可以修改您的帮助器方法，使其包含以下选项:*

```
*$_HelperFunctions_setOlderDate: function(oldDate) {
   ...            
   shownDate = months[month] + ' ' + day + ', ' + year;  
   var difference = currentTime - oldTime;
   var days = 0; var hours = 0;
   if (difference < (86400*7)) {
      days = Math.trunc(difference/86400);
      if (days > 1) shownDate = days + ' days ago';
      if (days == 1) shownDate = 'one day ago';
      if (days == 0) {
         hours = Math.trunc(difference/3600);
         if (hours > 1) shownDate = hours + ' hours ago';
         if (hours == 0) shownDate = 'less than an hour ago';
      }
   }           
   return shownDate;
}*
```

*`Math.trunc`是一个 Javascript 方法，它截断除法的余数。如果你用 5 除以 3，这个方法会得到 1 的结果，2 的余数被切掉。*

*该方法覆盖了默认的`shownDate`变量，我们已经将它设置为一个日期，比如 2020 年 4 月 8 日，取而代之的是返回一个输出，比如 5 天前的*。**

# **还有一个提示，如果你在你的网页上使用了一年的版权免责声明**

**很多网站都有类似 *Copyright 2020 Epilogue 这样的内容。保留所有权利。在他们的网站上。*在页脚或其他地方。**

**如果您还没有这样做，请使用一个简单的助手方法来自动更新当前年份，如下所示。**

```
**$_HelperFunctions_getCurrentYear: function() {
    var date = new Date();
    var currentYear = date.getFullYear();
    return currentYear;
}**
```

## ****用简单英语写的 JavaScript 笔记****

**我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。**

**我们还推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****