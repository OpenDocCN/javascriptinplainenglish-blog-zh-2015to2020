# Node.js 最佳实践— Nginx

> 原文：<https://javascript.plainenglish.io/node-js-best-practices-nginx-2f00b48b75e?source=collection_archive---------2----------------------->

![](img/1654b366b6d0504065413374cd350809.png)

Photo by [Mick Haupt](https://unsplash.com/@rocinante_11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 Nginx 添加反向代理

我们不应该将我们的 Express 应用程序直接暴露在互联网上。

相反，我们应该使用反向代理将流量导向我们的应用程序。

这样，我们可以添加缓存，控制我们的流量，等等。

我们可以通过运行以下命令来安装它:

```
apt update
apt install nginx
```

然后，我们可以通过运行以下命令来启动它:

```
systemctl start nginx
```

一旦我们运行了 Nginx，我们就可以进入`/etc/nginx/nginx.conf`来改变 Nginx 配置以指向我们的应用。

例如，我们可以写:

```
server {
  listen 80;
  location / {
     proxy_pass http://localhost:3000; 
  }
}
```

我们使用`proxy_pass`指令将任何流量从端口 80 传递到我们的 Express 应用程序，该应用程序正在监听端口 3000。

然后，我们重新启动 Nginx，通过运行以下命令使我们的配置生效:

```
systemctl restart nginx
```

# 使用 Nginx 实现负载平衡

要使用 Nginx 添加负载平衡，我们可以通过编写以下代码来编辑`nginx.conf`:

```
http {
  upstream fooapp {
    server localhost:3000;
    server domain2;
    server domain3;
    ...
  }
  ...
}
```

添加我们应用程序的实例。

`upstream`部分创建了一个服务器组，它将在我们指定的服务器之间对流量进行负载平衡。

然后在`server`部分，我们添加:

```
server {
   listen 80;
   location / {
     proxy_pass http://fooapp;
  }
}
```

使用`fooapp`服务器组。

然后，我们用以下命令重新启动 Nginx:

```
systemctl restart nginx
```

# 使用 Nginx 启用缓存

为了启用缓存，我们可以使用`proxy_cache_path`指令。

在`nginx.conf`中，我们写道:

```
http {
  proxy_cache_path /data/nginx/cache levels=1:2   keys_zone=STATIC:10m
  inactive=24h max_size=1g;
  ...
}
```

我们缓存 24 小时，最大缓存大小设置为 1GB。

此外，我们补充:

```
server {
   listen 80;
   location / {
     proxy_pass            http://fooapp;
     proxy_set_header      Host $host;
     proxy_buffering       on;
     proxy_cache           STATIC;
     proxy_cache_valid     200 1d;
     proxy_cache_use_stale error timeout invalid_header updating
            http_500 http_502 http_503 http_504;
  }
}
```

`proxy_buffering`设置为`on`以增加缓冲。

`proxy_cache`被设置为`STATIC`以启用缓存。

`proxy_cache_valid`将缓存设置为在 1 天后过期，状态代码为 200。

`proxy_cache_use_stale`确定在通信期间何时可以使用过时的缓存响应。

我们将它设置为在使用`updating`关键字更新当前时出现超时错误、无效头错误时使用。

如果响应状态代码是 500、502、503 或 504，我们也可以使用缓存。

# 使用 Nginx 启用 Gzip 压缩

我们可以通过使用`gzip`模块来启用 Nginx 的 Gzip 压缩。

例如，我们可以写:

```
server {
   gzip on;
   gzip_types      text/plain application/xml;
   gzip_proxied    no-cache no-store private expired auth;
   gzip_min_length 1000;
  ...
}
```

在`nginx.conf`中，用`gzip on`启用 Gzip。

`gzip_types`让我们将文件的 MIME 类型设置为 zip。

`gzip_proxied`让我们在代理请求上启用或禁用 gzipping。

我们用`private`、`expired`、`no-cache`和`no-store`、`Cache-Control`头值启用它。

`auth`表示如果请求头启用了授权字段，我们将启用压缩。

`gzip_min_length`意味着我们设置将被压缩的响应的最小长度。

长度是`Content-Length`响应头字段，以字节为单位。

![](img/9e8d678d99ae7053b9c5135348656d23.png)

Photo by [Zbynek Burival](https://unsplash.com/@zburival?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用 Nginx 做很多事情来提高我们应用的性能。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！