# 带有 Axios 拦截器的 React 基于令牌的身份验证模块

> 原文：<https://javascript.plainenglish.io/all-in-one-tutorial-on-how-to-create-the-authentication-part-of-your-react-app-2530e7435629?source=collection_archive---------0----------------------->

## 关于如何创建 React 应用程序的身份验证部分的一体化教程。

![](img/4cc517ffea8b3be1c4f69479378f676e.png)

Photo from pexels.com

# OAuth 2.0 授权框架

在我们开始之前，必须介绍身份验证令牌的定义和用法，即访问令牌和刷新令牌。

## **访问令牌**

访问令牌用于基于令牌的身份验证，以允许访问 API。用户成功验证和授权自己后，将收到访问令牌。然后这些令牌在调用 API 时作为凭证传递。一个*访问*令牌通知 API 它的持有者被授权*访问*API，并且如果它们落在[授权期间被授予的范围](https://oauth.net/2/scope/)内，则执行特定的动作。

## **刷新令牌**

基本上，刷新令牌用于获取新的访问令牌。刷新令牌的寿命通常比访问令牌的寿命长得多。当用户的访问令牌过期或无效时使用它，以便用户可以通过获取新的访问令牌来重新获得访问权。

## **我将在本教程中使用** [**轴**](https://www.npmjs.com/package/axios)

**Axios 是一个 JavaScript 库，用于从 Node.js 发出 HTTP 请求，或者从支持 ES6 Promise API 的浏览器发出 XMLHttpRequests。这里有一篇完整的文章解释使用 Axios 的好处。**

## **Axios 拦截器:**

**拦截器用于以某种实现某种全局错误处理方案的方式修改请求头。**

**如果请求失败，可以通过拦截任何请求来处理错误，如果请求符合某些*错误*条件，则执行一些代码。**

```
axiosInstance.interceptors.response.use(
  response => { return response; }, error => {
    if (errorSatifiesConditions(error)) {
      return doSomething().then({....
      // could be to resend original request 
      )} 
    }  else {
      doSomethingElse()...
```

## **[了解](https://reactjs.org/docs/hooks-intro.html)React hooks—React v 16，2019 年 2 月—**

**在本教程中，我将使用 React 钩子，即:useState 和 useContext。**

# **我们开始吧**

**我将向您简要介绍身份认证模块的运行和服务方式，然后，我将它分解为更小的组件，以便您了解所有内容。**

# **解释逻辑**

**第一步是用户输入 URL。如果这个用户以前登录过，你的应用程序应该检查 *localStorage* 。**

*   ****答——如果在 *localStorage、*** 中有关于用户的信息，你的 React 应用将继续查看用户的 access_token 是否仍然有效。
    *如果* `*access_token*` *处于活动状态，则*用户准备就绪，并成功通过认证。
    *如果* `*access_token*` *过期，*您的应用程序应该发送一个`refreshToken`请求并获得一个新的`access_token`**
*   ****B-如果 *localStorage、*** 中没有关于用户的信息，你的应用程序应该将用户重定向到一个登录页面，在那里他们可以给出用户名和密码(或注册页面)。**

**负责 A 的功能是`getUser()`，做 B 的功能是`login(username, password)`。**

****所以，** **主要思路是**，一旦 app 启动，app 调用`getUser()`要么认证用户，要么不认证。
如果不是，则应用程序使用用户凭证调用`login`，并且`login`函数调用`getUser`来获取`currUser`信息。**

**![](img/12c1a0d90f46ef911b628b39b7fc9c08.png)**

**Photo from pexels.com**

# **为了让它更容易**

## ***constants.js***

**创建一个包含所有常量的 constants.js 文件，这样您就不必在每次调用端点时都键入常量。
有了`axios.create`就更抽象*更容易使用。
**my_app 是 axios 实例**。***

```
import axios from "axios";export const URL = "https://your/url.com";
export const API_URL = "https://your/api/url.com";
export const my_app = axios.create({baseURL: API_URL});
```

# **设置对 API 的调用— *auth.js***

**创建一个包含所有函数的 auth.js 文件。**

## **`*1\. refreshToken*`**

**对刷新端点的基本调用。**

```
const refreshToken = () => { let currUser = JSON.parse(localStorage.getItem("my_app_user")); let getUserFormData = new FormData(); getUserFormData.append("grant_type", "refresh_token"); getUserFormData.append("refresh_token", currUser.refresh_token); return new Promise((resolve, reject) => { my_app .post(`${URL}/token/url/`, getUserFormData, { headers: { Authorization: "Basic {secret_key}" } }) .then(async response => { resolve(response); }) .catch(error => { reject(error); }); });};
```

## **2.`*getUser*`**

**`getUser` 负责检查 *localStorage* 中是否存储有用户。如果没有找到，它返回 null。**

**如果它在 *localStorage* ***，*** 中找到一个用户，它会获取该用户的`access_token` 上次刷新的日期，并从当前时间中减去该日期，然后将其与`access_token`的到期时间进行比较。如果它仍然有效，它继续并获得`currUser`信息。**

**如果无效，它调用`refreshToken`端点并用新的接入点替换它，并重置 *localStorage* 中`currUser`中的`lastRefresh`。你不应该忘记用新的访问令牌替换 axios 实例的头。**

```
my_app.defaults.headers.common["Authorization"] =
"Bearer " + currUser.access_token;
```

****Axios 拦截器:** 您将使用 Axios 拦截器拦截任何请求，如果请求失败，错误状态代码代表过期的访问令牌(通常:401，错误消息不同)。这意味着授权失败，因为`access_token`已经过期。
在这种情况下，您需要发送一个`*refreshToken*`请求，然后用从`*refreshToken*`请求中带回的新的有效`access_token` 重新发送一开始失败的请求。**

```
my_app.interceptors.response.use(response => { return response; },error => {if (error.response.status === 401 && error.response.message === "blablabla") {return refreshToken().then({....// resend original request
```

**最后，如果`login`或 `getUserInfo`或`refreshToken`请求失败，您应该注销并从 *localStorage* 中删除`currUser`，因为这样更安全，并且会将用户重定向到登录页面。您也可以在这里处理特定类型的错误。比如特定网络错误等等。**

**一个`refreshToken`请求可能失败的一个原因是`refresh_token`已经过期的罕见情况。请求令牌的生存期相对来说比`access_token`长得多，但是它仍然有可能过期，在这种情况下，您应该使用前面提到的`logout`将用户重定向到登录页面。**

```
const getUser = () => { let currUser = JSON.parse(localStorage.getItem("my_app_user")); if (!currUser) {
    // if no user in localStorage then the user must enter their      credentials to proceed return Promise.resolve(null); } // get the expiry time of the current access token and measure   whether it expired or not let currDate = new Date(); let diff = currDate.getTime() - currUser.lastRefresh; if (diff >= currUser["expires_in"] * 1000) { // access token expired need to refresh token // you could just call refreshToken I just typed it in so you could see it clearer // call refreshToken() // if success, update currUser with the new access_token and save this instant to be the last_refresh (for future refresh) and update axios headers // if fail logout() } else { // Do not need refresh my_app.defaults.headers.common["Authorization"] =
"Bearer " + currUser.access_token; my_app.interceptors.response.use( response => { return response; }, error => { if (error.response.status === 401 && some other condition that sprecifies an expir state){ // 401 is Unauthorized error // which means that this request failed // what we need to do is send a refresh request then resend the same request // that failed but with the new access point. // We can do this automatically using axios interceptors return refreshToken() .then(response => {

      // update currUser with new access_token // Set default headers to have authorization the access token as authorization for future requests // Get the original that failed due to 401 and resend it // with the new access token const config = error.config; config.headers.Authorization = "Bearer " + response.data.access_token; // Resending original request return new Promise((resolve, reject) => { my_app .request(config) .then(response => { resolve(response); }) .catch(error => { reject(error); }); }); }) .catch(error => { // just logout() if anything goes wrong});
    } logout(); return new Promise((resolve, reject) => { reject(error); }); } ); // finally get user  return my_app.get("/users/current/url").catch(error => { logout(); throw error; });
  }
};
```

## **3.`*login*`**

**这个函数简单地获取用户名和密码，并将它们和密钥一起发送过来进行身份验证。**

```
const login = (username, password) => { let loginFormData = new FormData(); loginFormData.append("grant_type", "password"); loginFormData.append("username", username); loginFormData.append("password", password); return new Promise((resolve, reject) => { axios .post(`${URL}/token/url`, loginFormData, { headers: { Authorization: "Basic {secret_key}" } }) .then(async response => { response.data.lastRefresh = new Date().getTime(); localStorage.setItem("my_app_user", JSON.stringify(response.data)); getUser() .then(response => { resolve(response); }) .catch(error => { reject(error); });      }) .catch(error => { reject(error); }); }); };
```

## **4.`logout`**

**基本上只是从*本地存储器*中删除`currUser`。**

```
const logout = () => { localStorage.removeItem("my_app_user"); return Promise.resolve();};
```

## ***5。认证上下文***

**这个上下文将一直保存你的用户变量。从`user`你就能知道。**

1.  **无论是否有用户登录。**
2.  **用户信息。**

```
import React, { createContext, useState } from "react";import auth from "../helpers/auth";import LoadingPage from "../components/LoadingPage";export const AuthenticationContext = createContext();const AuthenticationContextProvider = props => { const [user, setUser] = useState(null); const [fetched, setFetched] = useState(false); const login = (username, password) => { return auth.login(username, password).then(data => { setUser(data.data);});};const logout = async () => { setUser(null); await auth.logout();};const getUser = async () => { const user = await auth.getUser(); setUser(user ? user.data : null);};if (!fetched) { getUser().then(() => setFetched(true));}if (!fetched) { return <LoadingPage />;}return ( <AuthenticationContext.Provider value={{ user, login, logout, getUser, setUser }} > {props.children} </AuthenticationContext.Provider> );};export { AuthenticationContextProvider };
```

**`**fetched**` **让你知道你已经尝试获取** `currUser`，如果找到，用户信息将在`user`中，否则将为空。**

**下面是启动之前解释过的授权过程的部分代码。**

```
if (!fetched) { getUser().then(() => setFetched(true));}
```

## **6 *。最后一步***

**这一步是您的**认证开关**，它根据上下文中的`user`将用户重定向到经过认证的页面或未经认证的登录页面。**

```
const { user } = useContext(AuthenticationContext);return user ? <AuthenticatedApp /> : <UnauthenticatedApp />;
```

## **重要说明**

**将您的路线和路由器放在`Authenticatedapp`中会大大简化您的路线选择；如果用户成功通过身份验证，他们将被重定向到他们键入的 URL 的相应路由，而不是您手动进行路由。**

> ****最后**，这是一个完整的基于令牌的 React 认证教程。我希望你已经学会了。如果你有任何问题，请不要犹豫地问。**
> 
> **你会在这个 [GitHub 库](https://github.com/SalmaGhoneim/React-token-based-authentication-module-with-Axios-Interceptors-tutorial)中找到所有的代码文件。**

## ****简明英语笔记****

**你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)，[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！****

****此外，我们总是有兴趣帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！******