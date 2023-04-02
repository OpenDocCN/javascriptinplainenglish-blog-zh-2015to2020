# 创建可访问的 React 应用程序——鼠标事件和其他事情

> 原文：<https://javascript.plainenglish.io/creating-accessible-react-apps-mouse-events-and-other-things-1753783efe8c?source=collection_archive---------5----------------------->

![](img/59ebafb14a4ee21932c921748c399f3e.png)

Photo by [Ryan Stone](https://unsplash.com/@rstone_design?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建前端视图的库。它有一个庞大的图书馆生态系统与之合作。此外，我们可以用它来增强现有的应用程序。

在本文中，我们将看看如何创建可访问的 React 应用程序。

# 鼠标和指针事件

通过鼠标或指针事件暴露的任何功能也应该可以单独使用键盘来访问。

我们应该在按钮上使用`onBlur`和`onFocus`，如果我们用它们来打开或关闭某些东西。

例如，我们应该写:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { open: false };
  } handleClick() {
    this.setState({ open: true });
  } handleFocus() {
    this.setState({ open: true });
  } handleBlur() {
    setTimeout(() => {
      this.setState({ open: false });
    });
  } render() {
    return (
      <div
        onFocus={this.handleFocus.bind(this)}
        onBlur={this.handleBlur.bind(this)}
        aria-expanded={this.state.open}
      >
        <button onClick={this.handleClick.bind(this)}>Menu</button>
        {this.state.open && (
          <ul>
            <li>Apple</li>
            <li>Orange</li>
            <li>Mengo</li>
          </ul>
        )}
      </div>
    );
  }
}
```

在上面的代码中，我们将模糊和焦点事件处理程序附加到包装器 div。

此外，我们还有`aria-expanded`属性来指示 div 是否有扩展菜单。

当我们按 tab 键来聚焦按钮时，我们可以不用鼠标来切换菜单。这就是我们想要的，因为不是每个人都可以使用鼠标或其他定点设备。

# 更复杂的部件

我们应该加入 HTML aria 属性，这样 React 组件就可以访问我们的组件。

这对于复杂的组件更为重要，因为它们有许多部件。

# 设置语言

我们应该设置页面的语言，以便屏幕阅读器软件可以使用它来设置正确的语音设置。

# 设置文档标题

`title`文本应该正确描述当前页面，以便用户了解页面的内容。

# 色对比度

元素应该有足够的对比度，这样视力不好的人也能读懂。

我们可以通过使用[可着色](https://jxnblk.github.io/colorable/)包来计算具有足够对比度的颜色组合。

# 测试键盘

我们应该测试是否所有东西都可以只用键盘导航。

为此，我们可以按 Tab 和 Shift+Tab 分别向前和向后导航。

此外，我们应该能够使用回车键来激活元素。

箭头键应该与菜单和下拉菜单一起工作。

## eslint-plugin-jsx-a11y

ESLint 有一个插件可以检查 JSX 代码中的可访问性问题。许多 IDE 都直接与此集成。

它是一个 NPM 软件包，因此我们可以通过运行以下命令来安装它:

```
npm install eslint --save-dev
```

我们可以安装这个包，并把下面的内容放到我们项目的`.eslintrc`文件中，如下所示:

```
{
  "extends": ["react-app", "plugin:jsx-a11y/recommended"],
  "plugins": ["jsx-a11y"]
}
```

![](img/edf2a2652505f9f76f5f540652512ee8.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在浏览器中测试可访问性

我们可以通过使用自动测试可访问性的 [aXe-core](https://github.com/dequelabs/axe-core) 包来测试可访问性。

# 屏幕阅读器

屏幕阅读器应该能够正确阅读页面。常见的屏幕阅读器包括 NVDA、画外音和 JAWS。

谷歌 Chrome 也有 ChromeVox 扩展。

# 结论

让 React 应用变得可访问需要考虑很多方面。我们应该正确设置语言、标题和 aria 属性。

此外，页面上的元素应该可以通过键盘访问。

我们可以使用 ESLint 插件和自动化测试工具来测试可访问性。