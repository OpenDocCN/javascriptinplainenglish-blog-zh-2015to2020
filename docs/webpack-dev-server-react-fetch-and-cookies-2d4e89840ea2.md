# Webpack-dev-server、React、Fetch 和 Cookies

> 原文：<https://javascript.plainenglish.io/webpack-dev-server-react-fetch-and-cookies-2d4e89840ea2?source=collection_archive---------2----------------------->

![](img/ab0495dc5e2c308fa6e2f2668db0e943.png)

Photo by [Guilherme Vasconcelos](https://unsplash.com/@gui_vasconcelos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天很有趣。在开发 React 应用程序时，我使用 webpack-dev-server 来提供服务。我正在实现 JavaScript Web 令牌，通过一个 httpOnly cookie 通过一个`fetch()`请求在域之间刷新令牌。当我在 fetch 响应中收到 httpOnly cookie 时，该 cookie 不会被转换到 devtools 中的 Application 选项卡上，这意味着它不会在我的下一个 API 请求中自动传递。

我花了一段时间试图找到这个问题，并在这篇文章中提出我的解决方案(许多可能性之一)。

问题在于信任。如果您提供来自单个域的内容，就不会有什么问题。然而，如果您的 API 服务器运行在 [http://localhost:3000，](http://localhost:3000,)上，而您的 React 应用程序通过 webpack-dev-server 运行在 [http://localhost:8081](http://localhost:8081) 上(例如)，那么就浏览器而言，这些是不同的域，并且安全规则开始生效。这些规则需要更多的设置和理解来告诉浏览器我们真的信任另一个域。由于 webpack-dev-server 的工作方式，这些额外的步骤可能仍然不起作用。

如果你使用 Fetch()请求一个 API 资源，你必须包含`mode: 'cors'`和`credentials: 'same-origin'`。但即使是这些也可能不够。

“简单”的解决方法是将您的 API 请求转移到一个代理后面，这样看起来就像是来自您的主域。例如，导航到`[http://localhost:8081/api/login](http://localhost:8081/api/login)`会导致在浏览器不知道的情况下调用`[http://localhost:**3000**/api/login](http://localhost:3000/api/login)`。这在 Nginx/Apache 中相对容易设置:

```
// nginx
location '/api' {
  proxy_set_header   X-Forwarded-For $remote_addr;
  proxy_set_header   Host $http_host;
  proxy_pass         [http://l](http://192.168.43.31:5000)ocalhost:3000;
}
```

但是当我们开发时，我们可能只使用 webpack-dev-server。

Webpack-dev-server 也可以做代理。只需要一点小小的设置。

在您的`webpack.config.js`文件中，您可以像这样设置一个代理:

```
module.exports = merge(common, {
  ...
  devServer: {
    contentBase: './dist',
    // the historyAPIFallback allows react-router to work
    historyApiFallback: true,
    proxy: {
      // when a requst to /api is done, we want to apply a proxy
      '/api': {
        changeOrigin: true,
        cookieDomainRewrite: 'localhost',
        target: 'http://localhost:3000',
        onProxyReq: (proxyReq) => {
        if (proxyReq.getHeader('origin')) {
          proxyReq.setHeader('origin', 'http://localhost:3000')
        }
    },
  },
  ...// derived from code found at [https://sdk.gooddata.com/gooddata-ui/docs/4.1.1/ht_configure_webpack_proxy.html](https://sdk.gooddata.com/gooddata-ui/docs/4.1.1/ht_configure_webpack_proxy.html)
```

现在，当你重启你的开发服务器时，你可以向`/api`发出请求，它们将被传递到你的 API 服务器。就浏览器而言，你已经从同一个域请求了一个资源，所以那些讨厌的安全规则不适用。

最重要的是，httpOnly cookies 是在浏览器的 devtools 中的 Application 选项卡上正确设置的。(您可能仍然需要弄乱 fetch()的头和/或模式/凭证设置。)这意味着您发出的下一个请求也会传递这个 cookie。viola——我们可以使用 JWT 令牌，而无需将它存储在浏览器中，也不会将其暴露给恶意的 JS 攻击。

额外的好处是，我们的 React 应用程序不再需要跟踪 API 服务器的位置。所有 API 资源都可以指向`/api`，底层服务器负责正确的代理调用。

从长远来看，正确的答案是让它适合生产环境。这通常意味着 Nginx、Apache、IIS 或其他常见的 web 服务器。只要这些服务器能够配置目录代理，这就很容易。

即使这不是一个永久的解决方案，它也允许我们继续开发应用程序的其余部分。