# 为什么使用道具会让 React App 变得更强大、更灵活

> 原文：<https://javascript.plainenglish.io/why-using-props-makes-react-app-powerful-and-more-flexible-24bde40dd9ee?source=collection_archive---------5----------------------->

## 理解 React 中使用道具的力量

![](img/c84a29d92ab0ff6d475f2fd6f2e219c1.png)

Photo by [Tianyi Ma](https://unsplash.com/@tma?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将看到如何通过传递道具使组件更加灵活。

## 我们开始吧

假设，我们有一个对象数组，每个对象都包含要显示的标签和内容

```
const data = [
  { label: 'Home', content: <HomePage /> },
  { label: 'Contact', content: <ContactPage /> },
  { label: 'About', content: <AboutPage /> }
];
```

我们有一个接受该数组的`Main`组件，它将显示基于该数据的导航和内容。

```
<Main data={data} />
```

Main.js

正如你所看到的，我们有`renderNav`和`renderContent`函数来显示数据，基于哪个导航菜单被点击，我们显示与该菜单相关的内容。

演示:[https://codesandbox.io/s/initial-code-9oc42](https://codesandbox.io/s/initial-code-9oc42)

预告:[https://9oc42.csb.app/](https://9oc42.csb.app/)

你可以找到 Github 的源代码，直到这里[这里](https://github.com/myogeshchavan97/flexible-app/tree/initial-code)

现在，如果我们想在页面的底部而不是顶部显示导航怎么办？

我们如何做到这一点？

我们可以传递一个属性来标识顶部或底部，基于这个属性值，我们可以显示导航。

在 React 中，这段代码

```
render() {
  return (
    <div>
      <div>Header</div>
      <div>Content</div>
    </div>
  );
}
```

与下面的代码相同

```
render() {
  return [ <div>Header</div>, <div>Content</div> ];
}
```

因此，我们可以在一个数组中对 JSX 进行分组，react 会显示出来。

我们可以在这里使用相同的技术，在页面的顶部或底部显示导航。

在`Main.js`中，我们可以改变渲染方法代码如下

Main.js

对于`Main`组件，我们可以将道具作为

```
<Main data={data} showOnTop={true} />
```

这将在顶部显示导航，如果我们通过`showOnTop`为假，导航将在底部显示。

演示:[https://codesandbox.io/s/top-or-bottom-p5zyz](https://codesandbox.io/s/top-or-bottom-p5zyz)

预告:[https://p5zyz.csb.app/](https://p5zyz.csb.app/)

你可以找到 Github 的源代码，直到这里[这里](https://github.com/myogeshchavan97/flexible-app/tree/top-or-bottom)

现在，如果我们想禁用任何导航，以便它只能由高级用户访问。我们也可以实现它。

假设我们有另一个显示`Downloads`页面的导航菜单选项。

```
const data = [
  { label: 'Home', content: <HomePage /> },
  { label: 'Contact', content: <ContactPage /> },
  { label: 'Downloads', content: <DownloadsPage /> },
  { label: 'About', content: <AboutPage /> }
];
```

然后我们可以有如下的渲染方法

```
render() {
  const isPremiumUser = true; return (
    <Main
      data={data}
      showOnTop={true}
      menuToDisable={isPremiumUser ? 'Downloads' : ''}
    />
  );
}
```

在`Main`组件中，我们可以使用`menuToDisable`道具

Main.js

这里，从`onClick`处理程序返回`null`相当于不添加任何事件处理程序。所以现在，只要改变`menuToDisable`的值，我们就可以禁用任何导航菜单。

因此，要禁用关于页面，我们可以使用

```
<Main 
 data={data} 
 showOnTop={true} 
 menuToDisable={!isPremiumUser ? **'About'** : ''} 
/>
```

演示:[https://codesandbox.io/s/disable-menu-ktj6n](https://codesandbox.io/s/disable-menu-ktj6n)

预告:[https://ktj6n.csb.app/](https://ktj6n.csb.app/)

你可以在这里找到本文的完整 GitHub 源代码，在这里找到现场演示

今天到此为止。我希望你学到了新东西。

**别忘了订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章，直接在这里的收件箱** [**订阅。**](https://yogeshchavan.dev/)

## **用简单英语写的便条**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页[**plain English . io**](https://plainenglish.io/)上找到所有这些——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**