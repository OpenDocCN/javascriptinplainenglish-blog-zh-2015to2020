# Nuxt 内容中的社交分享图片

> 原文：<https://javascript.plainenglish.io/social-share-images-in-nuxt-content-be5f2ae36755?source=collection_archive---------13----------------------->

![](img/04a152fb67767feb334ab99d3b001bd2.png)

# 介绍

在社交媒体上分享博客内容或文章时，脱颖而出很重要。在 Twitter 帖子的海洋中，如果博客预览不够吸引眼球，用户可能会简单地滚动浏览你努力撰写的一篇文章！

在这篇文章中，我们将教你如何为你的 Nuxt 内容博客文章制作漂亮的分享卡！这篇文章将使用 Jason Lengstorfs 的精彩文章中的概念，他在文章中详细介绍了如何使用 Cloundinary 的 API 和自定义模板为文章生成图像，但是我们将更关注 Nuxt 内容！

我建议在继续之前去阅读他的帖子，因为你需要在 Cloundinary 中设置自己的模板，并上传任何你想用于模板的自定义字体。

# 设置

这篇文章不会详细介绍如何从头开始建立一个 Nuxt 内容博客，但是不言而喻的是，要确保你已经安装了`@nuxt/content`包并添加到你的`nuxt.config.js`模块中，如下所示:

```
modules: [
  '@nuxt/content',
],
```

为了开始生成动态社交媒体卡，我们还需要安装杰森·伦斯托夫的软件包`@jlengstorf/get-share-image`。

```
# install using npm 
npm install --save @jlengstorf/get-share-image # install using yarn 
yarn add @jlengstorf/get-share-image
```

一旦你安装好了所有的东西，并把你的模板上传到 Cloudinary，就该开始生成你的图片了！

# 获取博客并生成图像

在一个动态页面组件中(我的博客页面在/blog/_slug.vue 中),我们需要使用`asyncData` Nuxt 钩子，因为它在`head`方法之前被调用，我们需要在那里为文章设置打开的图表和 Twitter 元数据。

我们将从从`'@jlengstorf/get-share-image'`导入`getShareImage`开始，然后在获取我们特定页面的文章后从`asyncData`内部调用这个函数。

```
<script>
import getShareImage from '@jlengstorf/get-share-image';

export default {
  async asyncData({ $content, params }) {
    const article = await $content('blogs', params.slug).fetch()

    const socialImage = getShareImage({
        title: article.title,
        tagline:  article.subtitle,
        cloudName: 'YOUR_CLOUDINARY_NAME',
        imagePublicID: 'YOUR_TEMPLATE_NAME.png',
        titleFont: 'unienueueitalic.otf',
        titleExtraConfig: '_line_spacing_-10',
        taglineFont: 'unienueueitalic.otf',
        titleFontSize: '72',
        taglineFontSize: '48',
        titleColor: 'fff',
        taglineColor: '6CE3D4',
        textLeftOffset: '100',
        titleBottomOffset: '350',
        taglineTopOffset: '380'
      });

    return { article, socialImage }
  }
}
</script>
```

`getShareImage`函数将使用指定的文本、变换、颜色和字体通过 Cloudinary 生成一个图像 URL。例如，我这篇文章的网址是

```
[https://res.cloudinary.com/dzxp4ujfz/image/upload/w_1280,h_669,c_fill,q_auto,f_auto/w_760,c_fit,co_rgb:fff,g_south_west,x_100,y_350,l_text:unienueueitalic.otf_72_line_spacing_-10:Social%20Share%20Images%20in%20Nuxt%20Content/w_760,c_fit,co_rgb:6CE3D4,g_north_west,x_100,y_380,l_text:unienueueitalic.otf_48:Beautiful%20social%20sharing%20cards%20for%20your%20Nuxt%20Content%20blogs/template_oxlcmb.png](https://res.cloudinary.com/dzxp4ujfz/image/upload/w_1280,h_669,c_fill,q_auto,f_auto/w_760,c_fit,co_rgb:fff,g_south_west,x_100,y_350,l_text:unienueueitalic.otf_72_line_spacing_-10:Social%20Share%20Images%20in%20Nuxt%20Content/w_760,c_fit,co_rgb:6CE3D4,g_north_west,x_100,y_380,l_text:unienueueitalic.otf_48:Beautiful%20social%20sharing%20cards%20for%20your%20Nuxt%20Content%20blogs/template_oxlcmb.png)
```

因为我已经创建了自己的模板，并包含了自己的字体，所以当设置`textLeftOffset`或其他偏移量时，我的设置可能会与你的不同。包的 [Github 页面上提供了您可以设置的属性的完整列表。](https://github.com/jlengstorf/get-share-image#readme)

请随意在此处查看杰森·凌斯托夫的 Figma 模板，并根据您的喜好进行定制。

# 设置元标签

太好了，我们正在通过动态的 Nuxt Content 文章属性来生成我们的图像！

现在，我们如何将这些变量注入我们的博客页面“head ”,以便社交媒体用户能够看到我们的图像和元数据？

为此，我们将利用内置在 [head](https://nuxtjs.org/api/pages-head/) 方法中的 Nuxt.js，该方法允许我们设置 Open Graph 和 Twitter 元标签。我们还会包括一些有用的信息，比如文章发表的时间，以及最后一次使用 Nuxt Content 自动为我们注入的`createdAt`和`updatedAt`属性修改文章的时间。

```
<script>
import getShareImage from '@jlengstorf/get-share-image';
import getSiteMeta from "~/utils/getSiteMeta.js";

export default {
  async asyncData({ $content, params }) {
    const article = await $content('blogs', params.slug).fetch()

    const socialImage = getShareImage({
        title: article.title,
        tagline:  article.subtitle,
        cloudName: 'YOUR_CLOUDINARY_NAME',
        imagePublicID: 'YOUR_TEMPLATE_NAME.png',
        titleFont: 'unienueueitalic.otf',
        titleExtraConfig: '_line_spacing_-10',
        taglineFont: 'unienueueitalic.otf',
        titleFontSize: '72',
        taglineFontSize: '48',
        titleColor: 'fff',
        taglineColor: '6CE3D4',
        textLeftOffset: '100',
        titleBottomOffset: '350',
        taglineTopOffset: '380'
      });

    return { article, socialImage }
  },
  computed: {
    meta() {
      const metaData = {
        type: "article",
        title: this.article.title,
        description: this.article.description,
        url: `https://davidparks.dev/blog/${this.$route.params.slug}`,
        mainImage: this.socialImage,
      };
      return getSiteMeta(metaData);
    }
  },
  head() {
    return {
      title: this.article.title,
      meta: [
        ...this.meta,
        {
          property: "article:published_time",
          content: this.article.createdAt,
        },
        {
          property: "article:modified_time",
          content: this.article.updatedAt,
        },
        {
          property: "article:tag",
          content: this.article.tags ? this.article.tags.toString() : "",
        },
        { name: "twitter:label1", content: "Written by" },
        { name: "twitter:data1", content: "David Parks" },
        { name: "twitter:label2", content: "Filed under" },
        {
          name: "twitter:data2",
          content: this.article.tags ? this.article.tags.toString() : "",
        },
      ],
      link: [
        {
          hid: "canonical",
          rel: "canonical",
          href: `https://davidparks.dev/blog/${this.$route.params.slug}`,
        },
      ],
    };
  }
}
</script>
```

您已经在上面注意到我正在从`"~/utils/getSiteMeta.js"`进口`getSiteMeta`。这是一个我用来覆盖默认元标签的实用函数。如果显式提供，我们将使用计算属性来覆盖我在这个文件中设置的一些默认元数据值。这确保了我们正在将 Nuxt 内容降价文件中的适当变量注入我们的大脑。该文件如下所示:

```
const type = "website";
const url = "https://davidparks.dev";
const title = "David Parks";
const description = "David Parks is a Front-end Developer from Milwaukee, Wisconsin. This blog will focus on Nuxt.js, Vue.js, CSS, Animation and more!";
const mainImage = "https://davidparksdev.s3.us-east-2.amazonaws.com/template.png";
const twitterSite = "@dparksdev";
const twitterCard = "summary_large_image"
export default (meta) => {
  return [
    {
      hid: "description",
      name: "description",
      content: (meta && meta.description) || description,
    },
    {
      hid: "og:type",
      property: "og:type",
      content: (meta && meta.type) || type,
    },
    {
      hid: "og:url",
      property: "og:url",
      content: (meta && meta.url) || url,
    },
    {
      hid: "og:title",
      property: "og:title",
      content: (meta && meta.title) || title,
    },
    {
      hid: "og:description",
      property: "og:description",
      content: (meta && meta.description) || description,
    },
    {
      hid: "og:image",
      property: "og:image",
      content: (meta && meta.mainImage) || mainImage,
    },
    {
      hid: "twitter:url",
      name: "twitter:url",
      content: (meta && meta.url) || url,
    },
    {
      hid: "twitter:title",
      name: "twitter:title",
      content: (meta && meta.title) || title,
    },
    {
      hid: "twitter:description",
      name: "twitter:description",
      content: (meta && meta.description) || description,
    },
    {
      hid: "twitter:image",
      name: "twitter:image",
      content: (meta && meta.mainImage) || mainImage,
    },
    { 
      hid: "twitter:site",
      name: "twitter:site", 
      content: (meta && meta.twitterSite) || twitterSite,
    },
    { 
      hid: "twitter:card",
      name: "twitter:card", 
      content: (meta && meta.twitterCard) || twitterCard,
    }
  ];
};
```

除非有显式提供的覆盖，否则它将使用我在该文件顶部定义的回退值。如果你想避免那些你忘记设置元标签的情况，这是很好的！

计算出的属性`meta`然后通过扩展运算符`...this.meta,`被合并到`head`方法中。这将确保任何默认值都被覆盖，并且您的文章标题、描述和图像被正确地注入到您的文档头中。

# 使用脸书和推特工具进行测试

如果一切顺利，您现在应该可以在 DOM 中看到这些元标签了！

下一次你的网站部署时，当你在推特、脸书、领英或其他任何地方分享你的博客时，你会看到一个令人敬畏的共享图像！使用像推特的[卡片调试器](https://cards-dev.twitter.com/validator)和[脸书的开放图形调试器](https://developers.facebook.com/tools/debug/)这样的工具对于根据您的喜好调整它们并调试任何潜在的缺失标签是必不可少的。

# 包扎

这种方法的好处在于，如果您决定在未来某个时候更新或更改您的博客模板，它将更新所有博客的预览图像。它也节省了你在 Figma 或你选择的设计工具中为每个博客创建独特的预览图像的时间和麻烦。就这么定了，算了！

如果你已经走了这么远，干得好。我期待在不久的将来，在我的 feeds 上看到一些很棒的 Nuxt 内容博客，上面有漂亮的分享卡。感谢阅读！

*最初发布于 2020 年 10 月 18 日*[*https://David parks . dev*](https://davidparks.dev/blog/social-share-images-in-nuxt-content)*。*