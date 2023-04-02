# 排名靠前的网站数据

> 原文：<https://javascript.plainenglish.io/top-react-hooks-website-data-acb38749e2c?source=collection_archive---------9----------------------->

![](img/8f2c1633ecae72a38152c67907cab4b0.png)

Photo by [Ilya Pavlov](https://unsplash.com/@ilyapavlov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# @d2k/react-devto

@d2k/react-devto 是一个包，它为我们提供了钩子，可以从 dev.to 中获取内容。

要安装它，我们运行:

```
npm i @d2k/react-devto --save
```

然后我们可以通过编写来使用提供的钩子；

```
import React from "react";
import {
  useArticles,
  useFollowSuggestions,
  useTags,
  useUser
} from "@d2k/react-devto";
export default function App() {
  const { articles, loading, error } = useArticles(); return (
    <>
      <div>{articles.map(a => a.title)}</div>
      <div>{loading}</div>
      <div>{error}</div>
    </>
  );
}
```

它提供了获取最新文章的`useArticles`钩子。

`articles`有文章的数据。

`loading`有装载状态。

`error`如有错误。

它可以依次接受`page`、`tag`和`username`参数。

`page`是要转到的页面。

`tag`是要搜索的标签。

`username`有要找的用户名。

`useTags`挂钩有来自网站的标签。

我们可以通过书写来使用它:

```
import React from "react";
import {
  useArticles,
  useFollowSuggestions,
  useTags,
  useUser
} from "@d2k/react-devto";
export default function App() {
  const { tags, loading, error } = useTags(); return (
    <>
      <div>
        {tags.map(t => (
          <p>{t.name}</p>
        ))}
      </div>
    </>
  );
}
```

# @d2k/react-github

@d2k/react-github 是一系列让我们从 github 获取数据的 react 钩子。

要安装它，我们运行:

```
npm i @d2k/react-github --save
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useUser } from "@d2k/react-github";export default function App() {
  const { user, loading, error } = useUser("nat"); return (
    <>
      <div>{JSON.stringify(user)}</div>
    </>
  );
}
```

我们可以将用户名传递给`useUser`钩子来获取它的数据。

它包括 ID、登录名、公司等等。

`useRepos`钩子让我们获得用户拥有的回购。

例如，我们可以写:

```
import React from "react";
import { useRepos } from "@d2k/react-github";export default function App() {
  const { repos, loading, error } = useRepos("nat"); return (
    <>
      <div>{JSON.stringify(repos)}</div>
    </>
  );
}
```

我们使用`useRepos`钩子来获取`'nat'`用户的回复。

`useBranch`钩子让我们获得关于回购分支的数据。

为了使用它，我们写:

```
import React from "react";
import { useBranch } from "@d2k/react-github";export default function App() {
  const { branch, loading, error } = useBranch("facebook", "react", "master"); return (
    <>
      <div>{JSON.stringify(branch)}</div>
    </>
  );
}
```

我们按照用户名、repo 名称和分支名称的顺序获取 repo。

还有从回购的所有分支获取数据的`useBranches`钩子。

例如，我们可以写:

```
import React from "react";
import { useBranches } from "@d2k/react-github";export default function App() {
  const { branches, loading, error } = useBranches("facebook", "react"); return (
    <>
      <div>{JSON.stringify(branches)}</div>
    </>
  );
}
```

在所有示例中，`loading`和`error`具有加载和错误状态。

![](img/2c64d6495933f6c724f9cc04e47c1dab.png)

Photo by [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用一些有用的钩子从各种网站获取公共数据。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**