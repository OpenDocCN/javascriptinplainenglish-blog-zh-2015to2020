# 反应模式——文档和可重用性

> 原文：<https://javascript.plainenglish.io/react-patterns-documentation-and-reusability-3c7c4a383cd6?source=collection_archive---------10----------------------->

![](img/00d8a4bef03eadb2e34af0bd1b33bdf1.png)

Photo by [James Sutton](https://unsplash.com/@jamessutton_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将研究如何自动记录组件。

# 反应文档生成

React Docgen 包帮助我们自动创建组件的文档。

我们可以通过运行以下命令来安装它:

```
npm install --global react-docgen
```

然后我们可以运行:

```
react-docgen App.js
```

如果我们有一个按钮存储在`App.js`中。

然后我们得到一个 JSON 对象，它的输出是适当的类型。

如果我们在组件的顶部添加一个组件，我们还会得到一个描述字段，该字段的值是注释。

# 可重用组件

为了创建可重用的组件，我们将可重用的部分拉出组件。

例如，如果我们从一个 API 得到一些东西，我们可以写:

```
import React, { useState, useEffect } from "react";const fetchPerson = async name => {
  const res = await fetch(`https://api.agify.io/?name=${name}`);
  return await res.json();
};export default function App() {
  const [data, setData] = useState({}); const onLoad = async name => {
    const personData = await fetchPerson(name);
    setData(personData);
  }; useEffect(() => {
    onLoad("michael");
  }, []);
  return <p>{data.name}</p>;
}
```

我们将组件之间可以共享的部分移到了它自己的功能中。

`fetchPerson`函数让我们获取获取数据。

那么组件中必须有的部件就在`App`了。

我们有`onLoad`功能来设置`data`状态。

`useEffect`有状态。

如果我们记得不重复自己，那么我们将把代码分成可重用的部分。

如果我们创建一个列表，那么我们应该把列表条目分成独立的部分。

例如，我们可以写:

```
import React, { useState } from "react";const Item = ({ name }) => <p>{name}</p>;export default function App() {
  const [data] = useState(["foo", "bar"]); return (
    <div>
      {data.map((d, i) => (
        <Item name={d} key={i} />
      ))}
    </div>
  );
}
```

我们有一个`Item`组件来呈现条目的数据。

然后我们调用`map`并在回调中返回`Item`，用`Item`呈现列表。

我们传入带有我们的值的`name`道具。

此外，我们有`key`属性，因此 React 可以正确地跟踪这些值。

这样，我们可以在不影响`App`的情况下根据需要更改`Item`组件。

我们可以添加逻辑、状态等。因为我们需要。

如果我们愿意，我们也可以将`Item`用于其他目的。

# 生活方式指南

如果我们创建一个风格指南，那么一个新的开发者复制已经存在的东西的机会就少了。

他们可以找到可用的组件，然后使用已经构建的组件，而不是构建新的组件。

手动创建风格指南很难，因此有一些应用程序可以让我们更轻松地完成这项工作。

我们可以通过像 Bit 或者故事书这样的应用来实现。

React Storybook 让我们提取组件，并将它们放入动态样式指南中。

使用 Bit，我们可以将组件上传到 Bit web 应用程序，并将其作为一个包导入到其他应用程序中。

此外，我们可以预览上传的组件。

![](img/7f1c532d512bae9f6b66339db936e327.png)

Photo by [Andrew Umansky](https://unsplash.com/@angur?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 React Docgen 包为 React 组件创建文档。

为了制造可重用的组件，我们应该将它们提取成小块，并通过 props 将数据从父组件传递给子组件。

不改变状态的逻辑也可以提取。

为了防止意外的重复，我们可以用 Bit 或 Storybook 这样的应用程序创建一个风格指南。

## 简单英语的 JavaScript

你知道我们有四种出版物吗？通过 [**plainenglish.io**](https://plainenglish.io/) 找到他们——通过关注我们的出版物和 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**