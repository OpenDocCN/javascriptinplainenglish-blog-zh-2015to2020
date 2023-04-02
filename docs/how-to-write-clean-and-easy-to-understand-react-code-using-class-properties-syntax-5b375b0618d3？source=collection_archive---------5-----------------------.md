# 如何使用类属性语法编写清晰易懂的 React 代码

> 原文：<https://javascript.plainenglish.io/how-to-write-clean-and-easy-to-understand-react-code-using-class-properties-syntax-5b375b0618d3?source=collection_archive---------5----------------------->

## 不再需要在构造函数中绑定事件处理程序

![](img/c9fa613514f721b920779c8a4c0540a3.png)

Photo by [Filiberto Santillán](https://unsplash.com/@filijs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天，我们将看到一种简化 React 代码的方法，即不再需要绑定我们添加到组件中的每个新事件处理程序。

看看下面的代码:

现场演示:[https://codesandbox.io/s/boring-wu-qud80](https://codesandbox.io/s/boring-wu-qud80)

这里我们有一个在开始时初始化为零的计数器。我们还添加了一个事件处理程序，当我们单击`Add One`按钮时，它会将计数器值加 1。

为了在事件处理程序中使用关键字 **this** ,我们必须在构造函数中将其绑定为

```
this.handleAddOne = this.handleAddOne.bind(this);
```

此外，为了声明状态，我们必须创建一个构造函数，在其中添加一个超级调用，然后我们可以声明状态。这不仅麻烦，而且使代码变得不必要的复杂。
***随着事件处理程序数量的增加，*** `***.bind***` ***调用的数量也随之增加。我们可以使用类属性语法来避免这样做。***

**create-react-app 已经内置了对它的支持，你现在就可以开始使用这个语法了**

所以我们来详细探讨一下。

类属性语法允许我们在类内部直接绑定属性，如状态或实例变量，而不需要构造函数。

*如您所知，箭头函数没有自己的上下文。它把* ***这个*** *的上下文从函数所在的地方放了出来。因此，我们可以将事件处理程序转换为 arrow 函数语法，该语法将使用类上下文，这将消除绑定* ***this 的需要。***

现在，使用新语法重写上面的反例:

现场演示:[https://codesandbox.io/s/twilight-sea-lnk6z](https://codesandbox.io/s/twilight-sea-lnk6z)

正如你所看到的，我们已经将状态声明从构造函数直接移到了类内部，这将使状态成为类的属性。另外，我们已经将`handleAddOne`事件处理程序从函数语法转换为箭头函数语法。

因此该函数将保留**这个**上下文**。所以最后，我们已经完全消除了添加构造函数的需要，这将使我们的代码更干净，更容易理解。**

这不仅仅局限于事件处理程序，你可以将组件内部定义的任何函数转换成箭头函数，以避免绑定**这个**。

***使用类属性语法是现在*** `***React***` ***世界中的新标准，每个人都在积极地使用它。***

**安装:**

1.**`create-react-app`**内置了对它的支持，所以你可以直接使用这个新语法，无需任何配置。****

**2.如果您正在使用您自己的 webpack 设置，如本文中所述，并且您正在使用高于或等于 7 的 babel 版本，那么使用**

```
npm install --save-dev @babel/plugin-proposal-class-properties
```

**同样添加，**@ babel/plugin-proposal-class-properties**到`.babelrc`文件中的插件数组，如下所示**

```
{
 "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

**您已经准备好使用这个新语法了**

**3.如果您正在使用自己的 webpack 安装程序，并且使用的是低于 7 的 babel 版本，请使用**

```
npm install --save-dev babel-plugin-transform-class-properties
```

**另外，将**transform-class-properties**添加到`.babelrc`文件中的插件数组，如下所示**

```
{
 "plugins": ["transform-class-properties"]
}
```

**今天到此为止。希望你今天学到了新东西。**

****别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周简讯，里面有惊人的技巧、诀窍和文章。****