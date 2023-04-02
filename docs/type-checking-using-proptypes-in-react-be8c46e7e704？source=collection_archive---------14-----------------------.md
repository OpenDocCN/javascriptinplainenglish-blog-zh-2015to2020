# 在 React 中使用 PropTypes 进行类型检查

> 原文：<https://javascript.plainenglish.io/type-checking-using-proptypes-in-react-be8c46e7e704?source=collection_archive---------14----------------------->

PropTyping 允许我们对 props 进行类型检查，所以这是从父组件传递到子组件的数据。这使得调试 react 应用程序更加容易。出于本文的目的，我将以一个简单的图书应用程序为例。

![](img/e10febdc8e3ef8bf1cca23ee0aec81d3.png)

by Markus Spiske

## **它是如何工作的？**

首先，创建一个简单的 react 应用程序并安装 **proptypes** 包，`npm install --save prop-types`。现在，如果您检查 package.json 文件，您应该有一个 prop 类型的条目。接下来，将**属性类型**导入到我们的组件文件中。

```
import React, { Component } from 'react';
import PropTypes from "prop-types"class BookDetail extends React.Component {
  render() {
    return (
      <h1>{this.props.book}</h1>
    );
  }
}
```

下面是我们将用作示例的图书数据。

```
export const storeProducts = [
{
    id: 5,
    title: "Behold the Dreamers (Oprah's Book Club)",
    img: "img/BeholdtheDreamers-5.png",
    price: 24,
    author: "Imbolo Mbue",
    info:
      "Jende Jonga, a Cameroonian immigrant living in Harlem, has come to the United States to provide a better life for himself, his wife, Neni, and their six-year-old son. In the fall of 2007, Jende can hardly believe his luck when he lands a job as a chauffeur for Clark Edwards, a senior executive at Lehman Brothers. Clark demands punctuality, discretion, and loyalty—and Jende is eager to please. Clark’s wife, Cindy, even offers Neni temporary work at the Edwardses’ summer home in the Hamptons. With these opportunities, Jende and Neni can at last gain a foothold in America and imagine a brighter future...",
    inCart: false,
    count: 0,
    total: 0
  }
]
```

现在探讨数据有问题的可能性。假设你，团队中的某个人，或者之前参与项目的某个人，将`price`赋给一个布尔值，将`inCart`赋给一个整数，从而搞乱了 API。这些会给我们的应用程序带来大量的错误。

```
export const storeProducts = [
{
    id: 5,
    title: "Behold the Dreamers (Oprah's Book Club)",
    img: "img/BeholdtheDreamers-5.png",
    price: true,
    author: "Imbolo Mbue",
    info:
      "Jende Jonga, a Cameroonian immigrant living in Harlem, has come to the United States to provide a better life for himself, his wife, Neni, and their six-year-old son. In the fall of 2007, Jende can hardly believe his luck when he lands a job as a chauffeur for Clark Edwards, a senior executive at Lehman Brothers. Clark demands punctuality, discretion, and loyalty—and Jende is eager to please. Clark’s wife, Cindy, even offers Neni temporary work at the Edwardses’ summer home in the Hamptons. With these opportunities, Jende and Neni can at last gain a foothold in America and imagine a brighter future...",
    inCart: "24",
    count: 0,
    total: 0
  }
]
```

这就是**类型检查**的用武之地。我们有一个`BookDetail`组件，这是我们的子组件。我们希望对从父组件发送到子组件的道具进行类型检查。首先，我们将为`BookDetail`附加一个特殊属性。在组件名上调用`propTypes`，并将其设置为等于一个对象。在这里，我们可以分配我们需要验证的内容。在这种情况下，我们用`book`作为道具，使用`shape`方法让物体呈现特定的形状。在 shape 方法中，我们将指定我们期望的对象属性，它们是`id`、`img`、`title`、`price`和`inCart`。

```
class BookDetail extends React.Component {
  render() {
    return (
      <h1>{this.props.book}</h1>
    );
  }
}// Typechecking using PropType
BookDetail.propTypes = {
  book: PropTypes.shape({
    //here the PropType is looking for a number    
    id: PropTypes.number,
    //the PropType is looking for an string
    img: PropTypes.string,
    title: PropTypes.string,
    price: PropTypes.number,
    //here the PropType is looking for a boolean
    inCart: PropTypes.bool
  }).isRequired
}
```

在应用了`shape`方法之后，现在我们已经设置了`PropTypes`，下一步是使其成为必需的。保存时，您将在控制台上收到以下错误。

```
Warning: Failed prop type: Invalid prop `book.price` of type `boolean` supplied to `BookDetail`, expected `number`.
    in BookDetail (at BookList.js:24)
    in div (at BookList.js:20)
    in div (at BookList.js:17)
    in div (at BookList.js:16)
    in BookList (created by Context.Consumer)
    in Router (created by BrowserRouter)
    in BrowserRouter (at src/index.js:11)
    in BookProvider (at src/index.js:10)
```

该错误清楚地表明图书类型是当前的布尔值，但它需要一个数字。现在，您可以返回到您的数据并检查哪里需要更改。在这种情况下，我们将把`price`改为一个整数值。接下来，它将为`inCart`值抛出一个错误，该值当前是一个字符串，我们将它改为一个布尔值。

使用 prop-types 进行类型检查是一种快速简单的方法，可以检查数据中是否有错误，或者数据是否运行正常。一定要在你的下一个项目中尝试一下。