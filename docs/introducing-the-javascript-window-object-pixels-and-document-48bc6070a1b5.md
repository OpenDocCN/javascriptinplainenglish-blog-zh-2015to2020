# 介绍 JavaScript 窗口对象—像素和文档

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-pixels-and-document-48bc6070a1b5?source=collection_archive---------4----------------------->

![](img/52b47cd701063e421cce4b34770323d8.png)

Photo by [Rob Wingate](https://unsplash.com/@robwingate?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。

在本文中，我们将继续查看`window`对象的更多属性，如`devicePixelRatio`属性，并开始查看`document`属性的属性。

# window . devicepixelrratio

我们可以使用`devicePixelRatio`属性来获得显示设备的物理像素分辨率与 CSS 像素分辨率的比值。

也就是说，一个 CSS 像素的大小与一个物理像素的大小的比率，或者应该使用多少实际像素来绘制一个 CSS 像素。这很方便，因为有各种各样的显示器。

像 4K 或视网膜显示器这样的高分辨率显示器需要缩放以显示内容，这样它们就不会太小。

因此，在屏幕上显示一个 CSS 像素需要不止一个像素，以便在这些不太小的屏幕上显示更清晰的图像。

这是一个只读属性，当值改变时，没有办法得到通知，如果用户将窗口拖动到像素密度不同于原始像素密度的不同显示器上，可能会发生这种情况。因此，如果您想要检查该属性的更改，您必须偶尔手动检查，例如使用`setInterval`功能。

我们可以通过编写以下代码来访问它:

```
console.log(window.devicePixelRatio)
```

通过运行上面的代码，我们应该可以看到用`console.log`语句记录的每个 CSS 像素的物理像素数。

# 窗口.文档

属性拥有当前打开文档的所有属性和方法。这意味着它将所有的 DOM 节点对象和所有相关的方法解析到树中。它还具有许多属性，用于监听事件并获取关于当前浏览器选项卡中打开的文档的各种信息。`document`对象有一个构造函数，即不带参数的`Document`构造函数，它创建一个新的`Document`对象。

## 窗口.文档.正文

`document`对象也有许多属性。`document.body`属性让我们得到当前文档的`body`或`frameset`节点以及其中的所有内容。例如，如果我们在运行`console.log`语句时将`document.body`作为参数传入，那么我们应该从`console.log`输出中得到类似如下的结果:

```
<body data-n-head="">
    <div id="__nuxt"><!----><div id="__layout"><div><nav class="navbar navbar-expand-lg navbar-light bg-light" data-v-a3aba82c=""><a href="/" class="navbar-brand nuxt-link-exact-active nuxt-link-active" data-v-a3aba82c="">John Au-Yeung's Portfolio</a> <button type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation" class="navbar-toggler" data-v-a3aba82c=""><span class="navbar-toggler-icon" data-v-a3aba82c=""></span></button> <div id="navbarSupportedContent" class="collapse navbar-collapse" data-v-a3aba82c=""><ul class="navbar-nav mr-auto" data-v-a3aba82c=""><li class="nav-item active" data-v-a3aba82c=""><a href="/" class="nav-link nuxt-link-exact-active nuxt-link-active" data-v-a3aba82c="">Home</a></li> <li class="nav-item" data-v-a3aba82c=""><a href="/portfolio" class="nav-link" data-v-a3aba82c="">Portfolio</a></li> <li class="nav-item" data-v-a3aba82c=""><a href="/resume" class="nav-link" data-v-a3aba82c="">Resume</a></li> <li class="nav-item" data-v-a3aba82c=""><a href="/whyhireme" class="nav-link" data-v-a3aba82c="">Why Hire Me</a></li></ul></div></nav> <div data-v-88e6fdf8=""><div id="home" data-v-88e6fdf8=""><div class="jumbotron jumbotron-fluid text-center" data-v-88e6fdf8=""></div> <div class="cont" data-v-88e6fdf8=""><div class="col-md-12" data-v-88e6fdf8=""><h1 data-v-88e6fdf8="">About John</h1> <p data-v-88e6fdf8="">Hire me to build a business web and mobile apps affordably.</p></div> <div class="col-md-12" data-v-88e6fdf8=""><p data-v-88e6fdf8="">My expertise include:</p> <ul data-v-88e6fdf8=""><li data-v-88e6fdf8="">Web app development</li> <li data-v-88e6fdf8="">Mobile app development</li></ul></div> <div class="col-md-12" data-v-88e6fdf8=""><h1 data-v-88e6fdf8="">Social Media</h1> <ul data-v-88e6fdf8=""><li data-v-88e6fdf8=""><a href="[https://medium.com/@hohanga](https://medium.com/@hohanga)" target="_blank" data-v-88e6fdf8=""><i class="fab fa-medium" data-v-88e6fdf8="" aria-hidden="true"></i> Medium Blog
            </a></li> <li data-v-88e6fdf8=""><a href="[https://www.linkedin.com/in/hohanga](https://www.linkedin.com/in/hohanga)" target="_blank" data-v-88e6fdf8=""><i class="fab fa-linkedin" data-v-88e6fdf8="" aria-hidden="true"></i>
              LinkedIn
            </a></li> <li data-v-88e6fdf8=""><a href="[https://twitter.com/AuMayeung](https://twitter.com/AuMayeung)" target="_blank" data-v-88e6fdf8=""><i class="fab fa-twitter" data-v-88e6fdf8="" aria-hidden="true"></i>
              Twitter
            </a></li></ul></div> <div class="col-md-12" data-v-88e6fdf8=""><h1 data-v-88e6fdf8="">Contact</h1> <ul data-v-88e6fdf8=""><li data-v-88e6fdf8=""><a href="[mayeung@telus.net](mailto:mayeung@telus.net)" data-v-88e6fdf8=""><i class="fas fa-envelope-square" data-v-88e6fdf8="" aria-hidden="true"></i>
              Email
            </a></li></ul></div> <div class="col-md-12" data-v-88e6fdf8=""><h1 data-v-88e6fdf8="">Git Repositories</h1> <ul data-v-88e6fdf8=""><li data-v-88e6fdf8=""><a href="[https://bitbucket.org/hauyeung/](https://bitbucket.org/hauyeung/)" target="_blank" data-v-88e6fdf8=""><i class="fab fa-bitbucket" data-v-88e6fdf8="" aria-hidden="true"></i>
              Bitbucket
            </a></li></ul></div></div></div> <footer class="text-center" data-v-88e6fdf8=""><i class="far fa-copyright" data-v-88e6fdf8="" aria-hidden="true"></i>
    2019 John Au-Yeung's Portfolio.
    Icons Provided by Font Awesome.
  </footer></div></div></div></div><script>window.__NUXT__={layout:"default",data:[{}],error:null,state:{},serverRendered:!0}</script><script src="/_nuxt/aa95a776e06b06b1de50.js" defer=""></script><script src="/_nuxt/278a2a8cea1aa4220b7f.js" defer=""></script><script src="/_nuxt/49479c32fdfc8cc26830.js" defer=""></script><script src="/_nuxt/a006571538a8da07f921.js" defer=""></script><script src="/_nuxt/92eb89f167a526d23bc9.js" defer=""></script></body>
```

输出有`body`节点和它里面的所有 DOM 节点。我们也可以设置`body`属性。然而，如果我们这样做，我们会用你设置的覆盖体内的一切。为此，我们可以编写如下代码:

```
const body = document.createElement('body');
const hello = document.createElement('p');
hello.innerHTML = 'hello';
body.append(hello);
document.body = body;
```

在上面的代码中，我们首先创建`body`元素。这是必需的步骤，因为我们只能用`body`或`frameset`元素设置`document.body`。否则，我们将得到一个错误，说明您没有用这些元素设置`document.body`。随着`body`元素的创建，我们可以在里面创建任何我们想要的东西，并将其附加到身体上。在上例中，我们创建了一个`p`元素，然后将其`innerHTML`属性设置为`'hello'`。之后，我们将创建的`p`元素附加到`body`上，然后将其分配给`document.body`属性。之后，我们应该在屏幕上看到“你好”这个词。

![](img/dd73ee978265dd665068ee3954b47563.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## window.document.characterSet

`characterSet`属性是一个只读属性，它返回当前呈现的文档的字符编码。

字符编码是一组字符，以及如何将组成网页的字节解释为这些字符。字符编码可以在`Content-Type`标题中被覆盖，或者放入`meta`标签中。

例如，我们可以用`<meta charset=”utf-8">`来设置要用`utf-8`编码呈现的网页。也可以使用浏览器选项来覆盖它，比如通过右键单击 Internet Explorer 浏览器窗口来设置字符编码，例如，单击“编码”，然后选择要手动呈现页面的字符编码。

例如，我们可以通过书写来查看您正在查看的页面所使用的字符编码:

```
const charSet = document.characterSet;
console.log(charSet);
```

我们应该从`console.log`输出中得到类似于`‘UTF-8’`的结果。

在本文中，我们研究了`document`属性和`devicePixelRatio`属性。

`devicePixelRatio`属性让我们看到渲染一个 CSS 像素的物理像素的数量，而`document`属性是一个具有许多属性的对象，这些属性让我们获得关于当前打开的文档的信息，并允许我们给它附加事件处理程序并操纵它。

我们使用`document.body`属性将`body`元素设置为我们选择的内容，并使用相同的属性查看其属性。

此外，我们用`document.characterSet`属性获得了我们正在查看的网页的字符集。

在`document`对象中有更多的属性，所以请继续关注本系列的下一部分，看看`window`和`window.document`对象的更多属性。