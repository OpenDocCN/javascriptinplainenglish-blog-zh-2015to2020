# Node.js 中的正则表达式备忘单

> 原文：<https://javascript.plainenglish.io/learn-to-use-regular-expressions-like-a-ninja-in-node-js-20cfb6806f26?source=collection_archive---------0----------------------->

## 轻松学习、编写和执行正则表达式的详细故事

![](img/73dfc668c840595fa99fc8361dcd8cba.png)

Photo by [Gábor Molnár](https://unsplash.com/@gabormolnar92?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.介绍

由于正则表达式的 crypt 语法，它是一个困扰和困扰许多开发人员的话题。

正则表达式是包含搜索模式的字符串(例如**\[0–9]{ 2 } \**这个模式接受一个 2 位数，我们用它在文本中找到这些模式。

# 2.如何创建正则表达式

在 javascript 中，我们可以用两种方法创建正则表达式:

```
// a string with a format /<RegExp>/;
**var twoDigitRegExp = /[0–9]{2}/;**// Construct a new RegExp object
**var twoDigitRegExp = new RegExp(‘[0–9]{2}’);**
```

# 3.正则表达式构建块

## 量词

```
**p*** --> The ***** will match the previous **p** char for 0 or more times. 
Example **/js*/** will accept j (s char has **zero** times) , js (s char has **one** time) , jss (s char has **two** times).**p+** --> The **+** will match the previous **p** char for 1 or more times. 
Example **/js+/** will not  accept j (s char has **zero** times) but will accept js (s char has **one** time) , jss (s char has **two** times).**p?** --> The **?** will match the previous  **p** char for 0 or one time. 
Example **/js?/** will accept j (s char has **zero** times), js (s char has **one** time) but not accept jss (s char has **two** times).**p{n}** --> The **{n}** will match the previous  **p** char for n times. 
Example **/js{1}/** will accept js (s char has **one** time) but not accept jss (s char has **two** times).**p{n,}** --> The **{n}** will match the previous  **p** char for at least n times.**p{x,y}** --> The **{x,y}** will match the previous  **p** char from x to y times.
```

## 字符类别

```
**/.p/** -->The **.** will match every charatecter before **p.** Example **/.p/** will accept **keep , deep** but not accept **js.****/\d/** -->The **\d** will match every number from [0-9]. Example **/festival\d/** will accept festival2020 , festival 2019 but not festivalBeer.**/\D/** -->The **\D** will match every number is **not** from [0-9]. Example **/festival\D/** will accept festivalBeer but not festival2020.**/\w/** -->The **\w** will match every character **in** latin alphabet include underscore. Example **/festival\w/** will accept festival2020 , festival 2019 but not festival$$.**/\W/** -->The **\W** will match every character **not** **in** latin alphabet include underscore. Example **/festival\W/** will accept festival#$ but not accept festival2019.**/\s/** -->The **\s** will match a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Example **/festival \s2020/** will match ' 2020' at 'festival 2020'.**/\S/** -->The **\S** will matche a single character other than white space. Example **/\S\w*/** will match 'festival' at 'festival 2020'.
```

## 组和范围

```
**/gin|wine/** -->The **|** is the or operator**.** Example **/gin|wine/** will match 'gin' or 'wine' at 'Gin is my favorite drink'.**/[A-Z]/** -->The **[]** indicates a characters setto match every char in the set. Examples of character sets [A-Z] matches any char from A-Z, [@#] matches only @ and # and so on**.** Example **/[@#]\w*/g** will match '@wine' and '#scotch' at '@wine $gin #scotch'.**/[^A-Z]/** -->The **^** indicates the **not in operator** a characters set.**/(@|#)/ --> The ()** indicates capturing group. Example **/(@|#)(gin|wine)/g ** at '**@wine @gin scotch'** will match '@wine' and '@gin'
```

## 正则表达式标志

```
**/(@|#)(gin|wine)/g** --> Global search with **g flag** the regExp **/(@|#)(gin|wine)/g** will check every word that matches. Example **/(@|#)(gin|wine)/g** at '@wine @gin scotch' will match '@wine' and '@gin' but without **g flag** will match only first word'@wine'**/(@|#)(gin|wine)/i** -->Case sensitive search with **i flag**. Example **/(@|#)(gin|wine)/i** at '@WINE @gin scotch' will match '@WINE'.**/(@|#)(gin|wine)/gi -->** Combine **g and i flags** will check all words and with case sensitive. Example **/(@|#)(gin|wine)/i** at '@WINE @gin scotch' will match '@WINE' and '@gin'.
```

## 正则表达式断言

```
**@(?=gin)** --> **Lookahead assertion:** Matches '@' only if '@' **is** followed by 'gin'.**@(?!gin)** --> **Lookahead negative assertion:** Matches '@' only if '@' **is not** followed by 'gin'.**(?<=gin)@** --> **Lookbehind assertion:** Matches '@' only if 'gin' **is** followed by '@'.**(?<!gin)@** --> **Lookbehind negative assertion:** Matches '@' only if 'gin' **is not** followed by '@'.
```

[](https://medium.com/javascript-in-plain-english/build-a-flush-message-middleware-with-node-js-from-scratch-843f6e9823ba) [## 用 Node.js 构建一个 Flush 消息中间件

### 了解如何使用 node.js 和 express.js 从头开始构建一个 flush messages 中间件系统

medium.com](https://medium.com/javascript-in-plain-english/build-a-flush-message-middleware-with-node-js-from-scratch-843f6e9823ba) 

# 4.真实的例子

正则表达式主要用于两个主题:

1.  输入验证
2.  搜索-替换文本中的模式

## 输入验证示例

使用以下格式验证电子邮件:

1.  至少有 4 位数字的名称
2.  没有特殊字符的名字**^<()\[\]\ \ \/。, ;:\s @ '**
3.  电子邮件必须包含@
4.  至少有 4 位数字的域名
5.  不含特殊字符的域名**^<t27】()\[\]\ \ \/。, ;:\s @ '**
6.  仅限域名扩展**。com** 或**。网**

```
var regExpEmail = **/([A-Z]|[a-z]|[^<>()\[\]\\\/.,;:\s@"]){4,}\@([A-Z]|[a-z]|[^<>()\[\]\\\/.,;:\s@"]){4,}\.(com|net)/**;var email1 = 'petranb2@gmail.com'var email2 = 'petran@pkcoding.net'var email3 = 'petran@pkcoding.org'var email4 = 'pe<>ran@pkcoding.org'console.log(`Test ${email1}:`+regExpEmail.test(email1));console.log(`Test ${email2}:`+regExpEmail.test(email2));console.log(`Test ${email3}:`+regExpEmail.test(email3));console.log(`Test ${email4}:`+regExpEmail.test(email4));**<-- Console output -->**
Test [petranb2@gmail.com](mailto:petranb2@gmail.com):true
Test [petran@pkcoding.net](mailto:petran@pkcoding.net):true
Test [petran@pkcoding.org](mailto:petran@pkcoding.org):false
Test pe<>[ran@pkcoding.org](mailto:ran@pkcoding.org):false
```

使用以下格式验证密码:

1.  密码至少 6 位数。
2.  至少一个小写字母
3.  至少一个大写字母
4.  至少一个来自 **@ # $ % ^ & *** 的特殊字符

```
var regExpPassword = **/(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#\$%\^&\*])(?=.{6,})/**;var password1 = 'J@v@scr1pt'var password2 = 'N0d3js'var password3 = 'umbr3ll@'var password4 = 'A1rpl@ne'console.log(`Test ${password1}:`+regExpPassword.test(password1));console.log(`Test ${password2}:`+regExpPassword.test(password2));console.log(`Test ${password3}:`+regExpPassword.test(password3));console.log(`Test ${password4}:`+regExpPassword.test(password4));**<-- Console output -->** Test J@v@scr1pt:true
Test N0d3js:false
Test umbr3ll@:false
Test A1rpl@ne:true
```

使用以下格式验证产品代码:

1.  代码必须具有格式(XXX)-XXX
2.  每个 X 必须是一个数字

```
var regExpProductCode = **/(\(\d{3}\))-(\d{3})/**;var product1 = '(854)-458'var product2 = '(1234)-4sw'var product3 = '256-789'var product4 = '(123)-456'console.log(`Test ${product1}:`+regExpProductCode.test(product1));console.log(`Test ${product2}:`+regExpProductCode.test(product2));console.log(`Test ${product3}:`+regExpProductCode.test(product3));console.log(`Test ${product4}:`+regExpProductCode.test(product4));**<-- Console output -->** Test (854)-458:true
Test (1234)-4sw:false
Test 256-789:false
Test (123)-456:true
```

## 搜索-替换文本中的模式

搜索在一个点之后的文本是否有 2 个或更多的空格，并用 1 个空格替换它。

```
var regExpText = **/. {2,}(?=[A-Z|a-z])/**;var text = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.   Nunc';console.log(`Test ${text}:<<`+regExpText.exec(text)+`>>`);console.log(text.replace(regExpText,'. '));**<-- Console output -->**Test Lorem ipsum dolor sit amet, consectetur adipiscing elit.   Nunc:<<.   >>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc
```

搜索文本是否以点后的小写字母开头，并替换为大写字母

```
var regExpText = **/(?<=\.\s{1,})[a-z]/**;var text = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. nunc';console.log(`Test ${text}:<<`+regExpText.exec(text)+`>>`);var lowerCaseChar = regExpText.exec(text)[0];console.log(text.replace(regExpText,lowerCaseChar.toUpperCase()));**<-- Console output -->** Test Lorem ipsum dolor sit amet, consectetur adipiscing elit. nunc:<<n>>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc
```

# 5.安全提示

正则表达式可能是一个很好的选择，但是写得不好的正则表达式可能会占用大量 CPU 资源，阻塞 node.js 事件循环。

阅读如何防止这些情况发生，请访问:

[](https://medium.com/@liran.tal/node-js-pitfalls-how-a-regex-can-bring-your-system-down-cbf1dc6c4e02) [## 正则表达式如何关闭 Node.js 服务

### 正则表达式(RegEx)的使用在软件工程师和开发人员或 IT 角色中非常普遍，他们…

medium.com](https://medium.com/@liran.tal/node-js-pitfalls-how-a-regex-can-bring-your-system-down-cbf1dc6c4e02) 

# 6.结论

学习和使用正则表达式可以让你成为一名优秀的软件开发人员。

## 参考资料:

1.  [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
2.  [维基](https://en.wikipedia.org/wiki/Regular_expression)

**特别感谢** [**李然塔尔**](https://medium.com/u/43862af38199?source=post_page-----20cfb6806f26--------------------------------)给的安全建议

# 感谢你阅读我的故事

请随意评论，给我发电子邮件，告诉我任何想法、变化等等。

[](https://medium.com/javascript-in-plain-english/how-to-implement-a-stack-in-node-js-e7b43af282d4) [## 如何在 Node.js 中实现堆栈

### 栈是编程中最重要的数据结构之一，但是如何在 javascript 中实现栈呢？

medium.com](https://medium.com/javascript-in-plain-english/how-to-implement-a-stack-in-node-js-e7b43af282d4) [](https://medium.com/javascript-in-plain-english/store-clean-data-by-validating-models-with-mongoose-f6453dbdbff9) [## 用 Mongoose 保存正确格式数据的指南

### 如何在用 mongoose 库将数据保存到 MongoDB 之前对其进行验证？

medium.com](https://medium.com/javascript-in-plain-english/store-clean-data-by-validating-models-with-mongoose-f6453dbdbff9) 

## 进一步阅读

[](/best-tool-for-web-scraping-beautifulsoup-vs-regex-vs-advanced-web-scrapers-50b8fb92950d) [## 最佳网络抓取工具:beautiful soup vs . Regex vs . Advanced Web Scrapers

### BeautifulSoup、正则表达式或高级 web scraper——哪一个是 web 抓取的最佳工具？深潜…

javascript.plainenglish.io](/best-tool-for-web-scraping-beautifulsoup-vs-regex-vs-advanced-web-scrapers-50b8fb92950d) [](https://bit.cloud/blog/component-driven-microservices-with-nodejs-and-bit-l64shurc) [## 具有 NodeJS 和 Bit 的组件驱动的微服务

### 大多数人认为组件是前端的一部分。然而，CBSE(基于组件的软件工程)是…

比特云](https://bit.cloud/blog/component-driven-microservices-with-nodejs-and-bit-l64shurc) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*[***不和***](https://discord.gg/GtDtUAvyhW) *。对增长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) *。***