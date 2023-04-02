# 使用新技术构建我们自己的博客:AlpineJS

> 原文：<https://javascript.plainenglish.io/build-your-own-blog-with-alpinejs-757c81af6670?source=collection_archive---------4----------------------->

## 在这个由两部分组成的故事中，我们将使用两种现代技术开发我们自己的博客:Deno 用于后端，AlpineJS 用于前端。我们继续前端。

![](img/abdb0d0d2f26af157f74aff88ee6e3cf.png)

Photo by [Alex Siale](https://unsplash.com/@alexsiale?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在[之前的文章](https://medium.com/javascript-in-plain-english/build-your-own-blog-with-deno-62c9909c69ce)中，我们使用 Deno 开发了自己博客的后端。现在我们已经写了一些帖子，并创建了一个为他们服务的后端，让我们开发前端，将他们展示给世界其他地方。对于前端，我们将使用精益框架 [AlpineJS](https://github.com/alpinejs/alpine) 。

如果你想跳过理论直接跳到结果，可以在这里找到代码[，在这里](https://github.com/omirobarcelo/blog-frontend-alpinejs)看完博客[。](https://omirobarcelo.github.io/blog/)

# 阿尔卑斯山

AlpineJS 是在 HTML 模板中编写 JavaScript 行为的最小前端框架。正如作者在他们的 GitHub 中所说:

> 就像 JavaScript 的 [Tailwind](https://tailwindcss.com/) 一样。

AlpineJS 提供了一个轻量级的简单库来增加我们前端的反应性。我认为它对简单的水疗和原型制作特别有用。你可以把它添加到你的`index.html`上，然后开始编码。

因为我们的博客只有两个视图，一个是所有文章的列表，另一个是选中的文章，所以我们将直接编程，不需要任何传输。

# 布局

对于基本的布局，我们决定有一个标题与我们的标志，一个链接到文章列表，和一个黑暗模式的切换。然后是包含列表视图或文章视图的正文。在底部的页脚有版权和社会链接。

让我们从添加外部样式表和脚本开始。

我们将使用 [Tailwind CSS](https://tailwindcss.com/) 进行大部分样式设计，使用 [Pretty Checkbox](https://lokesh-coder.github.io/pretty-checkbox/) 作为 CSS 专用开关，使用 [Font Awesome](https://fontawesome.com/) 作为图标，使用 AlpineJS 作为框架。在这种情况下，我们下载了漂亮的复选框和字体牛逼本地服务他们更快更可靠的加载。

AlpineJS 处理有作用域的数据。对于一个块，您声明了一组数据和函数，其中的元素可以访问它们。

在模板的`body`中，我们用`x-data`定义了作用域数据，用`x-init`定义了后 DOM 初始化代码。

作用域数据`state`返回包含我们将在整个模板中使用的元素和函数的对象。

`darkMode`是一个布尔标志，用于标记用户是否切换了黑暗模式。

`posts`是一个保存已加载帖子列表的数组。

`entry`指示是否加载了特定的文章，其 HTML 格式的数据，以及上一篇和下一篇 id，以跳转到上一篇或下一篇文章。

`loadPosts`是从我们的后端加载帖子列表的初始化函数。

`loadEntry`改变光标，表示我们正在加载文章，从后端获取 HTML 格式的文章，并计算上一篇和下一篇文章的 id。加载数据后，我们滚动到顶部，并将光标改回来。

定义好数据和函数后，我们就可以编写标记了。在顶部我们有标题。

为了切换黑暗模式，AlpineJS 提供了类绑定。绑定一个对象，其中每个属性是一个布尔值，指示该类是否被应用。在我们的例子中，当`darkMode`被切换时，我们使用对象`{ 'dark': darkMode }`来应用`dark`类。

`x-bind:`允许简写`:`，所以`x-bind:class`可以写成`:class`。

在`img`中我们有自己的 logo，可以绑定到它的`src`属性。我们甚至可以根据用户选择的模式使用字符串模板来更改我们的徽标。

为了将`darkMode`双向数据绑定到输入开关，我们使用 AlpineJS 的`x-model`。每当我们切换输入时，`darkMode`的值就会相应地更新，将`dark`类添加到任何需要的地方，比如在头中。

页脚很简单，我们只使用一个 AlpineJS 指令来添加黑暗模式的`dark`类。

# 列表视图

在我们的文章列表视图中，我们不仅要显示列表本身，而且要从封面开始，以表明这个博客是什么。我们这样做是因为列表视图也是初始视图。

`background div`添加一个固定的背景封面图片。

`x-if`是另一个 AlpineJS 指令，用于显示或删除——而不仅仅是隐藏——DOM 元素。虽然没有加载的条目，我们显示我们的封面，这可以显示我们希望的任何内容，在我的情况下，我是谁。

如果用户想跳过我们漂亮的封面，我们添加一个带有 ID 的`div`从我们的标题链接跳转。

为了显示共享标记的多个元素，我们可以使用 AlpineJS 的 for，`x-for`。`x-for`遍历我们的`posts`数组，并使用帖子的 ID 作为键。这个键提示 AlpineJS 元素的身份，并可以使用它来决定排序和重新呈现。

对于`x-if`和`x-for`来说，它们内部只需要一个根元素。如果我们想嵌套它们，我们需要中间元素。

我们使用 TailwindCSS 的卡实现。每张卡片代表一个条目，它显示封面图像、标题和标签列表。我们可以点击卡片查看帖子。

对于事件的处理，AlpineJS 提供了`x-on`指令，例如:`x-on:click`。我们可以使用`@`简写，在我们的例子中，当我们点击我们有`@click="loadEntry(post.id)"`的卡片时，加载文章。

为了显示标签，我们使用`x-text`。该属性将我们选择的数据绑定到 DOM 元素的`innerText`属性。

# 发布视图

帖子视图将从我们的后端下载 HTML，并用我们自己的风格显示它。

当帖子被加载时，我们将显示按钮，以转到上一篇或下一篇帖子，然后回家。如果没有上一篇或下一篇文章，我们可以绑定到`disabled`属性并禁用按钮。当我们将一个处理程序绑定到一个事件时，我们不需要从我们的作用域中附加一个函数。在我们的例子中，为了回家，我们指示没有已加载的帖子。

为了显示 HTML 数据，我们使用 AlpinJS 的'`x-html`指令。该指令绑定到 DOM 元素的`innerHTML`属性。我们需要小心我们绑定的对象，以避免 XSS 的攻击。

# 式样

我们可以为我们的博客应用任何我们想要的风格。在我们的例子中，我们主要使用 TailwindCSS 类。对于我们自己定义的样式，有一个有趣的技巧来改善初始加载时间。

如果我们把所有的样式都放在`index.html`中，文件也会随之增长，用户需要等到文件完全下载后才能开始查看我们的博客。如果我们将所有的样式放到一个外部文件`index.css`中，我们会得到一个类似的结果，因为浏览器会等到所有的`link`元素都被加载后才开始渲染。

我们定义了混合方法。我们将关键的样式，即那些立即应用的样式，放在`index.html`中，并将其余的样式加载到一个外部文件中，将`media`属性更改为`print`。这将使浏览器在已经呈现页面的同时并行下载文件。为了使下载的样式适用于所有内容，而不仅仅是打印的页面，我们添加了`onload="this.media='all'".`

如果你想了解更多关于这一招的内容，可以查看[弗农的文章](https://blog.logrocket.com/improve-site-performance-inlining-css/)。

# 部署

由于这是一个没有任何传输文件的单页面应用程序，我们可以使用 GitHub 页面，给出`index.html`和所有需要的文件和资产。

感谢您的阅读！现在我们完成了博客的前端和后端。我们在这两方面都使用了新技术！如果我有什么错误或者不清楚的地方，请留下评论，我会尽快回答。

# 资源

[](https://github.com/alpinejs/alpine) [## 阿尔卑斯山/阿尔卑斯山

### 一个坚固的、最小的框架，用于在您的标记中组成 JavaScript 行为。-阿尔卑斯山/阿尔卑斯山

github.com](https://github.com/alpinejs/alpine) [](https://tailwindcss.com/) [## tailwind CSS——一个实用的 CSS 框架，用于快速构建定制设计

### 它们带有各种预先设计的组件，如按钮、卡片和警报，可以帮助您快速移动…

tailwindcss.com](https://tailwindcss.com/)  [## 漂亮-复选框. css

### 安装> yarn add pretty-checkbox//或者> npm install pretty-checkbox 或者，你也可以使用 CDN link…

lokesh-coder.github.io](https://lokesh-coder.github.io/pretty-checkbox/) [](https://fontawesome.com/) [## 字体真棒

### 世界上最受欢迎和最容易使用的图标集刚刚得到了升级。更多图标。更多款式。更多选择。

fontawesome.com](https://fontawesome.com/) [](https://blog.logrocket.com/improve-site-performance-inlining-css/) [## 通过内嵌你的 CSS - LogRocket 博客来提高网站性能

### 当你第一次着手建立一个网站或应用程序时，你的 index.html 文件就像你将永远…

blog.logrocket.com](https://blog.logrocket.com/improve-site-performance-inlining-css/)