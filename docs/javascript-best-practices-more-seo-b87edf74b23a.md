# JavaScript 最佳实践—更多 SEO

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-more-seo-b87edf74b23a?source=collection_archive---------6----------------------->

![](img/dabaa984c0042bc00a21f5a922581e1b.png)

Photo by [Andrew Small](https://unsplash.com/@andsmall?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 小心使用 Meta Robots 标签

我们可以添加`noindex` meta 标签来防止索引。

我们可以添加一个静态的 meta 标签:

```
<meta name="robots" content="noindex, nofollow">
```

我们可以通过以下方式动态添加一个:

```
fetch(`/api/items/${itemId}`)
  .then(response => response.json())
  .then(item=> {
    if (item.exists) {
      showItem(item); 
    } else {
      const metaRobots = document.createElement('meta');
      metaRobots.name = 'robots';
      metaRobots.content = 'noindex';
      document.head.appendChild(metaRobots);
    }
  })
```

我们创建 meta 元素，添加属性，并用`document.head.appendChild`将它附加到 head 标签上。

如果谷歌遇到`noindex`，那么 JavaScript 就不会运行。

如果带有`noindex`的 meta 标签被删除，它将对页面进行索引。

# 使用长期缓存

我们应该缓存资产以减少加载时间。

这可能会导致加载过时的资源。

为了在部署新内容时清除缓存，我们可以为它们的文件名添加一个唯一的指纹。

# 使用结构化数据

我们可以添加带有脚本标签的结构化数据:

```
<script type="application/ld+json">
  {
    "@context": "https://schema.org/",
    "@type": "Recipe",
    "name": "{{recipe_name}}",
    "image": [ "{{recipe_image}}" ],
    "author": {
      "@type": "Person",
      "name": "{{recipe_author}}"
    }
  }
</script>
```

这让谷歌知道我们页面的内容。

为了工作，必须将`type`设置为`“application/ld+json”`。

# 遵循 Web 组件的最佳实践

我们应该确保 Google 看到我们的 web 组件的呈现内容。

如果它是不可见的，那么谷歌不能索引它。

为了测试它们，我们可以使用[移动友好测试](https://g.co/mobilefriendly)或 [URL 检查工具](https://support.google.com/webmasters/answer/9012289)来查看呈现的 HTML。

为了确保光线 DOM 和阴影 DOM 都被渲染，我们应该使用`slot`元素:

```
<script>
  class MyComponent extends HTMLElement {
    constructor() {
      super();
      this.attachShadow({ mode: 'open' });
    } connectedCallback() {
      let p = document.createElement('p');
      p.innerHTML = 'Hello World: <slot></slot>';
      this.shadowRoot.appendChild(p);
    }
  }window.customElements.define('my-component', MyComponent);
</script><my-component>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
  <p>Suspendisse vitae eleifend nulla.</p>
</my-component>
```

我们在 shadow DOM 中使用了`slot`来呈现标签之间的项目。

然后一切都会显现。

# 修复图像和延迟加载的内容

如果我们有图像，我们应该延迟加载它们，这样它们只在显示给用户时才显示。

这减少了加载时间，因此用户和谷歌可以更快地看到呈现的内容。

# 标题

我们的页面需要一个标题。

我们可以添加一个标题标签:

```
<head>
   <title>Page Title</title>
</head>
```

这让谷歌很容易识别页面。

# 描述

此外，我们可以添加元描述来向搜索引擎描述我们的内容。

例如，我们可以写:

```
<head>
   <meta name="description" content="description of the page" />
 </head>
```

# 打开图形图像

为了让我们的页面显示社交媒体的特色图片，我们需要 open graph image 标签。

例如，我们可以写:

```
<head>
   <meta property="og:image" content="https:/example.com/image.png" />
 </head>
```

![](img/049cd217516d24694de8e0cc10e3e155.png)

Photo by [Don Kawahigashi](https://unsplash.com/@dkawahig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以通过延迟加载、缓存和遵循 web 组件的最佳实践来提高 web 应用的排名。

此外，我们应该为搜索引擎和社交媒体添加一些元标签。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**