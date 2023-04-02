# 在 Nuxt.js 中制作滚动指示器

> 原文：<https://javascript.plainenglish.io/make-a-scroll-indicator-in-nuxt-js-cb84e3a7cfc4?source=collection_archive---------9----------------------->

## 滚动指示符可以在博客中实现，以便用户知道他在页面内已经滚动到哪里

![](img/8b3508763615b7c07ad25844f69d94a9.png)

Photo by [ready made](https://www.pexels.com/@readymade?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/fresh-salad-served-on-black-plate-with-fork-and-knife-on-white-napkin-3850924/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在本文中，我们将在 Nuxt.js 中制作一个滚动指示器，如果您不熟悉 Nuxt.js 及其体系结构，请考虑一下这篇 [**文章。**](https://medium.com/javascript-in-plain-english/getting-started-with-nuxt-4652bc83ddc6)

滚动指示器可以在博客中实现，以便用户知道他在页面中滚动到了哪里。实现滚动指示器的一些优点是:

*   它为网页提供了一个美丽的外观，尤其是应用于博客文章时。
*   用户可以很容易地检查他/她的阅读进度。

我们将使用一个称为 **vue 滚动指示器**的 npm 包和插件。
要使用这个 npm 插件，我们首先需要将它安装到我们的应用程序中。

要安装 **vue 滚动指示器**，请打开您的终端并运行以下命令。

```
npm install vue-scroll-indicator
```

这个命令将把这个插件安装到我们的应用程序中。

## **注册插件**

要注册插件并使其在我们的应用程序中可用，我们需要导航到位于您的 Nuxt 应用程序结构根的插件文件夹，并创建一个名为***vue-roll-indicator . js***的文件。

您与此特定名称无关。你可以给它取任何你喜欢的名字。

之后，我们需要打开位于应用程序结构根的***nuxt . config . js***文件，注册我们的插件。

如果您想知道我们为什么这样做，这就是原因。

对于服务器端呈现的 Nuxt.js 应用程序，该插件将作为未定义的窗口错误抛出。这是因为插件被配置为只在客户端运行，而不是在服务器上。

为了防止服务器端呈现的 Nuxt 应用程序出现这种错误，我们需要告诉 Nuxt 不要在服务器上运行这个插件，而只在客户端运行它。

我们将 **SSR** 设为**的原因是假的**。

前往插件并注册 vue-roll-指示器，如下所示。

```
plugins: [
 { src: ‘~/plugins/vue-scroll-indicator’, ssr: false }
 ],```
```

## **将其渲染到我们的应用组件中**

为了将插件渲染到我们的组件中，我们不需要像其他组件一样导入它。我们仅将其包含如下。

```
<! — vue-scroll-indicator →><vue-scroll-indicator></vue-scroll-indicator>
```

## **打造我们的 vue-scroll 指示器**

这个包的另一个漂亮的特点是，我们可以设计滚动指示器的样式。

我们可以设置不同的背景颜色来匹配我们的网站，并增加滚动条指示器的高度。

我们如何应用样式？

```
 <! — vue-scroll-indicator →<vue-scroll-indicator height=”4px”
 color=”#32d9cb”
 background=”none”
>
</vue-scroll-indicator> 
```

**高度:**这里我们指定滚动指示器的高度。
**颜色:**这里我们提供了我们首选颜色的十六进制值。这将是 vue 滚动指示器的颜色。

## **结论**

要在服务器端呈现的 Nuxt 应用程序中使用插件，需要将 **SSR** 设置为 false。这将阻止插件在服务器中呈现。

嗯，这是一个很酷的功能，你可以添加到你的应用程序中。要了解更多关于如何使用这个插件的信息，请访问这个[链接。](https://www.npmjs.com/package/vue-scroll-indicator)

如果你觉得这篇文章有趣，请不要犹豫分享。