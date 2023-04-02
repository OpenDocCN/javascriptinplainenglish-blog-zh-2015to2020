# 反应模式—编写干净的代码

> 原文：<https://javascript.plainenglish.io/react-patterns-writing-clean-code-9535f211a6a9?source=collection_archive---------1----------------------->

![](img/a6d130690bac116eb7eb67844327cefa.png)

Photo by [Zoë Reeve](https://unsplash.com/@zoeeee_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将看看如何清理我们的 React 代码。

# 多属性

如果我们有很多属性或道具，我们应该把它们放在自己的线上。

例如，我们可以写:

```
<Content
 foo="bar"
 anotherProp="baz"
 onClick={this.handleClick}
/>
```

这样，我们可以清楚地看到每个道具。

道具应该有一个缩进。

这样，我们就不会有需要滚动的长队。

# 条件式

我们经常要写条件语句或者表达式来有条件的显示一些东西。

例如，我们可以写:

```
let link;
if (isLoggedIn) {
  link= <LogoutLink />
}
return <div>{link}</div>
```

然而，这很难理解。

更困难的是我们有多种成分和条件。

为了使它更简短，我们可以使用内联表达式来代替:

```
<div>
 {isLoggedIn && <LogoutLink />}
</div>
```

这是因为如果`isLoggedIn`是`false`，那么第二个表达式将不会被求值。

但如果是，那就一定会。

如果我们想为替代情况显示一些内容，而不是编写:

```
let link;
if (isLoggedIn) {
  link = <LogoutLink />
} else {
  link = <LoginLink />
}
return <div>{link}</div>
```

我们可以用一个三元表达式来简化它:

```
<div>
  {isLoggedIn ? <LogoutLink /> : <LoginLink />}
</div>
```

如果我们有更长的布尔表达式，那么我们应该把它们放在一个函数中，而不是把它们写出来:

```
const canShowSecretData = () => {
  return dataLoaded && (isAdmin || userHasPermissions)
}
```

然后我们可以在条件表达式中使用它来表达:

```
<div>
  {canShowSecretData() && <Secret />}
</div>
```

如果我们有一个类组件，我们也可以定义一个 getter:

```
get canShowSecretData(){
  return dataLoaded && (isAdmin || userHasPermissions)
}
```

那么我们可以使用:

```
<div>
 {this.canShowSecretData && <Secret />}
</div>
```

计算出的属性也可以放入函数中。

例如，我们可以写:

```
const getPriceAfterTax = () => {
  return currency * (1 + taxRate);
}
```

在类组件中，我们也可以写:

```
getPriceAfterTax() {
  return this.props.currency * (1 + this.props.taxRate);
}
```

如果它在一个类中，我们也可以使用一个 getter:

```
get priceAfterTax() {
  return this.props.currency * (1 + this.props.taxRate);
}
```

还有一个`render-if`包，让我们传入一个组件来有条件地显示。

我们可以通过编写以下内容来安装它:

```
npm install --save render-if
```

例如，我们可以写:

```
const canShowSecretData = renderIf(
 dataLoaded && (isAdmin || userHasPermissions)
)
```

然后，我们可以有条件地呈现我们的组件:

```
<div>
 {canShowSecretData(<Secret />)}
</div>
```

或者，我们可以使用`react-only-if`包来创建一个组件，该组件由该包附带的高阶组件有条件地呈现。

为了安装它，我们编写:

```
npm install --save react-only-if
```

然后我们可以写:

```
const SecretDataOnlyIf = onlyIf(
 ({ dataLoaded, isAdmin, userHasPermissions }) => {
   return dataLoaded && (isAdmin || userHasPermissions)
 }
)(Secret)...<div>
 <SecretOnlyIf
   dataLoaded={...}
   isAdmin={...}
   userHasPermissions={...}
 />
</div>
```

现在我们不需要在表达式中有逻辑来显示 JSX 代码中的`Secret`。

在将某物渲染为道具之前，我们传入表达式进行检查。

# 环

为了呈现对象列表，我们使用了`map`方法。

例如，我们可以写:

```
<div>
  {users.map(user => <p key={user.id}>{user.name}</p)}
</div>
```

注意，我们必须使用带有唯一 ID 的`key`属性，这样 React 才能正确跟踪列表项。

如果我们要动态地改变条目，这就更重要了。

# 控制语句

有一个`jsx-control-statements`包可以让我们有条件地呈现项目，尤其是在有多个案例的情况下。

我们通过编写以下内容来安装它:

```
npm install - save jsx-control-statements
```

然后在`.babelrc`文件中，我们添加:

```
"plugins": ["jsx-control-statements"]
```

然后我们可以写:

```
<If condition={canShowSecret}>
  <Secret />
</If>
```

如果我们有不止一个案例，我们可以写:

```
<Choose>
  <When condition={...}>
    <span>if</span>
  </When>
  <When condition={...}>
    <span>else if</span>
  </When>
  <Otherwise>
    <span>else</span>
  </Otherwise>
</Choose>
```

`When`就像`case`和`Otherwise`如果像`default`在一个`switch`语句中。

![](img/84e540b3113a61bfb9b53029e7e3e57c.png)

Photo by [Roksolana Zasiadko](https://unsplash.com/@cieloadentro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

在 React 组件中，有许多方法可以使事物变得干净。

如果我们有很多条件语句，我们可能想要使用那些列出的包。

如果我们想要呈现一个列表，我们使用数组的`map`方法。

## **简单英语的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**