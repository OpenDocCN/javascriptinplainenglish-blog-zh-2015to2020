# 现代 JavaScript 的精华—模板文字

> 原文：<https://javascript.plainenglish.io/best-of-modern-javascript-template-literals-5e6e34124a66?source=collection_archive---------1----------------------->

![](img/22efafdee2b6723d77cf8248a9e2daa0.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 模板文字。

# 模板文字

模板文字是一个字符串文字，我们可以将 JavaScript 表达式直接插入到字符串中。

例如，我们可以写:

```
const firstName = 'james';
console.log(`Hello ${firstName}`);
```

然后我们得到:

```
'Hello james'
```

它由反斜线而不是单引号和双引号分隔。

# 在模板文本中转义

反斜杠用于在模板文本中转义。

例如，我们可以写:

```
`\``
```

并获得:

```
'`'
```

然而，我们不能写:

```
`\${`
```

因为我们会得到一个语法错误。

除此之外，它们就像普通的字符串一样工作。

模板文本中的行终止符总是 LF。

LF 是换行符，是 Unix 和 macOS 使用的`\n`或 U+000A。

CR，也就是老 Mac OS 用的`\r`或者 U+900D。

CRLF，也就是 Windows 使用的`\r\n`。

在模板文字中，它们都被规范化为 LF。

# 标记的模板文字

带标记的模板文本是在模板文本前面放一个标记。

例如，我们可以写:

```
tagFunction`Hello ${firstName} ${lastName}`
```

其中`tagFunction`是我们在字符串上调用的函数。

这和写作是一样的:

```
tagFunction(['Hello ', ' '], firstName, lastName)
```

标签接受模板字符串和占位符的替换。

替换是在运行时完成的。

但是模板字符串在编译时是静态已知的。

标签函数还会得到未经解释的原始版本。

它还得到反斜杠特殊的最终版本。

# 原始字符串

ES6 有用于原始字符串的`String.raw`模板标签。

它返回字符串，所有反斜杠保持不变。

例如，我们可以写:

```
const str = String.raw`Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus fringilla mauris vel erat facilisis lacinia. \nVivamus tincidunt, massa sit amet fermentum rhoncus, nibh mi convallis elit, eget bibendum eros ipsum elementum purus. Donec tristique ligula mi, non dapibus quam finibus et. Orci varius natoque penatibus et magnis dis parturient montes`
```

`\n`完好无损。

每当我们需要保持字符串中的反斜杠时，这是很有用的。

我们可以使用`sh`模板标签来运行 shell 命令。

例如，我们可以写:

```
const proc = sh`ps ax | grep ${pid}`;
```

像 sh 模板标签包这样的包为我们提供了`sh`模板标签来运行带有标签的命令。

# 脸书图表

[脸书中继](https://facebook.github.io/relay/)让我们对 GraphQL API 进行查询。

例如，我们可以这样使用它:

```
import React from "react"
import { createFragmentContainer, graphql, QueryRenderer } from "react-relay"
import environment from "./lib/createRelayEnvironment"
import ArtistHeader from "./ArtistHeader"export default function ArtistRenderer({artistID}) {
  return (
    <QueryRenderer
      environment={environment}
      query={graphql`
        query QueryRenderersArtistQuery($artistID: String!) {
          # The root field for the query
          artist(id: $artistID) {
            # A reference to your fragment container
            ...ArtistHeader_artist
          }
        }
      `}
      variables={{artistID}}
      render={({error, props}) => {
        if (error) {
          return <div>{error.message}</div>;
        } else if (props) {
          return <Artist artist={props.artist} />;
        }
        return <div>Loading</div>;
      }}
    />
  );
}
```

使用来自[https://relay.dev/](https://relay.dev/)的例子。

我们只需将我们自己的模板字符串传入`graphql`标签来发出请求。

![](img/170eaa4a9cea4384242d31ae94eae846.png)

Photo by [Ashley Anderson](https://unsplash.com/@hartnashs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

模板字符串很有用。

他们让我们插值表达式，用标签调用。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**