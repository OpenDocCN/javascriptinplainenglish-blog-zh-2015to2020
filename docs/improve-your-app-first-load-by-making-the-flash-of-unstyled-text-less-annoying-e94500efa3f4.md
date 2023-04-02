# 通过减少无样式文本的闪烁来改善你的应用首次加载

> 原文：<https://javascript.plainenglish.io/improve-your-app-first-load-by-making-the-flash-of-unstyled-text-less-annoying-e94500efa3f4?source=collection_archive---------4----------------------->

## 图书馆发现

## 使用自定义字体有一些你可以考虑的含义，尤其是在开发速度慢/不稳定的连接时。

![](img/089245ec56b85db941546e1c8701d3fd.png)

如果您正在开发 web 应用程序，您可能会遇到以下情况，页面加载后几毫秒内文本发生变化。

![](img/7225d92c54bc3d0f8f11d9dde06341b1.png)

This is a FLOUT

如果你是一个“年轻”的开发者，你可能不会有太多的体验，因为**现在我们几乎每个人都有巨大的互联网带宽**。但是几年前，这种情况还是很常见的。

考虑到这一点，你现在可能会说

> 好吧，安德烈亚斯，但这不再是 2020 年的问题了。我为什么要担心？

是，也不是…事实上，即使在像巴黎“拉德芳斯”区这样的市中心，您可能会体验到良好的 4G 数据使用，但仍然有一些地方的互联网速度很慢，甚至是在 4G 工作的同一个地方，原因有多种:

*   同一个地方有太多的人
*   电子干扰，使连接不稳定
*   没有光纤也没有无线电中继(非洲，或一些退休的地方)

如果您想了解更多，请随意查看我的关于应用程序离线注意事项的文章。

[](https://medium.com/javascript-in-plain-english/general-considerations-for-offline-resilient-react-based-apps-8dcb7181e495) [## 基于离线弹性反应的应用程序的一般注意事项

### 你应该总是首先把你的应用程序设计成离线的，并且把网络看作是一个增强和…

medium.com](https://medium.com/javascript-in-plain-english/general-considerations-for-offline-resilient-react-based-apps-8dcb7181e495) 

**那么优化字体加载是个好主意吗？**

当然是的，这可能是个好主意。考虑到实施我们将在下面看到的内容所需的时间，你将为这些人提高你的 UX，并让他们开心。

# 为什么页面加载时字体会闪烁？

当你加载网页时，浏览器会寻找你在 CSS 表单中定义的字体，比如这里的`Output Sans`字体。

```
**@font-face** {
  font-family: Output Sans;
  src: url(output-sans.woff2) format('woff2'),
       url(output-sans.woff) format('woff');
}
```

**加载带有该字体的页面时，浏览器上会出现什么情况？**

简而言之，事情是这样的:

1.  浏览器下载 HTML 文档。
2.  它解析它并下载相关资源，如 CSS 表和 JS(如果没有内联)。
3.  然后，它解析这些资产，并解析它们自己的依赖关系(如果它们有一些依赖关系的话)，在我们的例子中是我们的字体。
4.  加载后，它会用正确的字体重新绘制用户界面，触发用户界面的变化，你可能会看到一个“闪光”

**这些操作会导致大量的往返。**

您遇到的小闪烁是由于浏览器显示文本时使用了您可以指定的备用字体:`font-family: "Output Sans", Times, serif;`或者它将依赖于系统默认字体。

然后，当主字体加载后，它会用新字体替换整个文档字体。

如果两者在大小上有太多的差异，这看起来可能不那么奇怪，但是如果你选择了一个非常不同的字体，你可能会看到一些闪光。

**如果你在页面上使用不同的字体，情况会更糟。**

# 我可以做些什么来防止页面加载时出现字体闪烁效果？

好吧，虽然没有魔术，你能做的就是让默认字体最接近你想要的字体。

**你可以调整你的风格，让 FOUT 不那么刺耳**；例如通过确保闪现的字体和加载的 web 字体看起来大小相同。

或者，也许隐藏您的文本，直到字体完全加载。您甚至可以在字体加载时显示页面加载器或动画占位符。

这样，您可以确保在呈现任何文本之前，字体已完全加载。

为了实现这一目标，有各种基于定制类或基于 React 等 web 框架的技术。

请注意，随着使用诸如 **NextJS** 或 **NuxtJS** 等框架的服务器渲染的“回归”，HTML 的第一次渲染是在 JS 框架在前端初始化时完成的，因此基于 CSS 的技术是相关的。

当然，我不是第一个想到这一点的人，有专门为此目的设计的库，我想在那篇文章中介绍它。

# 介绍 FontFace Observer:一个在浏览器中加载字体的小库

就库而言，主要有两个可用于此目的，两者都有相似的方法，但优化方式不同，或多或少有些固执己见

## 网络字体加载器

[](https://github.com/typekit/webfontloader) [## typekit/webfontloader

### 当通过@font-face 使用链接字体时，Web 字体加载器为您提供额外的控制。- typekit/webfontloader

github.com](https://github.com/typekit/webfontloader) 

这是一个有点固执己见，给了一点开销，所以我不会在这里开发它，随意看看它。

## 正面观察者

[](https://github.com/bramstein/fontfaceobserver) [## 布拉姆斯坦/fontfaceobserver

### Font Face Observer 是一个小型的@font-face 加载器和监视器(3.5KB 缩小版和 1.3KB gzipped 版)，兼容任何…

github.com](https://github.com/bramstein/fontfaceobserver) 

字体外观观察器使用滚动事件以最小的开销有效地检测字体加载。

文档设计得真的很好，因为它不是真的固执己见，你可以依赖任何你想要的策略。

首先用`npm install fontfaceobserver`或`yarn add fontfaceobserver`开始安装。

然后在 CSS 表单中注册你的字体

```
**@font-face** {
  font-family: Output Sans;
  src: url(output-sans.woff2) format('woff2'),
       url(output-sans.woff) format('woff');
}html.fonts-ready {
  font-family: 'Output Sans', cursive;
  font-size: 18px;
}html.fonts-loading {
  font-family: serif;
  font-size: 18px;
}
```

使用下面的一些片段

```
**const** font = new FontFaceObserver("Output Sans");

font.load().then(() => {
  console.log('Output Sans has loaded.'); // Add and remove custom classes
  document.documentElement.classList.add("fonts-ready");
  document.documentElement.classList.remove("fonts-loading");
});
```

在 React 中，你可以使用某种`useLayoutEffect`钩子来达到同样的目的，同时显示其他东西。

请记住，在大多数情况下，当 React 及其挂钩被触发时，字体应该已经加载完毕。因此，在安装 React 应用程序之前，在其他地方这样做可能更有意义

# 更进一步

您可以在 Adobe 阅读这篇小文章:

 [## 字体事件

### 字体事件可用于控制页面在字体仍在加载或无法加载时的显示方式

helpx.adobe.com](https://helpx.adobe.com/fonts/using/font-events.html) 

或者浏览以下网站提供的众多资源:

 [## 字体观察者

### 字体外观观察员给你控制网页字体加载使用一个简单的承诺为基础的界面。没关系…

fontfaceobserver.com](https://fontfaceobserver.com/) 

一如既往随时订阅更多:-)

[**🇫🇷STOP！你是法国人吗🥖？**您也可以访问 ici 网站，接收法国的私人通讯🙂](https://codingspark.io/)

## **一封来自简明英语的短信**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎