# Next.js 样式组件

> 原文：<https://javascript.plainenglish.io/next-js-styling-components-e282adde3ae?source=collection_archive---------11----------------------->

![](img/c6ecbeb270e1957af3e78b4e7b82dfda.png)

Photo by [Joshua Reddekopp](https://unsplash.com/@joshuaryanphoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解如何使用 Next.js 设计页面样式。

# 内置 CSS 支持

Next.js 允许我们从 JavaScript 文件导入 CSS。

它将`import`扩展到基本 JavaScript 用法之外，让我们可以导入 CSS 文件。

我们可以在项目中添加一个全局样式表。

为此，我们只需创建一个 CSS 文件并将其导入到`pages/_app.js`中。

所以我们可以写:

```
body {
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}
```

在`styles.css`。

然后在`pages/_app.js`中，我们写道:

```
import '../styles/globals.css'
import './styles.css'function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}export default MyApp
```

我们添加了`styles.css`进口。

其余的代码是在我们创建项目时生成的。

如果我们想从`node_modules`文件夹中导入，那么我们必须在`pages/_app.js`中这样做。

# 添加组件级 CSS

除了全局 CSS 之外，我们还可以添加组件级 CSS。

为此，我们遵循`[name].module.css`的命名约定，其中`[name]`是组件的文件名。

例如，假设我们有`pages/yesno.js`:

```
function YesNo({ data }) {
  return <p>{data.answer}</p>
}export async function getServerSideProps() {
  const res = await fetch(`https://yesno.wtf/api`)
  const data = await res.json()
  return { props: { data } }
}export default YesNo
```

我们可以用以下代码创建一个名为`yesno.module.js`的文件:

```
.yesno {
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}
```

然后我们可以将其导入`yesno.js`:

```
import styles from './yesno.module.css';function YesNo({ data }) {
  return <p className={styles.yesno}>{data.answer}</p>
}export async function getServerSideProps() {
  const res = await fetch(`https://yesno.wtf/api`)
  const data = await res.json()
  return { props: { data } }
}export default YesNo
```

CSS 中的`yesno`类在我们导入时被转换成`yesno`属性。

我们用它来填充`className`属性来设置元素的类名。

CSS 模块是一个可选功能，它只对扩展名为`.module.css`的文件启用。

这意味着如果我们想像以前一样导入它们，我们需要文件名的`.module`部分。

我们也可以像往常一样使用`link`标签来添加我们的 CSS 文件。

CSS 模块被自动连接、缩小和代码分割。

# Sass 支持

Next.js 让我们导入扩展名为`.scss`和`.sass`的 SASS 文件。

我们也可以将它们作为 CSS 的修改件，使用`.module.scss`和`.module.sass`扩展。

为了在我们的 Next.js 应用程序中使用 SASS，我们通过运行以下命令安装了`sass`包:

```
npm install sass
```

SCSS 语法是 CSS 语法的扩展，SASS 有一个不同于 CSS 的缩进语法。

因此，使用 SCSS 文件更容易。

# 自定义 Sass 选项

我们可以改变样式文件的路径，使其包含在`next.config.js`文件中。

例如，我们可以写:

```
const path = require('path')module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}
```

来改变我们想要包含的样式文件的路径。

# 结论

我们可以用 CSS 和 SASS / SCSS 来设计我们的 Next.js 应用程序。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**