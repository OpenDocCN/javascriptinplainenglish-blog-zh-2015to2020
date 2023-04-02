# 在 Angular 中创建自定义管道，用文本替换空值

> 原文：<https://javascript.plainenglish.io/create-custom-pipe-in-angular-to-replace-null-value-with-a-text-376b58b15b61?source=collection_archive---------3----------------------->

## 基本管道概述和在 Angular 中创建自定义管道的逐步过程

## 一点背景知识

Angular 中的管道是在 HTML 模板中转换数据的一种方式。它不会更改 TypeScript 文件中数据的值。管道接收数据作为输入，并将其转换为所需的输出。管道的一个很好的用例是转换数据格式。通常，我们从 API 数据中获得的日期时间格式并不是我们想要显示给用户的格式。这时，我们可以使用管道将数据转换成所需的格式。

![](img/72b51e41734cdcd3b6636b9bb0ab3cdc.png)

Photo by [Oskar Yildiz](https://unsplash.com/@oskaryil?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 内置管道

Angular 为典型的数据转换提供了[内置管道](https://angular.io/api/common#pipes)，例如 DatePipe、UpperCasePipe、LowerCasePipe 等。以便我们可以将数据转换成 HTML 模板中所需的格式。现在，让我们看一些关于 DatePipe 的例子。

在下面的代码中(官方[示例](https://angular.io/api/common/DatePipe#usage-example)的略微修改版本)，我们使用了**日期**管道。我们必须将管道放在模板表达式中的垂直线(|)之后。

管道可以接受可选参数来修改输出。要向管道传递参数，只需在管道表达式的末尾添加一个冒号和参数值。在下面的代码中，我们将' shortTime '、' medium '作为参数传递给 **date** 管道。

我们将在浏览器中得到下面的输出。

```
Today is Sep 25, 2020Or if you prefer, 09/25/2020The time is 2:16 PMDate from time stamp is Sep 25, 2020, 2:11:19 PM
```

## 我们定制管道的动机

我们都喜欢给用户最好的用户体验。因此，如果值为 null，用户在浏览器中看不到任何内容。不是很好的用户体验。我们将通过创建自己的自定义管道来解决这个问题，该管道将在模板表达式中用给定的文本替换空值或未定义的值。

## 让我们创建我们的自定义管道

我们将使用 Angular CLI 通过下面的命令创建我们自己的自定义管道。

```
ng generate pipe replace-null-with-text
```

Angular CLI 将为我们的自定义管道生成一个框架文件。

在 **@Pipe** 装饰器中指定的**名称**是管道的名称(例如**replaceullwithtext**)。我们将在 HTML 中使用这个名称，就像 **date** 管道示例一样。

在**转换**方法中，**值**参数是我们从 HTML 模板中获取的值，而 **args** 是我们想要发送给自定义管道的可选参数。请注意，您不必使用相同的参数名称，但请记住参数位置很重要。第一个参数总是从 HTML 模板中获取值，并且至少有一个参数是必需的。

*如果您正在 SharedModule 之类的 Angular 模块中创建管道，那么不要忘记从 module.ts 文件中将其导出，否则您将无法在其他模块中使用它。或者，如果您没有使用 Angular CLI 来生成框架文件，那么您也必须在模块声明中添加自定义管道类名。*

对于我们的自定义管道，我们希望它返回一个文本值，当来自 HTML 模板的值为 null 时，我们将把这个文本值作为参数从模板表达式中传递。因此，在下面的代码中，我们检查 HTML 模板中的值是否为空或未定义。如果值为空/未定义，那么我们将返回 **replaceText** 参数值，或者我们将返回相同的值。

我们还为 **replaceText** 添加了一个默认值‘N/A’。因此，如果我们不从 HTML 模板传递任何参数，那么对于 null/undefined 值，我们的自定义管道将返回' N/A '。

现在让我们来看看我们的定制管道的运行情况。😃

我们将在浏览器中得到以下输出。

```
Book Name : Awesome BookAuthor Name : Author Name Not AvailablePublished at : Published Date Not AvailablePrice: N/A
```

**book** 变量不为空，因此我们的自定义管道不会更改该值，我们在输出中不会看到任何类型的 book value 转换。但是由于我们的 **author** 和 **publishedAt** 值为空，我们的自定义管道将返回我们从 HTML 模板传递的 **replaceText** 值作为自定义管道的参数，对于 **price** 我们没有传递任何参数值来替换空值，因此我们的自定义管道将返回默认的 replaceText 值‘N/A’。

耶！😃我们得到了我们的定制角管。

*我也几乎每天都在学 Angular。所以，如果我犯了什么错误，请随时纠正我，并在评论区提出你的建议。*