# JavaScript 中可选的链接操作符

> 原文：<https://javascript.plainenglish.io/the-optional-chaining-operator-in-javascript-e151ecafcbd8?source=collection_archive---------6----------------------->

![](img/79de34dccdff68e196a84610bedb143a.png)

# 定义

可选的链接操作符`?.`允许读取位于连接对象链深处的属性值。

*可选链接作为 ES2020 标准的一部分引入。*

# 为什么会这样？

它改变了我们从深层对象获取属性的方式。可选的链接使你的代码看起来更整洁。

考虑这段代码，其中数据对象有一个查询和对查询的回答。

要访问`value`，您必须编写一个很长的条件语句，很难阅读和格式化😢

但是通过可选的链接，您可以轻松访问`value`😃你也不必担心空的&未定义的检查。

```
response?.data?.answer?.value*// Output*JavaScript is 💛
```

哇，这段代码看起来如此干净🧼和干脆！如果`value`不存在，我们可以给它分配一个默认值。

```
response?.data?.answer?.key || 'JavaScript is BAE 💛'*// Output*JavaScript is BAE 💛
```

# 设置 Babel 插件

Babel [7.8.0](https://babeljs.io/blog/2020/01/11/7.8.0) 默认支持新的`ECMAScript 2020`特性。不需要为可选链接启用单独的插件(`?.`)。

如果你使用的是最新的 Babel 版本，高于或等于 7.8.0，那么这是一个简单的设置

```
npm install --save-dev @babel/cli @babel/core @babel/preset-env
```

现在将以下配置添加到`.babelrc`

```
{ 
  "presets": [ "@babel/preset-env" ] 
}
```

必要的巴别塔模块和预设配置完成。现在是时候做巴别塔魔法✨了

运行此命令将代码转换到任何地方支持的版本。如果您已经全局安装了`babel`模块，此命令将会起作用。

```
babel app.js --out-file script-transpiled.js
```

所有可选链接代码应放在`app.js`中，然后执行上述命令。这产生了可以在主流浏览器和`node.js`中工作的 transpiled 代码。

# 1.函数调用的可选链接

当您试图调用一个可能不存在的方法时，可以使用可选链接。在函数调用中使用可选链接会导致表达式自动返回 undefined，而不是引发异常。

# 2.使用表达式的可选链接

如果左操作数为空或未定义，则不计算可选链接运算符后的表达式。

在执行第 3 行时，用户被定义为 null，`isTeenage`没有抛出任何错误，因为如果左操作数为 null 或未定义，表达式将不会被求值。

# 3.结合 nullish 合并运算符[这是 ES2020 的另一项功能]

```
let user = null;
let age = 12;
let isTeenage = user?.[value++] ?? 'not a teenager!';console.log('isTeenage :: ', isTeenage);// OutputisTeenage :: not a teenager!
```

# 关于可选链接的事情

*   干净易读的代码
*   不用担心对象中的`null`或`undefined`
*   误差更小

# 浏览器支持

*   铬合金— 80
*   边缘— 80
*   火狐浏览器— 74
*   Internet Explorer —否
*   歌剧— 67
*   Node.js — 14.0.0

# 参考

*   [MDN Web](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

![](img/d508e30dfe0dd2d6a9c89ceaf26face3.png)

Please follow [https://www.instagram.com/javascriptessentials](https://www.instagram.com/javascriptessentials/) on Instagram to learn JavaScript 💛 through a snapshot of code snippets