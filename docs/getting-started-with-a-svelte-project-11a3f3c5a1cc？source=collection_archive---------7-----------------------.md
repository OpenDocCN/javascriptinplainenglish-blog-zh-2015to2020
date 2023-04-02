# 开始一个苗条的项目

> 原文：<https://javascript.plainenglish.io/getting-started-with-a-svelte-project-11a3f3c5a1cc?source=collection_archive---------7----------------------->

![](img/a947a7a2e65a7907d745ac336fc2c740.png)

对于初学者，在 **SvelteJS** 资源库中有一个模板项目。您可以通过运行下面的命令来克隆此项目。

```
**npx degit** [**sveltejs/template**](https://github.com/sveltejs/template) **template-master**
```

![](img/fd7d0df3ee84348592d3c0c315a4a180.png)

在克隆了模板项目之后，您可以很容易地从 ***localhost:5000*** 端口发布项目。

```
**cd template-master****npm install
npm run dev**
```

![](img/2a379cf6fea60fba582315df7ff0683f.png)

模板项目结构如图所示。

![](img/c001c5e2897b46b5083483e358970878.png)

如果你安装了 SvelteJS 插件，在 Visual Studio 代码上写代码的时候会有帮助。

![](img/3f1fe3efd13e347373ccf6b92659eb10.png)

苗条的文件由三个主要部分组成。这些是可以编写操作的脚本部分，可以定义 html 元素的主要部分，以及用于修饰元素的样式部分。

![](img/ad262931c38f96ab024bf380298ed5cd.png)

您可以在脚本部分中定义要在主部分中使用的变量，如下所示。

![](img/82d84e62e55e0d4af56da9d6ddfd6d63.png)![](img/9affbe8fbaf25db34566d0aab03d6c2f.png)

SvelteJS 中嵌套组件的使用如图所示。首先，应该编写子组件。

![](img/6351264f7faba373389a0e4f43676176.png)

我们将把这里创建的 Nested.svelte 组件集成到 App.svelte 主组件中，如图所示。

![](img/6f1e67ff715f4b41e9aaf7a6a23467df.png)

导入的元件将成为可用的元素。

![](img/c8d145340bd62221cea3c4a0b8939b98.png)

定义一个按钮，处理 click 事件，如图所示。

![](img/f1d1050101c6e15ad8a53cf15bcc6d13.png)![](img/437552a907a23712f436efa861cb19e6.png)

SvelteJS 框架能够使用大多数 Javascript 操作。你可以通过下面的链接访问 SvelteJS 教程，并使用 SvelteJS 框架的特性。

[](https://svelte.dev/tutorial/basics) [## 苗条教程

### 欢迎来到苗条教程。这将教会你构建快速、小型 web 应用程序所需的一切…

苗条的人](https://svelte.dev/tutorial/basics) 

希望在新的文章中看到你…

和平结束了，

穆赫塔利普·德德