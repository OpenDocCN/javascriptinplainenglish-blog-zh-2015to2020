# 如何在 Heroku 部署的应用上实施 HTTPS，即 React

> 原文：<https://javascript.plainenglish.io/how-to-enforce-https-on-heroku-deployed-apps-i-e-react-6dd30f9fc4c?source=collection_archive---------8----------------------->

## 普通的 JavaScript 加上一些 React 魔法解决了这个问题！

![](img/3230eeb38441e21a4aaf3119651cd08a.png)

Photo [V School](https://coursework.vschool.io/deploying-mern-with-heroku/)

这个问题困扰了我一段时间，我不喜欢我在网上找到的任何解决方案，然后我的脑袋灵光一现！

React now 为我们提供了 React Hooks，现代 web 开发的第七大世界奇迹。这如何解决我们的问题？

我们可以编写一个自定义的钩子，它会在我们每次渲染页面时触发。假设你已经运行了你的 React 应用程序，继续为你的定制钩子创建一个文件夹和一个名为 **useRedirectToHttps.js** 的文件

在 **useRedirectToHttps.js** 中，我们将创建一个名为`useRedirectToHttps`的钩子，并在每次渲染时调用一个`useEffect()`。useEffect 的工作非常简单，获取当前的 URL，检查它是否是 HTTPS，然后重定向到带有 HTTPS 的页面

在任何 React 组件中，您都可以调用:

```
useRedirectToHttps() 
```

瞧啊。你的应用程序每次都会将用户重定向到 HTTPS 网站🎉

PS。关于我们在这里使用的 JavaScript 属性，有几点需要注意

**window . location . protocol**—返回您当前用来访问网站的协议的字符串表示形式— **注意，它也有一个冒号，而不仅仅是协议。**

**window . location . replace(" https://…"**—将用户重定向到 https，并通过从浏览器历史记录中删除条目来消除用户返回 HTTP 的可能性。

**location.pathname** —是一个 react-router 的好东西，它允许我们检查我们此刻在哪里，或者，如果你不使用 react-router，你也可以解析从 window.location.href 返回的字符串。