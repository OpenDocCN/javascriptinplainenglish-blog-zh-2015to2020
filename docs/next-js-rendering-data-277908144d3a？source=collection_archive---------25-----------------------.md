# Next.js 渲染数据

> 原文：<https://javascript.plainenglish.io/next-js-rendering-data-277908144d3a?source=collection_archive---------25----------------------->

![](img/7c4b712014a9382ee3bde37b0b9ce988.png)

Photo by [Kumpan Electric](https://unsplash.com/@kumpan_electric?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以轻松地创建服务器端呈现的 React 应用程序和静态站点。

在本文中，我们将了解如何使用 Next.js 呈现数据

# 静态渲染

我们可以创建带有动态路线的页面。

为此，我们向组件文件添加了一个`getStaticPaths`函数，并将其导出。

此外，我们必须在文件名周围添加括号，使其具有动态性。

例如，我们创建一个`country/[name].js`文件来添加动态路线，以获取国家数据，并用 Next.js 显示它。

在`country/[name].js`中，我们写道:

```
function Country({ country }) {
  return <p>{country.name}</p>
}export async function getStaticPaths() {
  const res = await fetch('https://restcountries.eu/rest/v2/all')
  const countries = await res.json()
  const paths = countries.map((country) => `/country/${country.name.toLowerCase()}`)
  return { paths, fallback: false }
}export async function getStaticProps({ params }) {
  const res = await fetch(`https://restcountries.eu/rest/v2/name/${params.name}`)
  const [country] = await res.json()
  return { props: { country } }
}export default Country
```

我们有`getStaticPaths`方法来返回我们想要预渲染的所有路径。

我们所要做的就是从 REST countries API 中获取数据，并创建一个我们想要呈现的路径数组。

`fallback: false`表示没有渲染的其他路线应该返回 404。

此外，我们还有`getStaticProps`函数来获取各个预渲染路线的数据。

`params`对象有我们传入的 URL 参数。

在`Country`组件中，我们呈现来自`getStaticProps`的结果。

现在，当我们转到类似于[http://localhost:3000/country/Canada](http://localhost:3000/country/canada)的 URL 时，我们应该会看到显示的国家名称。

这将静态地预呈现页面，并将它们用于所有请求。

静态生成适用于变化不大的页面，如营销页面、博客文章、产品列表和文档。

# 服务器端渲染

使用 Next.js 显示呈现内容的另一种方法是使用服务器端呈现。

为此，我们在页面中添加了`getServerSideProps`函数。

例如，我们可以创建一个`pages/yesno.js`文件并编写:

```
function YesNo({ data }) {
  return <p>{data.answer}</p>
}export async function getServerSideProps() {
  const res = await fetch(`https://yesno.wtf/api`)
  const data = await res.json()
  return { props: { data } }
}export default YesNo
```

我们有`getServerSideProps`函数来获取数据。

然后我们获取从`YesNo`组件的道具中获取的数据，并呈现它。

# 结论

使用 Next.js，我们可以用两种不同的方式呈现数据。

一种方法是静态预渲染路线。这样更快，也值得推荐

另一种方法是使用服务器端渲染来渲染路线。

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**