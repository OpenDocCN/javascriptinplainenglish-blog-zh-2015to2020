# 在 Node.js 中使用 Sharp 来输出、调整大小和裁剪图像

> 原文：<https://javascript.plainenglish.io/using-sharp-in-nodejs-to-output-resize-and-crop-images-on-the-fly-f8b150989760?source=collection_archive---------3----------------------->

![](img/2c3c998f702b9c342cf47cfa5dc90508.png)

这是开发人员梦寐以求的工具。

我正在努力争取一个完美的 Google PageSpeed 分数(查看我的文章'[如何在 Google Pagespeed Insights 上得到 90 分](https://stuartcosten.medium.com/how-to-get-over-90-on-google-pagespeed-insights-a85255ac9a05)'以找出原因)，在点击 Lighthouse run report 按钮之前，我知道我必须做一些事情来加载我的图像而不影响我的分数。

我想实现两件事:首先，明显的，懒惰的加载，但另一个更好奇，我希望我的图像是 webp。

对于那些不知道谷歌图像格式的人，这里有一段来自[网页](https://developers.google.com/speed/webp)的片段，比我能解释的更好:

> WebP 是一种现代图像格式，为网络上的图像提供卓越的无损和有损压缩。使用 WebP，网站管理员和 web 开发人员可以创建更小、更丰富的图像，使 web 速度更快。

Webp 图像可以比 PNG 图像小 26%,所以将这种奇特的新图像格式与延迟加载结合起来，就性能而言，我感觉自己是赢家，可以利用这两者来实现。

第一次使用 NodeJs 构建我的网站时，我不知道我在使用什么工具来实现我的 Pagespeed 性能梦想。

我习惯于用 PHP 创建网站，所以我知道有一些功能可以输出 webp 格式的图像，通常需要在服务器上制作一个副本，然后通过一些。htaccess magicery。

我开始在谷歌上搜索“将图像输出为 webp NodeJs ”,不断出现的一个是[夏普](https://www.npmjs.com/package/sharp)。

Sharp 是一个 NodeJs 包，可以将各种尺寸的图像转换成各种尺寸的'*网页友好的 JPEG、PNG 和 WebP 图像。*

我在想‘太好了，这正是我需要的！’

但结果是*所以*多了。

Sharp 可以非常快速地转换图像，并声称是最快的工具，但令人难以置信的是，它可以快速转换图像，速度快如闪电。

**这是什么意思？**

服务器上不再有不同大小或类型的复制图像。

你所需要的只是一张图片，剩下的由夏普来做。

**但还有更多！**

夏普不仅可以将您的图像转换成不同的大小、质量和格式。它还可以像 CSS*background-size:cover*一样为你裁剪图片。

这将大大减少我不得不使用 css 的工作量。

那么，我做了什么来让它工作呢？

首先，我创建了一个路径，它接受图像的名称，以及大小、裁剪和类型等变量。这给了我很大的力量来建立我的网站，因为把一张图片放入一个空间不是问题。

首先，为不支持 webp 的浏览器提供的带有 png 回退的 html:

```
<picture>
 <source type=”image/webp” srcset=”/site/[image_name].png?format=webp&width=350&height=200&crop=cover&quality=100">
 <source type=”image/png” srcset=”/site/[image_name].png?format=png&width=350&height=200&crop=cover&quality=80"> 
 <img src=”/site/[image_name].png?format=jpg&width=350&height=200&crop=cover&quality=10">
 </picture>
```

正如你所看到的，图片的 URL 包含了生成 3 种图片类型的所有参数。

上面的例子显示了 PNG，但是你可以将任何类型传入其中，它将被转换成 URL 中声明的任何格式。

现在 NodeJs 部分:

*安装夏普包…*

```
npm install sharp
```

*用这个代码设置路线…*

```
const express = require(“express”);
const router = express.Router();
const fs = require(“fs”);
const Sharp = require(‘sharp’);router.get(‘/:imageName’, (req, res, next) => {//set image caches
res.set(‘Cache-Control’, ‘public, max-age=31557600’);//get the original image path
var imagePath = “./public/images/” + req._parsedOriginalUrl.pathname;//check if it has a format, if not choose webp
const format = req.query.format ? req.query.format : “webp”;//ignore this line :-) wordpress throwback!
imagePath = imagePath.replace(“-scaled”, “”);//if it cant find an image show the image not found image
if (!fs.existsSync(imagePath)) {
 imagePath = “./public/images/image-not-found.png”;
}//get the variables from the url
const width = req.query.width ? parseInt(req.query.width) : 500;
const height = req.query.height ? parseInt(req.query.height) : 400;
const crop = req.query.crop ? req.query.crop : “cover”;
const quality = req.query.quality ? req.query.quality :100;//create the stream — so basically read the path
const stream = fs.createReadStream(imagePath);//transform the image based on our variable, then output using the qualityconst transform = Sharp().resize(width, height, {
  fit: crop
}).toFormat(format, {
  quality: parseInt(quality)
});//make sure the content type is set for the correct format.
res.set(‘Content-Type’, `image/${format}`);//then output the image
stream.pipe(transform).on(‘error’, (e) => {}).pipe(res);return stream;});module.exports = router;
```

这就是全部了！

感谢您阅读我的教程，希望这将有助于您追求最小的服务器空间使用和一个真正优雅的方式输出图像到您的网站上使用 NodeJs 和夏普。