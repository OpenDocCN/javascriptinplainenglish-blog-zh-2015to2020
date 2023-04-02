# 使用 Nuxt.js 的歌词查找应用程序

> 原文：<https://javascript.plainenglish.io/lyrics-finder-application-with-nuxt-js-5dec86ef7771?source=collection_archive---------14----------------------->

## 用 Nuxt.js 和 tailwind CSS 制作一个简单的歌词查找器应用程序

![](img/d043101d1a3011d2bc069eaa2dc710a8.png)

Photo by [Stas Knop](https://www.pexels.com/@stasknop?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/black-cassette-tape-on-top-of-red-and-yellow-surface-1626481/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在本文中，我们将使用 Nuxt.js 和 Tailwind CSS 制作一个简单的歌词查找器应用程序。

**先决条件**

*   安装 Node.js
*   Vue.js 的基础知识
*   Nuxt.js 的基础知识
*   顺风 CSS

如果你不熟悉 Nuxt.js 及其架构，请查看这个[博客](https://medium.com/javascript-in-plain-english/getting-started-with-nuxt-4652bc83ddc6)。

## **创建一个 Nuxt.js 项目**

要创建一个新的 Nuxt.js 项目，打开您的终端并根据您的首选包管理器运行下面的命令。

你也应该安装顺风 CSS，因为我们将使用它进行造型。

**NPM**

```
npm init nuxt-app lyrics-finder
```

**纱线**

```
yarn create nuxt-app lyrics-finder
```

这些命令将为我们创建一个新的 Nuxt.js 项目。
现在我们需要导航到 pages 目录和 ***index.vue*** 文件，我们将在这里编码我们的项目。
删除模板中的内容，用歌词查找应用程序的 h1 制作一个 div。对于这个项目，我们也将使用顺风 CSS 进行造型。

## 为我们的应用服务

为了提供我们的 Nuxt.js 项目应用程序文件，我们将在终端上运行下面的命令。

```
*npm run dev*
```

这将服务于我们的应用程序，并使其在 **localhost:3000 上可用。**

它也将热重新加载我们的应用程序，每次我们做出更改并保存，我们的更改将反映在浏览器中。完美！

现在我们需要创建一个表单输入部分，用户可以输入***歌曲名称* e** 和 ***艺术家姓名*** 进行歌词搜索。

## **表格输入部分**

我们的输入部分将包括两个输入字段和一个提交按钮。表单的代码如下所示。

```
<form @submit.prevent="searchLyrics" class="text-center pt-5"><input
v-model="artist"
type="text"
placeholder="Enter artist name"
class="pt-1"
/><input
v-model="title"
type="text"
placeholder="Enter song title"
class="pt-1"
/><button
type="submit"
class="bg-black text-white pr-4 pl-4 rounded pt-1 pb-1">Search</button></form>
```

我们在表单输入上使用 v-model 指令，这使得表单输入和应用程序数据状态之间的双向绑定变得轻而易举。

当用户在输入字段中输入数据时，它会自动将数据绑定到数据函数中的标题实例。

请参见下面的示例。

```
data() {
 return {
 // bind data
 artist: “chris brown”,
 title: “heat”,
 lyrics: [],
 };
 }
```

## **创建了**

创建实例后，将同步调用创建的生命周期挂钩。这将使创建的实例中的函数在每次创建应用程序实例时被触发。

同样，我们将利用的 API 端点不需要任何身份验证。

我们还将在创建的实例上使用 async await 来使我们的请求异步。

```
async created() {
 //Called synchronously after the instance is created

 }
```

在创建的生命周期挂钩上，我们将使用 fetch 发出 ***HTTPS*** 请求，如下所示。

```
async created() {*//Called synchronously after the instance is created**try* {
const response = *await* fetch(`https://api.lyrics.ovh/v1/${this.artist}/${this.title}`);const data = *await* response.json();
this.lyrics = data.lyrics;*// reset the data object to prevent from showing in the form input* this.artist = "";
this.title = "";} *catch* (error) {
console.log(error);}
}
```

## **进行搜索**

为了在用户点击提交时进行搜索，我们必须将 **@submit** 指令绑定到一个方法。

在表单上，我们已经用**锁住了*。阻止*到**的提交指令。这样做是为了防止每次单击提交按钮时表单冒泡并刷新页面。

```
<form [@submit](http://twitter.com/submit).prevent=”searchLyrics” class=”text-center pt-5">
```

我们有一个名为 ***searchLyrics*** 的方法，这是一个异步函数，它向 API 端点发出获取歌词的请求。

***searchLyrics*** 方法如下所示。

```
methods: {
 async searchLyrics() {
 // fetch lyrics from api here
 try { const response = await fetch(
 `[https://api.lyrics.ovh/v1/${this.artist}/${this.title}`](https://api.lyrics.ovh/v1/${this.artist}/${this.title}`)
 );
 const data = await response.json();//store the lyrics
this.lyrics=data.lyrics// reset the data object to prevent from showing in the form input
 this.artist = “”;
 this.title = “”; } catch (error) {
 console.log(error);
 }
 },
 }
```

为了将存储的歌词呈现到 DOM 中，我们需要使用双花括号调用数据实例中的歌词，如下所示。

```
<div class="pt-4 max-w-sm text-center m-auto"><p>{{ lyrics }}</p></div>
```

就这样，我们用 NuxtJs 做了一个简单的歌词查找应用程序。

这个项目的代码可以在[这里](https://github.com/developerphilo/Lyrics-Finder-App)找到。

**免责声明**:我们利用的 API 端点不提供某些歌曲的歌词，因此如果您搜索了您最喜欢的歌曲，但没有找到歌词，请不要惊慌。

## 结论

感谢您通读这篇文章。如果你觉得有帮助，请分享。

简单回顾一下，我们已经了解了如何:

*   使用 fetch 进行异步 API 调用
*   将表单输入绑定到数据实例
*   使用顺风类