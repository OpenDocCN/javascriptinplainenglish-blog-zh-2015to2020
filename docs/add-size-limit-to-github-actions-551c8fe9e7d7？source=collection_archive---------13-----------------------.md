# 向 GitHub 操作添加大小限制

> 原文：<https://javascript.plainenglish.io/add-size-limit-to-github-actions-551c8fe9e7d7?source=collection_archive---------13----------------------->

这篇文章讲述了如何将 JavaScript 的性能预算工具[大小限制](https://github.com/ai/size-limit)添加到 [GitHub 动作](https://b.remarkabl.org/github-actions)中。

![](img/43204db4cfabe222f675d030db81fb75.png)

# 设置

安装`[size-limit](https://www.npmjs.com/package/size-limit)`:

```
$ npm install --save-dev size-limit
```

运行`size-limit`二进制文件以获取消息:

```
$ npx size-limit
Install Size Limit preset depends on type of the projectFor application, where you send JS bundle directly to users
  npm install --save-dev @size-limit/preset-appFor frameworks, components and big libraries
  npm install --save-dev @size-limit/preset-big-libFor small (< 10 KB) libraries
  npm install --save-dev @size-limit/preset-small-libCheck out docs for more complicated cases
  [https://github.com/ai/size-limit/](https://github.com/ai/size-limit/)
```

在我们的例子中，我们将安装大库预置`[@size-limit/preset-big-lib](https://www.npmjs.com/package/@size-limit/preset-big-lib)`:

```
$ npm install --save-dev @size-limit/preset-big-lib
```

再次运行`size-limit`得到错误:

```
$ npx size-limit
 ERROR  Create Size Limit config in package.json "size-limit": [
    {
      "path": "dist/bundle.js",
      "limit": "10 KB"
    }
  ]
```

# 配置

[配置](https://github.com/ai/size-limit#config)选项有:

*   `path` : **路径**到你的捆绑
*   `limit` : **您捆绑的大小**(如`42 B`、`1337 KB`)或**时限**(如`314 ms`、`1.618 s`)

## 例子

如果您的包路径是`build/my-bundle.js`并且您不希望您的包超过`9000`字节，那么将以下内容添加到您的`package.json`中:

# 成功

`size-limit`的成功消息如下所示:

```
$ npx size-limit
✔ Adding to empty webpack project
✔ Running JS in headless Chrome Size limit:   8.79 KB
  Size:         8.69 KB with all dependencies, minified and gzipped
  Loading time: 50 ms   on slow 3G
  Running time: 60 ms   on Snapdragon 410
  Total time:   110 ms
```

# 失败

`size-limit`的失败消息如下所示:

```
$ npx size-limit
✔ Adding to empty webpack project
✔ Running JS in headless Chrome Package size limit has exceeded by 1 KB
  Size limit:   8.79 KB
  Size:         9.79 KB with all dependencies, minified and gzipped
  Loading time: 50 ms   on slow 3G
  Running time: 70 ms   on Snapdragon 410
  Total time:   120 ms Try to reduce size or increase limit at .size-limit.json
```

# GitHub 操作

创建使用[尺寸限制动作](https://github.com/marketplace/actions/size-limit-action)的`.github/workflows/size-limit.yml`:

添加工作流的初始 PR (pull request)将会失败，因为该操作试图与没有添加`size-limit`的基本分支进行比较(参见 [#33](https://github.com/andresz1/size-limit-action/issues/33) )。

一旦`size-limit`打开`master`，所有新创建的 PR 将测量束尺寸/性能。

参见[添加该工作流程的示例 PR](https://github.com/remarkablemark/html-react-parser/pull/197) 。

[*本文原载于 2020 年 12 月 20 日《remarkablemark.org》。*](https://b.remarkabl.org/2Kv374v)