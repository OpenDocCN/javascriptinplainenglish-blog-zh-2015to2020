# 7 个没人谈论的免费 API

> 原文：<https://javascript.plainenglish.io/7-free-apis-that-nobody-is-talking-about-cf974e15917?source=collection_archive---------0----------------------->

## 使用这些 API 创建独特而有趣的应用程序

![](img/b41a8d7edc7d8ade3de1d47a507bcdc2.png)

Photo by [Tachina Lee](https://unsplash.com/@chne_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

没有什么比找到一个与众不同的 API 更让我兴奋的了。

很多时候，我们只想关注前端，但也需要有趣的动态数据来显示。

这就是公共 API 发挥作用的地方。API 是应用编程接口的缩写。

使用它的核心好处是它允许一个程序与其他程序交互。

使用公共 API 可以让您专注于前端和重要的事情，而不必太担心数据库和后端。

下面是 7 个较少被提及的公共和免费 API。

## 1.[恶辱发生器](https://evilinsult.com/generate_insult.php?lang=en&type=json)

你有多少次试图侮辱你最好的朋友？现在你有帮手了！

正如 API 名称所暗示的，目标是提供一些最邪恶的侮辱。

你可以围绕这个 API 创建一个应用程序，或者将这个 API 与下面提供的其他优秀的 API 结合起来，比如在 meme 模板中实现生成的侮辱。

这个 API 使用起来非常简单。你只需要[访问一个 URL](https://evilinsult.com/generate_insult.php?lang=en&type=json) 就可以得到想要的 JSON 输出，甚至不需要注册一个密钥。

API 的输出示例如下:

```
{
"number":"117",
"language":"en",
"insult":"Some cause happiness wherever they go; others, whenever they go.",
"created":"2020-11-22 23:00:15",
"shown":"45712",
"createdby":"",
"active":"1",
"comment":"http:\/\/www.mirror.co.uk\/news\/weird-news\/worlds-20-most-bizarre-insults-7171396"
}
```

您还可以获得其他属性，如创建时间、语言、任何评论以及视图。

## 2.电影和电视宣传短片

[TMDb](https://www.themoviedb.org/documentation/api) 是一个著名的 API，但是你知道还有其他 API 可以提供来自特定节目和电影的见解吗？

以下是一些 API，您可以使用它们来开发以您最喜爱的节目为特色的应用程序:

1.  [绝命 API](https://breakingbadapi.com/documentation)
2.  [冰与火 API](https://anapioficeandfire.com/)
3.  [哈利波特 API](https://www.potterapi.com/)
4.  [YouTube API](https://developers.google.com/youtube/) (用于嵌入 YouTube 功能)
5.  [魔戒 API](https://the-one-api.dev/)

像上面的 API 一样，你甚至不需要注册一个密钥就可以开始使用一些 API。

不仅如此，使用非版权图像，您可以真正为您喜爱的节目创建一个伟大的粉丝应用程序。

下面是绝命毒师 API 的输出示例，你可以在这里获得[。](https://breakingbadapi.com/api/quotes)

它不需要一个键，但是每天有 10，000 个请求的速率限制。

```
{
   [
      {
         "quote_id":1,
         "quote":"I am not in danger, Skyler. I am the danger!",
         "author":"Walter White",
         "series":"Breaking Bad"
      },
      {
         "quote_id":2,
         "quote":"Stay out of my territory.",
         "author":"Walter White",
         "series":"Breaking Bad"
      },
      {
         "quote_id":3,
         "quote":"IFT",
         "author":"Skyler White",
         "series":"Breaking Bad"
      }
      .....
   ]
}
```

它返回一个 JSON，其中包含一组带引号的对象、引号的作者和一个 ID。

你可以将这些专用的 API 与 YouTube API 混合，为这些节目的粉丝创建一个终极应用。

## 3.[地图框](https://docs.mapbox.com/api/)

[Mapbox](https://docs.mapbox.com/api/maps/) 为开发者提供精确的位置信息和成熟的工具。

您可以即时访问动态、实时更新的地图，甚至可以进一步定制！

如果你有一个面向位置和地图的项目，这是一个必须知道的 API。

不过，值得一提的是，你必须免费注册才能获得唯一的访问令牌。

使用这个令牌，您可以使用这个 API 提供的令人惊叹的服务。

不仅如此，您还可以将 Mapbox 与诸如 fleet . js 库之类的库一起使用，并创建美观、移动友好的地图。

在我最近的一篇介绍 Mapbox 和 fleet . js 基础知识的文章中，我讨论了这一点以及更多内容。

[](https://medium.com/javascript-in-plain-english/add-interactive-maps-to-your-website-5e935924ff5c) [## 向您的网站添加交互式地图

### 不用谷歌地图！

medium.com](https://medium.com/javascript-in-plain-english/add-interactive-maps-to-your-website-5e935924ff5c) 

## 4.[美国宇航局 API](https://api.nasa.gov/)

美国宇航局提供了一个神话般的与空间相关的信息更新数据库。

使用这个 API，你可以创建迷人的、有教育意义的应用程序和网站。

你可以访问各种不同的数据，从当天的天文照片到火星车拍摄的照片。

你可以在这里浏览整个列表。

您还可以检索 NASA 的专利、软件和技术派生描述，您可以用它们来建立专利组合。

这个 API 非常多样化，提供了各种各样的数据。你甚至可以使用它访问 [NASA 图像和视频](https://images.nasa.gov/)库。

下面是好奇号在火星上拍摄的图片的查询示例。

```
{
   "photos":[
      {
         "id":102693,
         "sol":1000,
         "camera":{
            "id":20,
            "name":"FHAZ",
            "rover_id":5,
            "full_name":"Front Hazard Avoidance Camera"
         },
         "img_src":"[http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/01000/opgs/edr/fcam/FLB_486265257EDR_F0481570FHAZ00323M_.JPG](http://mars.jpl.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/01000/opgs/edr/fcam/FLB_486265257EDR_F0481570FHAZ00323M_.JPG)",
         "earth_date":"2015-05-30",
         "rover":{
            "id":5,
            "name":"Curiosity",
            "landing_date":"2012-08-06",
            "launch_date":"2011-11-26",
            "status":"active"
         }
      },
     .....
   ]
}
```

## 5. [GIF 搜索](https://developers.giphy.com/docs/api)

Source: [GIPHY](https://giphy.com/gifs/impastortv-tv-land-tvland-impastor-3o7TKr2xg9OWcU8DWo)

我们都喜欢使用和创建 gif，但是你知道你可以使用 [GIPHY](https://developers.giphy.com/docs/api#quick-start-guide) 免费将 gif 整合到你的下一个应用中吗？

GIPHY 是目前世界上最大的 GIF 和贴纸库，使用他们的官方 API，你可以利用庞大的集合免费制作独特的应用程序。

使用[搜索端点](https://developers.giphy.com/docs/api/endpoint/#search)，用户可以根据他们的查询获得最相关的 gif。

您还可以访问分析和其他工具，从而创建个性化的用户体验。

然而，对我来说最常用的功能是 [translate endpoint](https://developers.giphy.com/docs/api/endpoint#translate:~:text=Translate%20Endpoint,-GIPHY) ，它可以将单词和短语转换成完美的 GIF 或贴纸。您可以在 0-10 的范围内指定怪异程度。

请注意，您必须通过显示**“由 GIPHY 提供动力”**来提供适当的属性，无论在哪里使用 API。

下面是这个 API 的输出示例:

```
{data: [GIF Object[]](https://developers.giphy.com/docs/api/schema#gif-object)pagination: [Pagination Object](https://developers.giphy.com/docs/api/schema#pagination-object)meta: [Meta Object](https://developers.giphy.com/docs/api/schema#meta-object)}
```

## 6.[最爱语录 API](https://favqs.com/api/)

顾名思义，这个 API 为您提供了一个周到的报价集合。

您可以使用这些引用来显示在您网站的登录页面或应用程序的闪屏上，以产生丰富的用户体验。

您还可以通过这个 API 创建和管理用户和会话。但是，在每个会话的 20 秒间隔内，存在 30 个请求的速率限制。

这个 API 还具有过滤、投票、列出、更新和删除报价的端点。

下面是当天终点的[报价的输出。](https://favqs.com/api/qotd)

```
{
   "qotd_date":"2020-11-23T00:00:00.000+00:00",
   "quote":{
      "id":29463,
      "dialogue":false,
      "private":false,
      "tags":[
         "great"
      ],
      "url":"[https://favqs.com/quotes/walt-whitman/29463-the-great-cit-](https://favqs.com/quotes/walt-whitman/29463-the-great-cit-)",
      "favorites_count":1,
      "upvotes_count":2,
      "downvotes_count":0,
      "author":"Walt Whitman",
      "author_permalink":"walt-whitman",
      "body":"The great city is that which has the greatest man or woman: if it be a few ragged huts, it is still the greatest city in the whole world."
   }
}
```

## 7.[毛豆营养和配方分析 API](https://www.edamam.com/)

毛豆慷慨地提供了超过 700，000 种食物和[170 万+营养分析](https://www.edamam.com/#navbar-brand-centered:~:text=1.7%2B%20million%20nutritionally)食谱的[数据库。](https://www.edamam.com/#navbar-brand-centered:~:text=Database%20of%20700%2C000%2B%20food%20items)

如果你想展示你的前端技能，这个 API 是很棒的，因为你可以在这个 API 提供的食物食谱旁边添加高质量的食物图片。

免费计划不能用于商业，但它提供了一套全面的功能，如自然语言处理支持和每月 200 个食谱。

您可以在这里找到关于不同计划的全部细节[。](https://developer.edamam.com/edamam-nutrition-api)

用户只需输入配料，就能获得营养分析，帮助他们吃得更聪明、更好。

你可以在这个 API 的演示中点击查看这个很酷的特性[。](https://developer.edamam.com/edamam-nutrition-api-demo)

他们还有其他 API,可以与其他 API 结合使用，创建一站式美食应用。

他们增加了一种新的饮食过滤器，专门针对正在进行的疫情，该过滤器利用关于营养物质和食物的科学出版物来增强免疫力。

## 最后的想法

API 是任何应用成功的重要工具。

通过使用第三方公共 API，开发人员可以专注于重要的事情，同时通过这些 API 方便地为他们的应用程序添加强大的功能。

然而，和其他人一样使用相同的 API 不仅会产生不必要的竞争，而且不会提供任何真正的价值。

利用独特和灵活的 API 可以创建一些非常漂亮的项目，您可以在您的专业作品集中展示这些项目。