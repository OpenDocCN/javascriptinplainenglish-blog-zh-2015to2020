# Vue 提示—通过 HTTPS 登录、获取请求和服务 Vue 项目

> 原文：<https://javascript.plainenglish.io/vue-tips-login-get-requests-and-serve-vue-project-via-https-c6ad1e08fec2?source=collection_archive---------8----------------------->

![](img/b6bc2519b52da18d1f2cd08c80e11d0f.png)

Photo by [Matheus Queiroz](https://unsplash.com/@queirozmm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Vue.js 是一个用于创建前端 web 应用程序的流行框架。

在本文中，我们将了解一些编写更好的 Vue.js 应用程序的技巧。

# 使用 Axios 强制下载 GET 请求

通过将响应数据传递到`Blob`构造函数中，我们可以使响应总是下载的。

例如，我们可以写:

```
axios
  .get(`download-pdf`, {
    responseType: 'arraybuffer'
  })
  .then(response => {
    const blob = new Blob(
      [response.data], 
      { type: 'application/pdf' }
    ),
    const url = window.URL.createObjectURL(blob);
    window.open(url) ;
  })
```

我们向`download-pdf`端点发出 GET 请求，下载我们的 PDF。

我们确保我们指定了`responseType`是`'arraybuffer'`来表明它是一个二进制文件。

然后在`then`回调中，我们得到了`response`参数，它在`data`属性中包含了我们想要的数据。

我们将它传递给数组中的`Blob`构造函数。

我们指定响应数据的 MIME 类型。

然后我们创建 URL，让我们用`createObjectURL`下载文件。

最后，我们用`url`调用`window.open`来下载文件。

我们也可以通过在`then`回调中做一个小的改变来指定下载文件的文件名。

为此，我们写道:

```
axios
  .get(`download-pdf`, {
    responseType: 'arraybuffer'
  })
  .then(response => {
    const blob = new Blob(
      [response.data], 
      { type: 'application/pdf' }
    ),
    const link = document.createElement('a');
    link.href = window.URL.createObjectURL(blob);
    link.download = "file.pdf";
    link.click();
  })
```

我们没有直接创建一个对象 URL，而是先创建一个不可见的链接，并将文件名设置为它的`download`属性。

然后我们调用`click`来下载。

# 当垂直模型改变时触发事件

我们可以在输入中添加一个变更事件监听器来监听输入值的变更。

例如，我们可以写:

```
<input 
  type="radio" 
  name="option" 
  value=""
  v-model="status" 
  v-on:change="onChange"
>
```

`onChange`是变更事件处理程序的名称。

我们也可以将`v-on:`简称为`@`:

```
<input 
  type="radio" 
  name="option" 
  value=""
  v-model="status" 
  @change="onChange"
>
```

此外，我们可以为我们的模型属性添加一个观察器，而不是附加一个变更事件侦听器。

例如，我们可以写:

```
new Vue({
  el: "#app",
  data: {   
    status: ''
  },
  watch: {
    status(val, oldVal) {
      console.log(val, oldVal)
    }
  }
});
```

使用`watch`属性，我们为`status`变量添加了一个观察器，如名称所示。

`val`为当前值，`oldVal`为旧值。

# 使用 Vue 路由器登录后重定向到请求的页面

当重定向到登录页面时，我们可以向查询参数添加一个重定向路径。

例如，我们可以写:

```
onClick() {
  if (!isAuthenticated) {
    this.$router.push({ name: 'login', query: { redirect: '/profile' } });
  }
}
```

我们检查认证凭证。

如果他们不存在，那么我们调用`this.$router.push`来重定向到登录页面的路径。

`query`属性有登录成功时重定向到的路径。

我们有一个带有`redirect`键和`/profile`值的查询字符串。

然后，在我们的登录表单组件中，我们可以编写:

```
submitForm() {
  login(this.credentials)
    .then(() => this.$router.push(this.$route.query.redirect || '/'))
    .catch(error => {
       //...
    })
}
```

在`methods`地产。

我们称之为`login`，它返回一个承诺。

所以我们可以用回调函数调用`then`来重定向到查询字符串中的路径。

我们得到了带有`this.$route.query.redirect`属性的路径。

然后我们调用`this.$router.push`进行重定向。

如果没有定义，我们重定向到`/`路线。

# 使用 HTTPS 运行 Vue.js 开发服务器

我们可以通过修改`build/dev-server.js`中的代码来读取证书和私钥文件，从而运行 Vue 的开发服务器和 HTTPS。

然后我们可以用它们调用`https.createServer`来创建 HTTPS 服务器。

例如，我们可以写:

```
const https = require('https');
const fs = require('fs');
const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem'))
};
const server = https.createServer(options, app).listen(port);
```

我们读取文件并将它们放入`options`对象中。

然后我们将对象传递给`https.createServer`方法。

`app`是快递服务器的 app 对象。

![](img/31010a9648d94761c1528397f861527f.png)

Photo by [Adam Grabek](https://unsplash.com/@agmakonts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以提供一个开发 HTTPS 服务器 Vue 项目。

还有，我们可以用 Axios 下载文件。

我们还可以添加观察器来观察可变的变化。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**