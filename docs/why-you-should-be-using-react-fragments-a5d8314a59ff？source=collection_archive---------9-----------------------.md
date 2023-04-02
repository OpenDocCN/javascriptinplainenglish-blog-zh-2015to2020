# 为什么应该使用片段

> 原文：<https://javascript.plainenglish.io/why-you-should-be-using-react-fragments-a5d8314a59ff?source=collection_archive---------9----------------------->

![](img/044b6afacef01da8174663398caccd22.png)

[@casparrubin](https://unsplash.com/@casparrubin) unsplash.com

## 如何使用 React 提升 React 应用程序？碎片

在之前的一篇博客文章中(见这里的)，我们讨论了为什么需要在一个 div 中包装我们的 React 元素，如果你还没有看过那篇文章，请看看这里！

在那篇文章的最后，我们讨论了为什么让不必要的 div 元素进入 DOM 可能会导致问题。嗯，我想一定有一个解决这个问题的方法，对吗？

在 React 16.2 版中，引入了一个称为 React 片段的新特性。可以把这个看作是一个不翻译成 HTML 的组件。

而不是使用`<div>`来包装我们使用的元素或速记语法`<>`。

下面我们给出了当您没有将 React 元素包装在`div`中时会出现的错误的典型解决方案。

```
import React from 'react'const App = () => {
  return (
           <div> 
              <p>Hello</p>
              <p>World</p>
           </div>
      )
    }
```

下面是我们如何使用`React.fragment`重写它

```
import React from 'react'
const App = () => {
  return ( 
           <React.fragment>
             <p>Hello</p>
             <p>World</p>
           </React.fragment>
      )
    }
```

现在写`React.fragment`可能会很痛苦！所以 React 也引入了`<>`语法

```
import React from 'react'
const App = () => {
  return ( 
           <>
             <p>Hello</p>
             <p>World</p>
           </>
      )
    }
```

被 transpiler 转换成常规的 JavaScript，转换后看起来像这样。一些我们在以前的帖子中看到的东西！

`React.createElement(React.fragment, null, ...children)`

不同的是，它没有被插入到 DOM 中！

# 碎片有什么好处

如前一篇文章所述。以下是你使用片段的主要原因。

1.  您的 react 应用程序越来越大，DOM 中无用 div 的数量也在增加。
2.  性能稍微好一点，所以如果你想节省一些时间，这可能是一种方法。
3.  当您关注布局和最终 HTML 的呈现时，并不考虑布局应该如何显示

# 我如何使用它们

# 1.返回元素组

这个直接取自 React 文档。假设我们想使用 React 呈现一个表格。在`div`中包装`td`标签将不会正确地呈现表格！但是使用 React 片段可以！

```
import React, Fragment from 'react'
const App = () => {
  return ( 
           <React.fragment>
               <td>Hello</td>
               <td>World</td>
           </React.Fragment>
      )
    }
```

这使得

```
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

# 2.有条件渲染

这里我们呈现一个表单，告诉我们用户是否已经登录。如果没有，我们使用片段来显示多个 React 元素，这些元素显示登录表单。如果为真，我们将显示一条登录信息。注意，我们使用三元运算符来进行这种类型的条件渲染！

```
const App = () => {
  return ( 
           <form>
             {LoggedIn ? (
            <React.Fragment>
              <h3>Welcome</h3>
              <p>You are logged in!</p>
            </React.Fragment>
        ) : (
            <React.Fragment>
              <h3>Login</h3>
              <label for="username">Username</label><br/>
              <input type="text" id="username" /><br/>
              <label for="password">Password</label><br/>
              <input type="password" id="password" /><br/>
              <input type="submit" value="Login" />
            </React.Fragment>
        )}
      </form>
      )
 }
```

# 3.显示带有片段的数组

现在，有时你会想要显示一个数组，但是为了做到这一点，React 建议你包含一个键属性。这是因为它使 react 更容易基于此更改 DOM。现在请注意，您应该使用`React.fragment`而不是`<>`，因为现在`<>`还不支持使用键属性。

```
const App = () => { 
  users = [
      {
        id: 1,
        name: "User1"
        phone: "123456789"
      },
      {
        id: 2,
        name: "User2",
        phone: "234567890"
      },
      {
        id: 3,
       name: "user2",
       phone: "345678901"
    }
  ]
  return (
     <React.Fragment>
        {this.users.map(user => (
          <React.Fragment key={user.id}>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
            <p>{user.phone}</p>
          </React.Fragment>
        ))}
    </React.Fragment>
  )
}
```

# 其他文章

[](https://medium.com/javascript-in-plain-english/embed-a-qr-code-scanner-and-browser-into-your-next-mobile-app-42245cbdebf) [## 将二维码扫描仪和浏览器嵌入你的下一款手机应用

### 如何使用 React Native 嵌入二维码扫描仪和网络浏览器

medium.com](https://medium.com/javascript-in-plain-english/embed-a-qr-code-scanner-and-browser-into-your-next-mobile-app-42245cbdebf) [](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) [## 为什么我们必须包装 React 组件？

### 理解 React 应用程序中的 div 包装！

medium.com](https://medium.com/javascript-in-plain-english/why-do-we-have-to-wrap-react-components-b168232dbd3a) 

## 关于作者

我是一名执业医师和教育家，也是一名网站开发者。请在这里查看[关于我在博客和其他帖子上的项目进展的更多细节。如果你想和我联系，请点击这里](https://dev.to/aaronsm46722627/www.coding-medic.com) [aaron . Smith . 07 @ aberdeen . AC . uk](mailto:aaron.smith.07@aberdeen.ac.uk)