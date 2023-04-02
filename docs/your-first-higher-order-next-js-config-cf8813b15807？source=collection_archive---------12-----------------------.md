# 您的第一个高阶 Next.js 配置:为您的 Next.js 应用程序生成一个站点地图

> 原文：<https://javascript.plainenglish.io/your-first-higher-order-next-js-config-cf8813b15807?source=collection_archive---------12----------------------->

![](img/b5738cc38717c5d102dc9e3bd515a061.png)

我惊讶地意识到 [Next.js](https://nextjs.org/) 没有现成的工具来生成 *sitemap.xml* 。在我的研究过程中，我偶然发现了一个[库](https://github.com/IlusionDev/nextjs-sitemap-generator)，但是我对它们提供的接口没有什么印象。所以，我决定亲自动手，设计一个更高阶的 Next.js 配置。让我们来讨论一下，这样当您需要时，您也可以创建一个更高阶的配置。

**TL；DR —** 访问**[https://github.com/cansin/next-with-sitemap](https://github.com/cansin/next-with-sitemap)，开始使用 npm 包，如果你只是想为你的应用程序生成一个站点地图，或者继续阅读了解更多。**

## **高阶 Next.js 配置的剖析**

**所以首先要做的是。您可能想知道为什么我们需要高阶配置？究竟什么是高阶配置？高阶配置是对[高阶功能](https://en.wikipedia.org/wiki/Higher-order_function)概念的文字游戏。因此，它是一个将一个配置作为参数并返回一个配置作为结果的函数。**

> **高阶配置是一个函数，它将一个配置作为其参数，并返回一个新的配置作为其结果。**

**仅仅通过在`next.config.js`内部编写我们需要的代码，就有可能实现我们在这里要做的事情。但是根据一般经验，随着项目的增长，将某些方面封装成一段代码更易于管理。因此，希望将其作为高阶配置。**

**正如我们所说的，我们需要从创建一个函数开始，该函数将在`next.config.js`获取现有的 Next.js 配置，并生成一个能够生成站点地图的增强版本。因此，在您的项目下创建一个`config/withSitemap.js`文件，并添加以下代码:**

```
// at config/withSitemap.js**function** *withSitemap*(nextConfig = {}) {
  **return** {
    ...nextConfig,
    webpack(webpackConfig, options) {
      **let** config = webpackConfig;
      **if** (**typeof** nextConfig.**webpack** === **"function"**) {
        config = nextConfig.**webpack**(config, options);
      } // Do something with the config **return** config;
    },
  };
}***module***.**exports** = *withSitemap*;
```

**这个骨架做了两件事。由于`...nextConfig`的扩展，它将下一个配置中未被触及的部分传递下去。它还运行任何现有的 webpack 配置，这要感谢在`nextConfig.webpack`上的条件运行。我们为什么要为 webpack 费心？因为我们实际上正要扩充我们的 webpack 配置来生成站点地图文件。但稍后会详细介绍。**

## **带有 itemap 的选项应该有**

**大多数现代的 Next.js 应用程序都有一个存放公共静态内容的`public`文件夹，以及一个存放每个页面的 JavaScript 文件的`pages`文件夹。我们还需要知道所服务的应用程序的真实领域，以便 *sitemap.xml* 文件能够正确地表示它。又多了一个进入的选择。在那里，我们还可以添加一个选项来禁用 *sitemap.xml* 生成，以及另一个选项来禁用 *robots.txt* 生成。**

**我们希望在 Next.js 配置中的`sitemap`对象中接收这些选项。这样`next.config.js`就可以是:**

```
// at next.config.js (final code)const withSitemap = require("./config/withSitemap");module.exports = withSitemap({
  sitemap: {
    domain: "https://www.example.com",
    destFolder: "public",
    pagesFolder: "pages",
    generateRobots: true,
    generateSitemap: true,
  },
  // .
  // ..
  // ... other Next.js config
});
```

**看起来很干净，不是吗？好了，现在我们知道了要返回给 Next.js 配置的接口。让我们继续讨论代码的实质内容。**

## **创建自定义 Webpack 插件**

**为了生成站点地图文件，我们将把一个 webpack 插件挂接到我们的应用程序上，它将从给定的选项和文件系统中读取必要的数据，并发出`sitemap.xml`和`robots.txt`。Webpack 本身有关于[编写插件](https://webpack.js.org/contribute/writing-a-plugin/)的优秀文档。我们将遵循那里描述的最佳实践。让我们继续在`config/webpack/SitemapPlugin.js`下创建一个名为`SitemapPlugin`的 webpack 插件，并将其填写为:**

```
// at config/webpack/SitemapPlugin.js**const** validateOptions = ***require***(**"schema-utils"**);**const** schema = {
  **type**: **"object"**,
  **properties**: {
    **domain**: {
      **type**: **"string"**,
    },
    **destPath**: {
      **type**: **"string"**,
    },
    **pagesPath**: {
      **type**: **"string"**,
    },
    **generateRobots**: {
      **type**: **"boolean"**,
    },
    **generateSitemap**: {
      **type**: **"boolean"**,
    },
  },
};**class** SitemapPlugin {
  constructor(options) {
    validateOptions(schema, options, **"SitemapPlugin"**);
    **this**.**options** = options;
  } apply(compiler) {
    // This is where the magic happens
  }
}***module***.**exports** = SitemapPlugin;
```

***模式*将帮助我们验证插件的给定选项。正如你可能猜到的，我们将把给高阶配置的选项传递给 webpack 插件。**

## **使用 withSitemap 中的 SitemapPlugin**

**下一步是用项目地图函数从*中调用*网站地图插件*。这很简单，因为:***

```
// at config/withSitemap.js (final code)**const** path = ***require***(**"path"**);**const** SitemapPlugin = ***require***(**"./webpack/SitemapPlugin"**);**function** *withSitemap*(nextConfig = {}) {
  **return** {
    ...nextConfig,
    webpack(webpackConfig, options) {
      **let** config = webpackConfig;
      **if** (**typeof** nextConfig.**webpack** === **"function"**) {
        config = nextConfig.**webpack**(config, options);
      }

      // Hooking the Sitemap Plugin - start
      **const** {
        isServer,
        config: {
          pageExtensions,
          sitemap: { 
            destFolder = **"public"**, 
            pagesFolder = **"pages"**, 
            ...sitemapOptions 
          } = {},
        },
      } = options; **if** (isServer) {
        **return** config;
      } **const** destPath = path.join(options.**dir**, destFolder);
      **const** pagesPath = path.join(options.**dir**, pagesFolder); ***console***.log(**"> Generating sitemap.xml and robots.txt"**);
      ***console***.log(**`> Pages path: "**${pagesPath}**"`**);
      ***console***.log(**`> Destination path: "**${destPath}**"`**); config.**plugins**.push(
        **new** SitemapPlugin({
          destPath,
          pagesPath,
          pageExtensions,
          ...sitemapOptions,
        })
      );
      // Hooking the Sitemap Plugin - end**return** config;
    },
  };
}***module***.**exports** = *withSitemap*;
```

**我们正在做一些事情。首先，我们使用析构语法为我们期望的选项提供一些合理的默认值。正如我们所讨论的，大多数 Next.js 应用程序将使用`public`作为默认的*公共*文件夹，使用`pages`作为默认的*页面*文件夹。然后我们通过一个*路径计算这些文件夹的实际路径。将*连接到由 *options.dir* 指定的根目录。我们也将`pageExtensions` Next.js 配置传递给我们的插件。这是一个鲜为人知的配置，允许开发人员定义哪些文件扩展名将被视为 pages 文件夹下的页面文件(例如，`.page.js`而不是`.js`)。**

## **实现实际的站点地图生成🎉**

**我们最终把所有东西都捆绑到了我们的 webpack 插件上。现在是生成实际文件的时候了。我们将依靠几个外部库来完成这项工作。让我们添加它们:**

```
$ yarn add x2js xml-formatter glob schema-utils clean-webpack-plugin
```

**现在库已经安装好了，让我们把代码放到插件中，讨论一下:**

```
// at config/webpack/SitemapPlugin.js (final code)**const** fs = ***require***(**"fs"**);
**const** path = ***require***(**"path"**);**const** { CleanWebpackPlugin } = ***require***(**"clean-webpack-plugin"**);
**const** validateOptions = ***require***(**"schema-utils"**);
**const** X2JS = ***require***(**"x2js"**);
**const** *format* = ***require***(**"xml-formatter"**);
**const** glob = ***require***(**"glob"**);**const** schema = {
  **type**: **"object"**,
  **properties**: {
    **domain**: {
      **type**: **"string"**,
    },
    **destPath**: {
      **type**: **"string"**,
    },
    **pagesPath**: {
      **type**: **"string"**,
    },
    **pagesExtension**: {
      **type**: **"array"**,
    },
    **generateRobots**: {
      **type**: **"boolean"**,
    },
    **generateSitemap**: {
      **type**: **"boolean"**,
    },
    **robotsFilename**: {
      **type**: **"string"**,
    },
    **sitemapFilename**: {
      **type**: **"string"**,
    },
  },
};**class** SitemapPlugin {
  constructor(options) {
    validateOptions(schema, options, **"SitemapPlugin"**);
    **this**.**options** = options;
  } apply(compiler) {
    **const** {
      domain,
      destPath,
      pagesPath,
      pageExtensions,
      generateRobots = **true**,
      generateSitemap = **true**,
      robotsFilename = **"robots.txt"**,
      sitemapFilename = **"sitemap.xml"**,
    } = **this**.**options**;
    **const** robotsDest = path.join(destPath, robotsFilename);
    **const** sitemapDest = path.join(destPath, sitemapFilename); **new** CleanWebpackPlugin({
      **cleanOnceBeforeBuildPatterns**: [robotsDest, sitemapDest],
    }).apply(compiler);compiler.**hooks**.**done**.**tap**(**"SitemapPlugin"**, () => {
      **const** x2js = **new** X2JS();
      **const** pages = glob
        .*sync*(**`**/*.+(**${pageExtensions.join(**","**)}**)`**, { **cwd**: pagesPath })
        .map((page) =>
          page.replace(
            **new *RegExp***(
              **`(.*?)(index)?\\.(**${pageExtensions
                .map((ext) => ext.replace(**"."**, **"\\."**))
                .join(**"|"**)}**)`** ),
            **"$1"** )
        )
        .filter((page) => !page.startsWith(**"_"**));**const** date = **new *Date***().toISOString().slice(0, 10);
      **const** sitemapObj = {
        **urlset**: {
          **_xmlns**: **"https://www.sitemaps.org/schemas/sitemap/0.9/"**,
          **url**: [],
        },
      }; pages.forEach((page) => {
        sitemapObj.**urlset**.**url**.push({
          **loc**: **`**${domain}**/**${page}**`**,
          **lastmod**: date,
        });
      }); **if** (robots) {
        **const** robotsTxt = **`User-agent: *\nAllow: /\nSitemap:** ${domain}**/**${sitemapFilename}**`**;
        fs.*writeFileSync*(robotsDest, robotsTxt, {
          **flag**: **"as"**,
        });
      } **if** (sitemap) {
        **const** sitemapXml = *format*(
          **`<?xml version="1.0" encoding="UTF-8"?>**${x2js.js2xml(sitemapObj)}**`**,
          {
            **collapseContent**: **true**,
          }
        );
        fs.*writeFileSync*(sitemapDest, sitemapXml, {
          **flag**: **"as"**,
        });
      }
    });
  }
}***module***.**exports** = SitemapPlugin;
```

**好的，首先，我们使用 CleanWebpackPlugin 为 *sitemap.xml* 和 *robots.txt* 清除任何现有文件(可能来自以前的构建)。然后我们创建一个钩子，一旦编译器做了其他的事情，它就会运行。在这个钩子的处理程序中，我们使用了一个 [glob 模式](https://en.wikipedia.org/wiki/Glob_(programming))来遍历 *pagesPath 中的每个页面文件名。*我们利用模式中的*页面扩展*来确保我们不会遍历不应该被认为是页面文件的文件名。然后，我们执行一个正则表达式替换，后面跟着一个过滤器，以获得这些页面文件将映射到的 URL 的干净列表(例如，`pages/auth/login.js`变成了`auth/login`)。**

> **恭喜，您刚刚创建了一个更高阶的 Next.js 配置，它为您的页面生成 sitemap.xml。**

**一旦我们知道将什么路径放入 *sitemap.xml* 中，下一步实际上就是生成 xml。为此，我们依赖这个叫做 [x2js](https://github.com/x2js/x2js#readme) 的酷库。这个库使我们能够将普通的 JavaScript 对象转换成 XML 字符串。使用`domain`选项和我们刚刚计算的相对页面 URL，我们开始创建我们的 XML 对象，其中根是`<urlset>`、*、*，每个 URL 都有一个`<url>`条目。最后，我们依靠`fs`将我们生成的内容放入 *sitemap.xml* 和 *robots.txt.***

## **让我们看看它的实际效果。**

**现在，如果您继续运行`yarn next build`，您应该会看到`sitemap.xml`和`robots.txt`都在您的`public`文件夹中生成，它们将由 Next.js 在您的根域中提供服务。git 忽略这两个文件，这样它们就不会提交到您的版本控制中。**

## **潜在的改进**

**人们可以将更多的信息放到 *sitemap.xml* 中，比如 *changefreq* 和 *priority* (以及 *robots.txt* ) *。我们可以简单地在 Next.js 配置中添加更多选项来捕获这些数据，并将其传递给我们的插件。我们目前也不处理任何动态 URL(例如`pages/user/[id].js`)。这将是一个受欢迎的补充。***

**听起来是个不错的主意？也许在 https://github.com/cansin/next-with-sitemap[创建一个拉请求(无耻的插头🤓).](https://github.com/cansin/next-with-sitemap)**

## **结论**

**一旦你学会了这种通用模式来实现自我封装的 Next.js 配置改进，管理你的配置将变得毫不费力，也许还能得到你希望存在的库。我希望这是对你当前/下一个项目有价值的信息。**