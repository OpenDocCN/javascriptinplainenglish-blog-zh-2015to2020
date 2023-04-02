# React 中的身份验证处理

> 原文：<https://javascript.plainenglish.io/authentication-in-react-caf2abfa0494?source=collection_archive---------0----------------------->

## 用钩子和上下文

![](img/bf502d7e19380d7ee34876e1649802f1.png)

Photo by [Micah Williams](https://unsplash.com/@mr_williams_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 用户认证是现代前端应用的主要支柱之一

我们将在这里创建一个 React 应用程序，并在此过程中添加身份验证部分，最终完成自动登录和自动注销。

## 基本认证

我们的 react 应用程序将有一个 App 组件，它将托管另外两个组件。一个是认证逻辑，另一个是受保护的组件，用户只有通过认证后才能看到。

## App.js

```
import react from ‘react’export default function App(){
   return (
       <div class="center">
          <Authentication/>
          <ProtectedResource />
       </div>
   )
}
```

这里是**认证**和**保护资源**组件的代码。

## 认证. js

```
import react from ‘react’export default function Authentication(){
   return (
       <>
          <button className="login">Login</button>
          <button className="logout">Logout</button>
       </>
   )
}
```

## protectedPage.js

```
import react from ‘react’export default function ProtectedResource(){
   return (
       <h1>I am a authenticated resource</h1>
   )
}
```

目前这两个组件都是可见的，因为没有合适的逻辑。

让我们开始一点一点地添加我们的身份验证。由于我们将在多个组件中需要认证信息，我们可以利用 react 中的一个概念，即**上下文**。

> 上下文允许我们在应用程序的任何组件之间传递数据，而不需要使用 props。

我们的上下文应该是这样的:

## 上下文. js

```
import { createContext } from “react”;export const AuthContext = createContext({
    isLoggedIn: false,
    token: null,
    login: () => {},
    logout: () => {}
});
```

我们在这里定义了几个属性，一旦用户登录或注销，我们将在 App.js 文件中设置这些属性。

这就是我们如何在 App.js 中使用 AuthContext，方法是像下面这样导入它:

## App.js

```
import react from ‘react’
import {AuthContext} from ‘./context’;export default function App(){
   return (
       <AuthContext.Provider>
          <Authentication/>
          <ProtectedResource />
       <AuthContext.Provider />
   )
}
```

用 AuthContext 包装我们的应用程序组件可以确保上下文中的任何内容发生变化时，应用程序中使用上下文的子组件也会发生变化。

我们现在可以在应用程序组件中管理一些状态，并将其绑定到我们上下文的值属性，因此当状态发生变化时，我们将能够重新呈现或更新对我们上下文感兴趣的组件。

## App.js

```
import react, { useState } from ‘react’
import {AuthContext} from ‘./context’;export default function App(){
    const [loggedIn, setLoggedIn] = useState(false); const login = () => {
        setLoggedIn(true);
    } const logout = () => {
        setLoggedIn(false);
    }return (
       <AuthContext.Provider value={isLoggedIn: isLoggedIn, login: login, logout: logout}>
           <div class="center">
              <Authentication/>
              <ProtectedResource />
           </div>
       <AuthContext.Provider />
   )
}
```

现在，我们来听听其他组件中的上下文变化:

## protectedPage.js

```
import react, { useContext } from ‘react’
import { AuthContext } from "./context";export default function ProtectedResource(){ 
   const authContext = useContext(AuthContext); return authContext.isLoggedIn && <h2>Protected resource</h2>;
}
```

## 认证. js

```
import react, { useContext } from ‘react’
import { AuthContext } from "./context";export default function Authentication(){
   const authContext = useContext(AuthContext); const loginHandler = () => {
       authContext.login();
   }; const logoutHandler = () => {
       authContext.logout();
   };return (
       <>
          {!authContext.isLoggedIn && <button className="login" onClick={loginHandler}>Login</button>}
          {authContext.isLoggedIn && (<button className="login" onClick={logoutHandler}>Logout</button>
       </>
   )
}
```

您还会注意到，我在身份验证组件中添加了处理程序，它会改变我们上下文中的值。

以下是沙盒中的代码概述:

Basic Authentication

以下几点总结了我们迄今取得的成就:

*   创建了一个包含两个组件“身份验证”和“受保护资源”的应用程序
*   创建了 AuthContext，它包含要与组件共享的身份验证信息
*   用上下文和托管状态包装我们的应用程序组件，用于登录和注销
*   侦听“身份验证”和“受保护的资源”组件中的上下文更改。

# 自动登录和自动注销

现在有趣的部分来了，我们在客户端持久化用户的会话。

![](img/1e04f8f02156643f58e7528ed73b8282.png)

Photo by [Erda Estremera](https://unsplash.com/@erdaest?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

让我们对我们的身份验证组件进行一些更改，假设 me 对服务器进行一个登录调用，结果接收到一个令牌以进行进一步的身份验证。我们还假设这个令牌的过期时间被设置为距后端 1 小时。这个到期时间将会派上用场，以确保我们的客户端在令牌到期之前做出相应的响应。

## 认证. js

```
import react, { useContext } from ‘react’
import { AuthContext } from "./context";export default function Authentication(){
   const authContext = useContext(AuthContext); const loginHandler = () => {
       *//API call to server...* *//response from server* const userResponse = {
           token: "abjd2323jb443jbbb"
       };
       authContext.login(userResponse.token);//setAuthContext       
   }; const logoutHandler = () => {
       authContext.logout();
   }; return (
       <>
          {!authContext.isLoggedIn && <button className="login" onClick={loginHandler}>Login</button>}
          {authContext.isLoggedIn && (<button className="logout" onClick={logoutHandler}>Logout</button>
       </>
   )
}
```

让我们也管理 App.js 中的令牌状态:

## App.js

```
import react, { useState } from ‘react’
import {AuthContext} from ‘./context’;export default function App(){
    const [token, setToken] = useState(null); const login = () => {
        setToken(token);
    } const logout = () => {
        setToken(null);
    } return (
       <AuthContext.Provider value={isLoggedIn: !!token, login:         login, logout: logout}>
          <Authentication/>
          <ProtectedResource />
       <AuthContext.Provider />
   )
}
```

我们从 App 组件中移除了 **isLoggedIn** 状态，并引入了**令牌**状态变量。

> 如果你想知道的话！！值属性中的标记只是将标记值转换为布尔值。

现在，如果您单击登录，应用程序将会以同样的方式运行。它会让您登录并显示受保护的组件。但是，如果重新加载页面，就会出现持久性问题。我们的应用程序的状态在重新加载时丢失，我们被重定向回登录页面。那么，我们在这里可以做什么来保持应用程序状态不变，因为我们知道我们的令牌有一个小时的过期时间。

# 管理令牌到期日期

![](img/95ed6fe540909629bbd9cc01d944a8ac.png)

Photo by [Henry & Co.](https://unsplash.com/@hngstrm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以利用 localStorage 来保存我们的令牌和到期时间，这样即使当用户重新加载页面时，也不会发生重定向到登录状态的情况。

让我们对 App.js 中的代码进行相应的更改:

## App.js

```
import react, { useState } from ‘react’
import {AuthContext} from ‘./context’;export default function App(){
    const [token, setToken] = useState(null);
    const [tokenExpirationTime, setTokenExpirationTime] = useState(); const login = (token) => {
        setToken(token);
        const expiration = new Date(new Date().getTime() + 1000 * 60  * 60); setTokenExpirationDate(expiration);//set expiration time one hour from current time localStorage.setItem(
             "userData",
              JSON.stringify({
                     token,
                     expirationTime: expiration.toISOString()
               })
         );
     } const logout = () => {
        setToken(null);
        setTokenExpirationTime(null);
         localStorage.removeItem(‘userData’);
     } return (
       <AuthContext.Provider value={isLoggedIn: !!token, login:         login, logout: logout}>
          <Authentication/>
          <ProtectedResource />
       <AuthContext.Provider />
     )
}
```

**我们已经创建了一个新的 tokenExpirationTime 状态变量，并将我们的令牌及其到期时间保存在 localStorage 中**。注销时，我们清除两个状态变量。

现在，如果用户重新加载页面，检查某个内容是否在 localStorage 中的最好方法是什么？—是的，你猜对了，在使用效果挂钩。

```
useEffect(() => {
    const storedData = JSON.parse(localStorage.getItem(“userData”)); if (storedData && storedData.token && new Date(storedData.expirationTime) > new Date()) 
     {
        login(storedData.token, new Date(storedData.expirationTime));
     }}, [login]);
```

我们检查 localStorage 是否有相关的数据，如果过期时间仍然在未来，那么只有我们调用我们的登录函数。这里 login 现在用两个参数调用；第二个是未来的到期日。所以我们需要在我们的登录函数中接受如下内容:

```
const login = (token, expirationTime) => {
    setToken(token);
    const expiration = expirationTime || new Date(new  Date().getTime() + 1000 * 60 * 60);

    setTokenExpirationDate(expiration); localStorage.setItem(
          “userData”,
          JSON.stringify({
                token,
                expirationTime: expiration.toISOString()
     })
    );
};
```

通过上面的方法，我们可以确保使用正确的到期时间，并且它不总是被设置为从当前时间算起的一个小时。

现在我们需要设置一个定时器，这样一旦定时器到期，用户就会自动注销。我们的 App.js 最终版本如下:

## App.js

```
import react, { useState, useEffect } from ‘react’
import {AuthContext} from ‘./context’;let logoutTimer;export default function App(){
    const [token, setToken] = useState(null);
    const [tokenExpirationTime, setTokenExpirationTime] = useState(); const login = (token, expirationTime) => {
        setToken(token);
        const expiration = expirationTime || new Date(new Date().getTime() + 1000 * 60  * 60); setTokenExpirationDate(expiration);
        localStorage.setItem(
             "userData",
              JSON.stringify({
                     token,
                     expirationTime: expiration.toISOString()
               })
         );
     } const logout = () => {
        setToken(null);
        setTokenExpirationTime(null);
        localStorage.removeItem(‘userData’);
    } //hook to check if something is there in localStorage and logs user in accordingly
    useEffect(() => {
        const storedData = JSON.parse(localStorage.getItem(“userData”)); if (storedData && storedData.token && new  Date(storedData.expirationTime) > new Date()) 
        {
           login(storedData.token, new Date(storedData.expirationTime));
        } }, [login]); //new useEffect hook to set the timer if the expiration time is in future otherwise we clear the timer here
     useEffect(() => {
         if (token && tokenExpirationDate) {
            const remainingTime = tokenExpirationDate.getTime() - new Date().getTime();
            logoutTimer = setTimeout(logout, remainingTime);
          } else {
            clearTimeout(logoutTimer);
          }
      }, [token, logout, tokenExpirationDate]); return (
       <AuthContext.Provider value={isLoggedIn: !!token, login:         login, logout: logout}>
          <Authentication/>
          <ProtectedResource />
       <AuthContext.Provider />
     )
} 
```

如果您点击登录并再次重新加载应用程序，由于我们上面添加的逻辑，它将留在受保护的资源中。

这么多代码看不到输出，这肯定有点令人困惑，所以添加了一个最终沙箱。希望当你通读代码时，一切都有意义。

仅此而已。

感谢阅读。希望你从这篇文章中有所收获！

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****